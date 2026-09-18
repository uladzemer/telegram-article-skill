# Telegram Rich Text Editor — capabilities reference

Shipped **14 July 2026** in Telegram 12.9 (Android/iOS) and Telegram Desktop 6.9.4.
Opened via **paperclip 📎 → "Article"**; on phones there is also a quick button above
the send key.

- **Publishing requires Telegram Premium.** Without it the editor opens but will not send.
  (One source reports the gate is partly *inside* the editor — headings, advanced
  formatting and carousels unavailable on free accounts — while others describe only the
  send button being blocked. Unresolved; check on a current build.)
- **Readers do not need Premium.** Viewing support landed in 12.8. How the message
  degrades on clients older than 12.8 is **not documented anywhere** — unverified.
- **Message limit: 32 768 characters** (8× a normal message).

## Typography: structural, not visual

This is the most common misunderstanding, so state it plainly to users:

**There is no font picker, no font size control and no text color.** You choose an
element's *role*; the client and the reader's theme decide how it looks.

| Available | Not available |
|---|---|
| 6 heading levels (H1–H6) — the only size control | Arbitrary font size / point size |
| Bold, italic, underline, strikethrough | Font family selection |
| Highlight (colored outline) | Free text color palette |
| Superscript, subscript | Paragraph alignment (confirmed only *inside tables*) |
| Monospace and code blocks with syntax highlighting | |
| Spoiler (hidden text) | |
| Premium custom emoji | |

When a user asks for "bigger text" or "a nicer font", the honest answer is: pick a heading
level, and use highlight or bold for emphasis. Nothing else exists.

## Structural elements

| Element | Behavior |
|---|---|
| **Quote** | Standard block, full width |
| **Pull-quote** | Centered, sized to its text — an accent for a key phrase, not a citation |
| **Collapsible block** | Text hidden behind a heading; reader clicks to expand. Implemented as a variant of the quote ("collapsible quote"). Good for long caveats, spoilers, secondary detail. |
| **Footer / footnote** | A note line at the end of the article — sources, disclaimers, small print. Whether it supports numbered anchors tied to positions in the text is **not found**; descriptions suggest a single trailing line. |
| **Divider** | Horizontal rule |
| **Lists** | Bulleted, numbered, and checklist |
| **Table** | See below |

### Checklists — open question

The editor offers a checklist list style. Whether a **reader** can tick its boxes is
**unconfirmed**: guides describe clicking a box *while authoring*. Telegram separately
ships **native checklists** (a distinct feature, sent from the attachment menu) which
*are* interactive and track completion per user — with an `others_can_complete` option.

Do not promise reader-interactive checkboxes inside an article. If interaction matters,
send a native checklist as its own message.

### Tables

Real tables: add/remove rows and columns, borders on/off, alternating row fill,
per-column text alignment, adjustable column widths.

**Cells hold text and emoji only** — no images, video or stickers inside a cell.

## Media

- Inserted **inside** the editor: photo icon on desktop, paperclip on mobile. Text flows
  before and after; media sits **between paragraphs** as a block.
- **Photos, video, audio, documents and location** are supported. Arbitrary **files**
  were added 25 August 2026.
- **Captions per media item** — a separate translucent caption the editor offers to fill
  automatically.
- **Carousel / collage:** add the first photo, then use the **+** in its top-right corner
  to add more; the **tiles button** next to the + converts the set into a carousel, and
  clicking again returns it to a grid/collage. Readers swipe the carousel.
- **Text wrap, image alignment and explicit sizing are not found** in any source. Media
  appears to be a block element with no such controls — but no source states this
  outright. Treat as unconfirmed.
- **Whether images survive a clipboard paste is not documented.** Plan on adding media by
  hand in the editor and marking intended positions in your draft.

## Formulas

- **LaTeX syntax.** Covers mathematical, physical and chemical notation.
- **Inline or block** — toggled by a checkbox in the formula's settings.
- You type LaTeX source; the editor renders it.
- The AI assistant can generate formulas from a prompt.
- **Not found:** which panel icon inserts one, the supported LaTeX subset, the rendering
  engine (KaTeX vs MathJax), length limits, and whether `$…$` delimiters apply — the
  formula is inserted as an element through the UI rather than parsed out of text.

Deliver formulas as copyable LaTeX source for the user to paste into the formula element,
never as an image.

## The clipboard mechanism

The editor accepts **rich clipboard content**. A browser copying rendered content places
two flavors on the clipboard — `text/html` and `text/plain` — and the editor reads the
HTML flavor, mapping it onto its own elements.

**Verified 2026-09-18** (Telegram Desktop, Windows 11, Premium): a page with `h1`, `h2`,
`p`, `ul`, `ol`, `table` and `b` was copied via `navigator.clipboard.write()` with a
`text/html` flavor and pasted into the Article editor. **All of it transferred, including
both tables.**

### This contradicts published guidance

Russian guides (T—J, Sports.ru, ppc.world) state that tables cannot be imported from
external applications and must be rebuilt by hand. Telegram documents paste behavior
**nowhere** — not in the launch post, not in help.

Treat the verified result as the working assumption, but have users **test one paragraph
first**. Behavior may differ across Desktop / mobile / web and may change between releases.
If a table does not survive on a given client, it is rebuilt with the ▦ button; the rest of
the formatting still transfers.

### Implementation

```js
const item = new ClipboardItem({
  'text/html':  new Blob([html],  { type: 'text/html' }),
  'text/plain': new Blob([plain], { type: 'text/plain' })
});
await navigator.clipboard.write([item]);
```

- The Article editor consumes `text/html`; a normal message field consumes `text/plain`
  (it discards nearly all formatting, so a purpose-built plain variant reads better than
  whatever the browser would flatten).
- `ClipboardItem` requires a **secure context** (`https:` or `localhost`). Keep an
  `execCommand('copy')` fallback that selects a rendered clone.
- The source block must be **rendered in the document**. `display:none` and `<template>`
  contents are not selectable — use `position:absolute; left:-99999px; width:680px`.
- Give the off-screen block an explicit width; some clients derive table column
  proportions from the rendered layout.

### Markup that survives

```
h1 h2 h3  p  b i u s  a  ul ol li  table tr th td  blockquote  hr  code
```

No `class`, `style`, `div` or `span`. They are stripped, and a wrapper can prevent the
content inside it from being recognized as a block.

## Pitfalls

- 🔴 **No autosave.** Close the editor before sending and the text is gone. This is the
  reason to write elsewhere and paste in.
- 🔴 **Tables reportedly do not paste** from Excel/Sheets/external apps (our test
  contradicts this for browser-rendered HTML — see above).
- **No collaborative editing or commenting.**
- **AI output needs checking** — accuracy is not guaranteed.
- Image-upload bugs were reported in late June 2026, mostly fixed in later builds.
- **Bots reading such messages see empty `msg.text`** — relevant if a bot consumes
  channel posts. Referenced in community write-ups; not verified here.
- **Not found:** feature differences between mobile and desktop (only UI placement
  differs), how an article looks when forwarded or in web.telegram.org, and whether a
  published article can be edited afterwards.

## Interactivity: what an author cannot do

Clicking parts of a message and having them act as buttons **is real**, but it comes from
**Bot API Rich Messages**, not the editor:

- **10.1** (11 June 2026) — rich structure for bots: paragraphs, headings, anchors, lists,
  tables, collapsible details, cards, media blocks
- **10.2** (14 July 2026) — blocks, collages, slideshows, formulas, dividers
- **10.3** (24 August 2026) — **`RichTextButton`** (a button *inside* a paragraph),
  **`RichBlockButtons`** (a button row between blocks), collapsible quotes, disabled buttons

`RichMessageButton` carries the full inline-button action set: `callback_data`, `url`
(including `tg://`), `web_app`, `login_url`, `switch_inline_query`, `copy_text`, plus
`style` — `danger` / `success` / `primary` / **`link`** (borderless, reads as text in the
flow). That last style is what makes a message look like clickable prose.

Apps such as Telegram's chess client work this way: a bot posts a rich message and
**edits it in place** on each callback, redrawing the board. The "edited" timestamp on such
messages is the giveaway.

**What an article author can do without a bot:**

1. **Deep links as ordinary hyperlinks** — `t.me/<bot>?start=<param>`,
   `t.me/<bot>?startapp=<param>` (launches a Mini App), `tg://resolve?domain=…`,
   `tg://settings/…`. Formally a link; in effect a button. This covers most "clicking does
   something" needs.
2. **Collapsible blocks** — the reader expands them.
3. **A native checklist** as a separate message alongside the article.

**What requires a bot:** buttons inside text, button rows between blocks, `callback_data`
handling, in-place message editing, `web_app` launch buttons, polls and quizzes.

## Other Telegram publishing targets

**Bot API `parse_mode=HTML`** — roughly eleven inline tags (`b, strong, i, em, u, ins, s,
strike, code, pre, a`, plus `tg-spoiler`, `blockquote`). No headings, lists or tables.
`<br>` unsupported — use `\n`.

**Telegraph** (telegra.ph) — standalone, no Premium, automatic Instant View. Allowed tags:
`a, aside, b, blockquote, br, code, em, figcaption, figure, h3, h4, hr, i, iframe, img, li,
ol, p, pre, s, strong, u, ul, video`; only `href` and `src` attributes. **Headings limited
to h3/h4, no tables**, but `iframe` allows embeds. Its API takes a Node array, not HTML;
libraries such as `html-telegraph-poster` convert.

**Instant View** — renders *external* sites inside Telegram via per-domain templates
written at instantview.telegram.org and moderated by Telegram.

## Sources

- [Telegram Blog — Rich Text Editor](https://telegram.org/blog/communities-editor-invisible-messages)
- [Telegram Blog — Rich Text for Bots](https://telegram.org/blog/watch-apps-and-more)
- [Telegram Blog — Checklists](https://telegram.org/blog/checklists-suggested-posts)
- [Bot API changelog](https://core.telegram.org/bots/api-changelog)
- [Deep links](https://core.telegram.org/api/links)
- [Telegraph API](https://telegra.ph/api)
- [T—J guide (RU)](https://t-j.ru/telegram-editor-guide/)
- [Sports.ru guide (RU)](https://cyber.sports.ru/tech/blogs/3421529.html)
- [ppc.world breakdown (RU)](https://ppc.world/articles/novyy-tekstovyy-redaktor-telegram-polnyy-razbor-vozmozhnostey/)
- Clipboard behavior: direct verification 2026-09-18, not from any published source.
