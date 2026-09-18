# Examples

## showcase.html

Two ready-to-paste articles — **English and Russian** — that explain the skill while
demonstrating every element the Article editor supports. Each one is simultaneously a demo
and the documentation: paste it into Telegram and the article tells you what to look for.

**Live page: [uladzemer.github.io/telegram-article-skill](https://uladzemer.github.io/telegram-article-skill/)** — the same file, served
over https so the copy button works without a local server.

Open it (or this file locally) and press **Copy with formatting**, then in Telegram:
**paperclip 📎 → "Article" → Ctrl+V**.

Each article contains:

| Element | Count |
|---|---|
| Headings (H1 + H2) | 6 |
| Table | 1, with an emoji cell |
| Lists | 2 (bulleted + numbered) |
| Quote | 1 |
| Code block | 1 |
| Divider | 1 |
| Super / subscript | 2 |
| Inline code, links, bold, italic | throughout |

Each article is roughly **1 800 characters** — short enough to screenshot in one or two
shots, while still exercising every structural element.

Elements that **cannot** arrive through a paste — images, carousels, formulas, pull-quotes,
collapsible blocks, spoilers, highlight — are added in the editor by hand. The articles say so.

### What to verify when pasting

All of the following were confirmed on Telegram Desktop (see the screenshots above).
Re-run them when testing an unfamiliar client:

1. **Does the table survive?** ✅ on Desktop — the headline question, and the one published
   guides get wrong.
2. Do the heading levels come through as distinct sizes? ✅
3. Does the code block keep its line breaks and monospace? ✅
4. Do super- and subscript survive (πr², H₂O)? ✅
5. Does the emoji inside the table cell render? ✅
6. Does the quote become a real quote block? ✅

**Still untested:** mobile clients and web.telegram.org.

Separately worth testing by hand in the editor, since no paste can carry them: whether a
**checklist** created with the editor's own list style is tickable by a *reader*, and how
images, carousels and formulas behave.

## Screenshots

Both articles pasted into the Telegram Article editor — Desktop, Windows, Premium,
18 September 2026.

| English | Russian |
|---|---|
| ![English article in Telegram](screenshots/article-en.png) | ![Russian article in Telegram](screenshots/article-ru.png) |

**What the paste carried across:** all headings, both tables — complete with header row,
borders, alternating row fill and the 🎬 emoji in a cell — the quote, both list types, the
code block (which Telegram rendered with its own copy button), the divider, superscript and
subscript (πr², H₂O), inline code and links.

Nothing had to be rebuilt by hand. This contradicts published guides, which state that
tables cannot be pasted at all.

## copy-button.html

A blank version of the same mechanism lives in
[`../skills/telegram-article/assets/copy-button.html`](../skills/telegram-article/assets/copy-button.html)
— use it as the starting point for a new article rather than editing the showcase.
