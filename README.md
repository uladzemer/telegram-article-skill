# Telegram Article Skill

An agent skill for writing well-formatted articles in Telegram's built-in
**Rich Text Editor** (paperclip 📎 → "Article", Telegram 12.9+) — headings, tables, lists,
quotes, collapsible blocks, images, carousels and LaTeX formulas.

Works with **Claude Code**, **Codex**, and other agents that read `SKILL.md` files.

[Русская версия ниже ↓](#telegram-article-skill-ru)

---

## Two problems it solves

**Formatting in Telegram is structural, not visual.** There is no font picker, no font
size, no text color. You pick an element's *role* — heading level, quote, callout, code,
highlight — and the client renders it. Agents that promise "a nicer font" are promising
something that does not exist.

**The editor is WYSIWYG.** Paste HTML source into it and you get literal `<h1>` text on
screen. Every agent that hands you an HTML file to "paste into Telegram" produces exactly
that failure.

## The insight

**The editor accepts rich clipboard content.** Copy a *rendered* page from a browser —
not its source — and the formatting transfers: headings, lists, bold, links, and tables.

Telegram documents this nowhere. Published guides state that tables cannot be pasted.
We verified otherwise on Telegram Desktop (Windows, Premium) on 2026-09-18.

So the skill's deliverable is not "an HTML file". It is **a rendered page with a copy
button** that writes both `text/html` and `text/plain` to the clipboard.

The skill also covers the editorial side — which content shape wants a table versus a
list, where blocking requirements belong, how to use carousels and captions, and how to
hand over LaTeX formulas — plus honest limits: what the editor cannot do, and which
"clickable article" effects require a bot rather than the editor.

## What's in here

```
skills/telegram-article/
├── SKILL.md                            # the skill: workflow, structure rules, anti-patterns
├── references/
│   ├── editor-capabilities.md          # what the editor supports/strips, clipboard mechanics, limits
│   └── writing-patterns.md             # structures for launches, promos, digests, changelogs
└── assets/
    └── copy-button.html                # working drop-in template
```

Writing patterns included: product launch, promo, digest, how-to guide, longread,
technical writeup with math, and changelog.

## Install

**Claude Code** — copy the skill into your skills directory:

```bash
git clone https://github.com/<you>/telegram-article-skill.git
cp -r telegram-article-skill/skills/telegram-article ~/.claude/skills/
```

Project-scoped instead: copy it into `.claude/skills/` inside the repo.

**Codex / other agents** — point the agent at `skills/telegram-article/SKILL.md`,
or paste its contents into your instructions file.

## Use

Ask in plain language:

> Write a Telegram article about the new pricing, with a table of plans

> Turn this into a Telegram longread with proper headings and a pull-quote

> Сделай статью в телеграм про запуск — с таблицей и списком шагов

The agent produces an HTML page. Open it in a browser, press **Copy with formatting**,
then in Telegram: **📎 → Article → Ctrl+V**.

## Known limits

- The Article editor requires **Telegram Premium**.
- **No autosave** — closing the editor loses the text.
- Paste behavior is undocumented by Telegram and may vary by client or change between
  releases. **Test one paragraph before pasting a long text.**
- `ClipboardItem` needs a secure context (`https:` or `localhost`); the template falls
  back to `execCommand` elsewhere.

## Not for

**Interactive messages.** Telegram apps where you tap parts of the message and it responds
— the chess client, for example — are built with **Bot API Rich Messages** (10.3, August
2026): a bot posts a message with buttons *inside* the text and edits it in place on each
tap. An article author cannot make those in the editor. What an author *can* do is use
`t.me/…` and `tg://…` deep links as ordinary hyperlinks — including `?startapp=` to launch
a Mini App — which reads and behaves like a button. Covered in
`references/editor-capabilities.md`.

**Bot API** (`parse_mode=HTML`) — programmatic sending, ~11 inline tags, no headings or
tables. **Telegraph** — no Premium needed and gets Instant View, but only `h3`/`h4`
headings and no tables. Both covered in `references/editor-capabilities.md`.

## License

MIT — see [LICENSE](LICENSE).

---

<a name="telegram-article-skill-ru"></a>

# Telegram Article Skill (рус.)

Навык для агентов, который пишет красиво оформленные статьи во встроенном
**редакторе статей Telegram** (скрепка 📎 → «Статья», Telegram 12.9+) — заголовки,
таблицы, списки, цитаты, сворачиваемые блоки, картинки, карусели и формулы LaTeX.

Работает с **Claude Code**, **Codex** и другими агентами, читающими файлы `SKILL.md`.

## Две проблемы, которые он решает

**Оформление в Telegram структурное, а не визуальное.** Там нет выбора шрифта, нет
размера кегля, нет цвета текста. Вы выбираете *роль* элемента — уровень заголовка,
цитату, выноску, код, выделение — а как это выглядит, решает клиент. Крупность задаётся
только уровнем заголовка.

**Редактор визуальный.** Вставленный в него HTML-код превращается в текст: на экране
видно `<h1>`. Любой агент, который выдаёт «HTML-файл, вставьте в телеграм», приводит
ровно к этому.

## Решение

**Редактор принимает оформление из буфера обмена.** Нужно копировать не исходник,
а *отрендеренную* страницу из браузера — тогда переносятся заголовки, списки, жирный,
ссылки и таблицы.

Telegram нигде это не описывает, а опубликованные гайды прямо утверждают, что таблицы
вставить нельзя. Проверено обратное: Telegram Desktop, Windows, Premium, 18.09.2026.

Поэтому результат работы навыка — не «HTML-файл», а **отрендеренная страница с кнопкой
копирования**, которая кладёт в буфер сразу два слоя: `text/html` и `text/plain`.

Навык покрывает и редакторскую часть: что уместно таблицей, а что списком; куда ставить
блокирующие требования; как использовать карусели и подписи к картинкам; как отдавать
формулы. И честные границы: чего редактор не умеет и какие «кликабельные статьи»
требуют бота, а не редактора.

## Состав

```
skills/telegram-article/
├── SKILL.md                            # сам навык: порядок работы, правила структуры, антипаттерны
├── references/
│   ├── editor-capabilities.md          # что редактор поддерживает и что срезает, механика буфера, лимиты
│   └── writing-patterns.md             # структуры под запуск, акцию, дайджест, changelog
└── assets/
    └── copy-button.html                # готовый рабочий шаблон
```

Структуры в комплекте: запуск продукта, акция, дайджест, инструкция, лонгрид,
технический разбор с формулами, changelog.

## Установка

**Claude Code:**

```bash
git clone https://github.com/<you>/telegram-article-skill.git
cp -r telegram-article-skill/skills/telegram-article ~/.claude/skills/
```

Либо в конкретный проект — в папку `.claude/skills/` внутри репозитория.

**Codex и другие агенты** — укажите путь к `skills/telegram-article/SKILL.md`
или вставьте его содержимое в файл инструкций.

## Использование

Просто попросите словами:

> Сделай статью в телеграм про запуск — с таблицей и списком шагов

> Переделай этот текст в лонгрид для канала, с заголовками и выноской

Агент выдаст HTML-страницу. Откройте её в браузере, нажмите **«Скопировать с оформлением»**,
затем в Telegram: **📎 → «Статья» → Ctrl+V**.

## Ограничения

- Редактор статей доступен только с **Telegram Premium**.
- **Автосохранения нет** — закрыли окно, потеряли текст.
- Поведение вставки Telegram официально не описывает, оно может отличаться на разных
  клиентах и меняться с обновлениями. **Проверяйте на одном абзаце перед длинным текстом.**
- `ClipboardItem` требует защищённого соединения (`https:` или `localhost`);
  в остальных случаях шаблон переключается на `execCommand`.

## Не для этого

**Интерактивные сообщения.** Приложения в Telegram, где нажимаешь на части сообщения и
оно отвечает (например, шахматы), сделаны на **Bot API Rich Messages** (10.3, август
2026): бот публикует сообщение с кнопками *внутри* текста и на каждое нажатие
перередактирует его на месте. Автор статьи такого в редакторе не сделает. Что автору
доступно — ссылки `t.me/…` и `tg://…` как обычные гиперссылки, включая `?startapp=` для
запуска Mini App: выглядит и работает как кнопка. Разобрано в
`references/editor-capabilities.md`.

**Bot API** (`parse_mode=HTML`) — программная отправка, около 11 строчных тегов,
без заголовков и таблиц. **Telegraph** — Premium не нужен и даётся Instant View,
но заголовки только `h3`/`h4` и таблиц нет вовсе. Оба кратко разобраны
в `references/editor-capabilities.md`.

## Лицензия

MIT — см. [LICENSE](LICENSE).
