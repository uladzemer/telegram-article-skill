# Telegram Article Skill

An agent skill for writing posts that use Telegram's built-in **Rich Text Editor**
(paperclip 📎 → "Article", Telegram 12.9+) — the one with headings, tables, lists and
collapsible blocks.

Works with **Claude Code**, **Codex**, and other agents that read `SKILL.md` files.

[Русская версия ниже ↓](#telegram-article-skill-ru)

---

## The problem

The Article editor is WYSIWYG. Paste HTML source into it and you get literal `<h1>` text
on screen. Every agent that helpfully hands you an HTML file to "paste into Telegram"
produces exactly that failure.

## The insight

**The editor accepts rich clipboard content.** Copy a *rendered* page from a browser —
not its source — and the formatting transfers: headings, lists, bold, links, and tables.

Telegram documents this nowhere. Published guides state that tables cannot be pasted.
We verified otherwise on Telegram Desktop (Windows, Premium) on 2026-09-18.

So the skill's deliverable is not "an HTML file". It is **a rendered page with a copy button**
that writes both `text/html` and `text/plain` to the clipboard.

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

`SKILL.md` covers the editorial side too — what belongs in a table versus a list, where
blocking requirements go, why UI labels must be verified rather than recalled.

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

> Write a Telegram announcement about the new pricing, with a table of plans

> Сделай пост в телеграм про запуск, в режиме статьи

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

**Bot API** (`parse_mode=HTML`) — programmatic sending, ~11 inline tags, no headings or
tables. **Telegraph** — no Premium needed and gets Instant View, but only `h3`/`h4`
headings and no tables. Both are covered briefly in `references/editor-capabilities.md`.

## License

MIT — see [LICENSE](LICENSE).

---

<a name="telegram-article-skill-ru"></a>

# Telegram Article Skill (рус.)

Навык для агентов, который пишет посты под встроенный **редактор статей Telegram**
(скрепка 📎 → «Статья», Telegram 12.9+) — тот, где есть заголовки, таблицы, списки
и сворачиваемые блоки.

Работает с **Claude Code**, **Codex** и другими агентами, читающими файлы `SKILL.md`.

## Проблема

Редактор статей — визуальный. Вставленный в него HTML-код превращается в текст:
на экране видно `<h1>`. Любой агент, который выдаёт «HTML-файл, вставьте в телеграм»,
приводит ровно к этому.

## Решение

**Редактор принимает оформление из буфера обмена.** Нужно копировать не исходник,
а *отрендеренную* страницу из браузера — тогда переносятся заголовки, списки, жирный,
ссылки и таблицы.

Telegram нигде это не описывает, а опубликованные гайды прямо утверждают, что таблицы
вставить нельзя. Проверено обратное: Telegram Desktop, Windows, Premium, 18.09.2026.

Поэтому результат работы навыка — не «HTML-файл», а **отрендеренная страница с кнопкой
копирования**, которая кладёт в буфер сразу два слоя: `text/html` и `text/plain`.

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

Навык покрывает и редакторскую часть: что уместно таблицей, а что списком; куда ставить
блокирующие требования; почему названия кнопок нужно сверять с продуктом, а не вспоминать.

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

> Сделай пост в телеграм про запуск, в режиме статьи

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

**Bot API** (`parse_mode=HTML`) — программная отправка, около 11 строчных тегов,
без заголовков и таблиц. **Telegraph** — Premium не нужен и даётся Instant View,
но заголовки только `h3`/`h4` и таблиц нет вовсе. Оба кратко разобраны
в `references/editor-capabilities.md`.

## Лицензия

MIT — см. [LICENSE](LICENSE).
