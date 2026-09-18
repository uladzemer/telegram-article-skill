# Telegram Rich Text Editor — capabilities and clipboard behavior

Shipped **14 July 2026** in Telegram 12.9. Opened via **paperclip 📎 → "Article"**.
Requires **Telegram Premium** to publish.

## What it supports

| Feature | Detail |
|---|---|
| Headings | 6 levels (H1–H6) |
| Bold, italic, underline, strikethrough | Yes |
| Monospace, code blocks | Yes |
| Links | Inline, attached to selected text |
| Quotes | Regular (collapsible) and centered pull-quote |
| Spoiler | Hidden text |
| Superscript, subscript, highlight | Yes |
| Lists | Bulleted, numbered, **checklist** |
| Tables | Yes — real tables, see below |
| Dividers | Horizontal rules |
| Collapsible blocks, footnotes | Yes |
| Media inside text | Images, video, audio, carousels |
| Formulas | LaTeX-like |
| Emoji | Yes, including inside table cells |

### Tables

Real tables, not ASCII imitation. Configurable: add/remove rows and columns,
borders on/off, alternating row fill, text alignment.

Cells hold **text and emoji only** — images cannot go inside a cell.

## What it does NOT support

- **Pasted HTML source.** Typing or pasting `<h1>Title</h1>` as text yields the literal
  string `<h1>Title</h1>`. The editor is WYSIWYG; it has no markup input mode.
  This is the single most common user failure.
- **Background colors, custom fonts, arbitrary CSS.** The editor imposes its own typography.
  Nothing from a stylesheet survives.
- **No autosave.** Closing the editor before sending loses the text.

## The clipboard mechanism (the important part)

The editor accepts **rich clipboard content**. When a browser copies rendered content,
it places two representations on the clipboard: `text/html` and `text/plain`.
The editor reads the `text/html` flavor and maps it onto its own structural elements.

**Verified 2026-09-18** (Telegram Desktop, Windows 11, Telegram Premium):
a page containing `h1`, `h2`, `p`, `ul`, `ol`, `table` and `b` was copied via
`navigator.clipboard.write()` with a `text/html` flavor and pasted into the Article editor.
**All of it transferred, including both tables.**

### This contradicts published guidance

Russian guides (T—J, Sports.ru) state that a ready-made table cannot be pasted and must be
rebuilt by hand. That was not our observation. Telegram's own documentation describes
paste behavior **nowhere** — neither in the launch blog post nor in the help pages.

Treat the verified result as the working assumption, but **tell users to test one paragraph
first**: behavior may differ by platform (Desktop vs mobile vs web) and may change between
releases. If tables do not survive on a given client, they are rebuilt with the ▦ button —
the rest of the formatting still transfers.

### Implementation notes

Writing both MIME types covers both destinations:

```js
const item = new ClipboardItem({
  'text/html':  new Blob([html],  { type: 'text/html' }),
  'text/plain': new Blob([plain], { type: 'text/plain' })
});
await navigator.clipboard.write([item]);
```

- The Article editor consumes `text/html`.
- A normal message field consumes `text/plain` (it discards almost all formatting —
  headings, tables and nested lists are lost, so a purpose-built plain variant reads better
  than whatever the browser would flatten).
- `ClipboardItem` requires a **secure context** (`https:` or `localhost`). Keep an
  `execCommand('copy')` fallback that selects a rendered clone.
- The source block must be **rendered in the document**. `display:none` and `<template>`
  contents are not selectable; use off-screen positioning instead:
  `position:absolute; left:-99999px; width:680px`.
- Give the off-screen block an explicit width — some clients derive table column
  proportions from the rendered layout.

## Markup that survives

Keep the hidden block minimal and semantic:

```
h1 h2 h3  p  b  i  u  s  a  ul ol li  table tr th td  blockquote  hr  code
```

Do not add `class`, `style`, `div` or `span` wrappers. They are stripped, and in some
cases a wrapper prevents the content inside it from being recognized as a block.

## Limits

| What | Limit |
|---|---|
| Normal message | 4 096 characters |
| Article-editor message | **32 768** characters |
| Media caption, free | 1 024 characters |
| Media caption, Premium | 2 048 characters |

## Other Telegram publishing targets

**Bot API** (`parse_mode=HTML`) — for programmatic sending. Supports roughly eleven inline
tags: `b, strong, i, em, u, ins, s, strike, code, pre, a`, plus `tg-spoiler` and `blockquote`.
**No headings, lists or tables.** `<br>` is not supported — use `\n`.

**Telegraph** (telegra.ph) — standalone publishing tool, no Premium required, gets Instant
View automatically. Allowed tags: `a, aside, b, blockquote, br, code, em, figcaption, figure,
h3, h4, hr, i, iframe, img, li, ol, p, pre, s, strong, u, ul, video`. Only `href` and `src`
attributes. **Headings limited to h3/h4, no tables at all** — but `iframe` allows embedding
video. Its API takes a Node array, not an HTML string; libraries such as
`html-telegraph-poster` convert HTML into that structure.

**Instant View** — renders *external* websites inside Telegram via per-domain templates
written at instantview.telegram.org and moderated by Telegram. A separate project, not a button.

## Sources

- [Telegram Blog — Rich Text Editor](https://telegram.org/blog/communities-editor-invisible-messages)
- [Telegraph API](https://telegra.ph/api)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Instant View Manual](https://instantview.telegram.org/docs)
- [T—J guide (RU)](https://t-j.ru/telegram-editor-guide/)
- Clipboard behavior: direct verification, 2026-09-18, not from any published source.
