---
name: telegram-article
description: Write and format posts for Telegram's built-in Rich Text Editor (paperclip → "Article", Telegram 12.9+), which supports headings, tables, lists, quotes and collapsible blocks. Use when the user asks to write a Telegram post, announcement, channel article, product launch, promo or digest for Telegram — or mentions "режим статьи", "редактор статей", "Telegram article", "пост в телеграм", "Telegram Premium editor". The editor does NOT accept pasted HTML source; this skill produces a rendered HTML page whose formatting transfers into the editor through the clipboard. Not for Bot API sending (parse_mode=HTML/MarkdownV2) — that is a different target.
---

# Telegram Article Editor

Telegram's Rich Text Editor (shipped 14 July 2026 in Telegram 12.9) is a **WYSIWYG** editor,
not a markup field. It is reached via **paperclip 📎 → "Article"** and requires **Telegram Premium**.

The core problem it creates: people paste HTML source into it and get literal `<h1>` text.
The core insight this skill encodes: **the editor accepts rich clipboard content**. Copy a
*rendered* page from a browser and formatting — including tables — survives the paste.

This is not documented by Telegram, and published guides claim tables cannot be pasted.
They can (verified 2026-09-18, Telegram Desktop on Windows). See `references/editor-capabilities.md`.

## The rule that matters

> **Never hand the user HTML source to paste into Telegram.**
> Produce a rendered page with a copy button. The clipboard carries the formatting.

Pasting source is the single most common failure. The tags appear verbatim as text.

## Workflow

### 1. Establish the facts before writing

Marketing copy invents details. Before writing any step the reader will follow:

- **Verify UI labels against the actual product** — menu names, button labels, section titles.
  Grep the codebase or ask. Do not reproduce paths from an older draft; they drift.
- **Verify offer terms** — promo code, duration, deadline, limits, platform requirements.
- If a fact cannot be verified, ask rather than guess. A wrong button name in an announcement
  becomes a support ticket on day one.

### 2. Structure for the format

The editor has real structural elements. Use them for what they are:

| Element | Use it for |
|---|---|
| Heading (6 levels; use 2) | Section breaks a reader scans |
| Table | Comparable rows: service/benefit, condition/detail, plan/price |
| Numbered list | Steps the reader performs in order |
| Bulleted list | Non-sequential facts, benefits |
| Checklist | Requirements the reader ticks off |
| Bold | The one phrase per paragraph that must land |
| Collapsible quote | Long caveats that would break the flow |

Rules of thumb:

- Lead with the hook, then the offer, then the mechanics, then the caveats.
- Paragraphs of 2–3 sentences. This is read on a phone.
- Put steps in a numbered list, never in prose.
- A table needs 3+ rows to earn its place; below that use a list.
- Caveats that cause support tickets (platform, browser, device limits) go **above** the steps,
  not in a footnote. Readers who fail silently churn.

### 3. Produce the deliverable

Write **one HTML file** — a rendered page containing:

1. The announcement, styled for reading in a browser.
2. A **hidden block** holding the same content in editor-compatible markup
   (`h1`/`h2`/`p`/`ul`/`ol`/`table`/`b`/`a` only — no classes, no styling).
3. A **copy button** that writes both `text/html` and `text/plain` to the clipboard.
4. Paste instructions and the Premium caveat.

Copy `assets/copy-button.html` for a working implementation. Key points:

- Use `ClipboardItem` with **both** MIME types. The editor takes `text/html`;
  a plain message field takes `text/plain`.
- Keep a `document.execCommand('copy')` fallback over a selected clone — `ClipboardItem`
  is unavailable outside secure contexts.
- The hidden block must be **rendered**, not a `<template>`. Off-screen positioning
  (`position:absolute;left:-99999px`) keeps it selectable; `display:none` does not.
- Generate the plain-text variant by walking the DOM: lists become `• ` / `1. ` lines,
  table rows collapse to `cell — cell`.

### 4. State the limits honestly

Always tell the user, in the deliverable itself:

- The editor requires **Telegram Premium**.
- There is **no autosave** — closing the editor loses the text.
- Test the paste on one paragraph before committing the whole text.
- Article messages hold **32 768** characters (8× a normal message).

## Anti-patterns

| Don't | Why |
|---|---|
| Hand over HTML source to paste | Tags appear literally. This is the failure users hit. |
| Style the hidden block with CSS | Background, fonts and colors are stripped. Only structure survives. |
| Use `<div>` wrappers in the hidden block | Adds nothing; the editor keeps only semantic elements. |
| Reproduce UI paths from an old draft | Product labels drift. Verify against the code. |
| Bury platform requirements at the bottom | Readers fail silently and never write to support. |
| Write one long block of prose | Unreadable on a phone, where it will be read. |
| Target Bot API `parse_mode` | Different product: inline tags only, no headings/tables/lists. |

## Reference material

- `references/editor-capabilities.md` — what the editor supports, what it strips,
  the clipboard mechanism, limits, and how this was verified.
- `references/writing-patterns.md` — structures for announcements, launches,
  digests and promos, with a worked example.
- `assets/copy-button.html` — drop-in copy implementation.

## Related targets (not this skill)

- **Bot API** (`parse_mode=HTML`) — programmatic sending. ~11 inline tags,
  no headings, lists or tables. Use for bots, not for human-authored articles.
- **Telegraph** (telegra.ph) — no Premium needed, gets Instant View automatically,
  but only `h3`/`h4` headings and **no tables**.
