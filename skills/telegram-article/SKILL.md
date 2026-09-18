---
name: telegram-article
description: Write and format articles for Telegram's built-in Rich Text Editor (paperclip → "Article", Telegram 12.9+) — headings, tables, lists, quotes, collapsible blocks, images, carousels and LaTeX formulas. Use when the user asks to write a Telegram article, post, channel publication, announcement, digest, guide, longread or changelog — or mentions "режим статьи", "редактор статей", "Telegram article", "пост в телеграм", "статья в телеграм". The editor does NOT accept pasted HTML source; this skill produces a rendered page whose formatting transfers through the clipboard. Not for Bot API sending (parse_mode, Rich Messages) — that is a different target.
---

# Telegram Article Editor

Telegram's Rich Text Editor (shipped 14 July 2026 in Telegram 12.9) is a **WYSIWYG** editor
reached via **paperclip 📎 → "Article"**. Publishing requires **Telegram Premium**;
readers need only 12.8+.

Two things this skill exists to get right:

1. **Formatting here is structural, not visual.** There is no font picker, no font size,
   no text color. You choose an element's *role* — heading level, quote, callout, code,
   highlight — and the client renders it. Asking for "bigger text" means picking a heading level.
2. **The editor does not accept pasted HTML source.** Paste `<h1>Title</h1>` and you get
   that string on screen. It *does* accept rich clipboard content: copying a **rendered**
   page carries the structure across.

## The rule that matters

> **Never hand the user HTML source to paste into Telegram.**
> Produce a rendered page with a copy button. The clipboard carries the formatting.

Verified 2026-09-18 on Telegram Desktop (Windows, Premium): headings, lists, bold, links
**and tables** transferred from a browser paste. Published guides claim tables cannot be
pasted, and Telegram documents paste behavior nowhere — so tell users to test one paragraph
before committing a long text, and mention the ▦ button as the fallback for tables.

## What the editor gives you

| Element | Use it for |
|---|---|
| Headings, 6 levels | Section structure. This is your only size control. |
| Bold / italic / underline / strikethrough | Emphasis inside a paragraph |
| Highlight (colored outline) | The one phrase that must not be missed |
| Superscript / subscript | Footnote marks, units, math |
| Monospace, code blocks | Commands, config, identifiers — with syntax highlighting |
| Spoiler | Hidden text the reader reveals |
| Quote | Sourced passage, full width |
| Pull-quote (centered) | Accent phrase, sized to its text |
| Collapsible block | Long caveats, spoilers, secondary detail behind a heading |
| Footer / footnote | Sources, disclaimers, small print at the end |
| Bulleted / numbered list | Facts / ordered steps |
| Checklist | Requirements to tick off |
| Table | Comparable rows — 3+ rows to earn its place |
| Divider | Hard break between topics |
| Images, video, audio, files, location | Media between paragraphs |
| Carousel / collage | A set of images as one swipeable or tiled block |
| LaTeX formula | Math, physics, chemistry — inline or as a block |

Full detail, including what is unconfirmed, is in `references/editor-capabilities.md`.

## Workflow

### 1. Establish the facts before writing

- **Verify UI labels, product names and paths against the source** — grep the codebase or
  ask. Never reproduce them from an older draft; they drift, and a wrong button name in a
  published article becomes a support ticket.
- **Verify numbers, dates, terms and limits.** If a fact cannot be verified, ask.
- **Ask what the reader is meant to do** after reading. An article with no intended action
  is a digest; one with an action needs its steps to survive skimming.

### 2. Choose structure by content shape

- Sequence of actions → numbered list, never prose.
- Uniform rows with a shared second column → table.
- Non-sequential facts → bulleted list.
- Long caveat that breaks the flow → collapsible block.
- A phrase that carries the whole piece → pull-quote.
- Math → formula element, not an image of one.
- Several images that belong together → carousel, not a stack.

Rules of thumb:

- Lead with the hook, then substance, then mechanics, then caveats.
- Paragraphs of 2–3 sentences. This is read on a phone.
- **Blocking requirements go above the steps**, never in a footnote. Readers who fail
  silently do not write to support — they leave.
- One bold phrase per paragraph. More and nothing stands out.
- Headings are a table of contents: they should read as an outline on their own.

### 3. Produce the deliverable

Write **one HTML file** — a rendered page containing:

1. The article, styled for reading in a browser.
2. A **hidden block** with the same content in editor-compatible markup
   (`h1`–`h3`, `p`, `b`, `i`, `a`, `ul`, `ol`, `table`, `blockquote`, `code`, `hr` — no
   classes, no styles, no wrappers).
3. A **copy button** writing both `text/html` and `text/plain` to the clipboard.
4. Paste instructions and the Premium/autosave caveats.

Copy `assets/copy-button.html`. Key points:

- Use `ClipboardItem` with **both** MIME types — the editor takes `text/html`,
  a normal message field takes `text/plain`.
- Keep an `execCommand('copy')` fallback over a selected clone; `ClipboardItem` needs a
  secure context.
- The hidden block must be **rendered** — off-screen (`position:absolute;left:-99999px`),
  not `display:none` and not a `<template>`, or it cannot be selected.
- Give it an explicit width; some clients derive table proportions from layout.
- Build the plain-text variant by walking the DOM: lists become `• ` / `1. ` lines,
  table rows collapse to `cell — cell`.

### 4. Hand over media and formulas as instructions

Images, carousels and formulas **are not part of the paste** — they are added in the editor.
So when the article needs them, say where and how, in the deliverable:

- Mark each intended image position in the page ("here: screenshot of the login screen").
- For a carousel: add the first image, use the **+** on it for the rest, then the tiles
  button to switch between carousel and collage.
- For a formula: give the LaTeX source in a copyable block so the user pastes it into the
  formula element.
- Note that tables take text and emoji only — no images inside cells.

### 5. State the limits honestly

In the deliverable itself:

- Publishing requires **Telegram Premium**.
- **No autosave** — closing the editor loses everything. This is why the text is written
  outside and pasted in, not typed there.
- Test the paste on one paragraph first.
- Article messages hold **32 768** characters (8× a normal message).

## Anti-patterns

| Don't | Why |
|---|---|
| Hand over HTML source to paste | Tags appear literally. The failure everyone hits. |
| Promise font choices, sizes or colors | They do not exist. Size = heading level. |
| Style the hidden block with CSS | Background, fonts and colors are stripped. |
| Use `div`/`span` wrappers in the hidden block | Stripped, and can stop content being recognized as a block |
| Reproduce UI paths from an old draft | Labels drift. Verify against the product. |
| Bury platform requirements at the bottom | Readers fail silently and churn. |
| Put an image of a formula | The editor renders LaTeX natively |
| Write one long block of prose | Unreadable on a phone, where it will be read |
| Target Bot API `parse_mode` | Different product: inline tags only |

## Reference material

- `references/editor-capabilities.md` — every element, what is stripped, clipboard
  mechanics, media and formula handling, limits, and what remains unverified.
- `references/writing-patterns.md` — structures for launches, promos, digests, guides,
  changelogs and longreads, with a worked example.
- `assets/copy-button.html` — drop-in copy implementation.

## Related targets (not this skill)

- **Bot API Rich Messages** (10.1–10.3, June–August 2026) — the same rich structure sent
  programmatically, *plus* buttons inside the text flow (`RichTextButton`) and between
  blocks (`RichBlockButtons`), with `callback_data`, `web_app`, styles, disabled states.
  This is how interactive in-message apps are built. **An author cannot make these in the
  editor** — they require a bot. See the note in `references/editor-capabilities.md`.
- **Bot API `parse_mode=HTML`** — ~11 inline tags, no headings, lists or tables.
- **Telegraph** (telegra.ph) — no Premium, automatic Instant View, but only `h3`/`h4`
  headings and no tables.
