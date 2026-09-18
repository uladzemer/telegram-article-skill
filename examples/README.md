# Examples

## showcase.html

Two ready-to-paste articles — **English and Russian** — that explain the skill while
demonstrating every element the Article editor supports. Each one is simultaneously a demo
and the documentation: paste it into Telegram and the article tells you what to look for.

Open the file in a browser and press **Copy with formatting**, then in Telegram:
**paperclip 📎 → "Article" → Ctrl+V**.

Each article contains:

| Element | Count |
|---|---|
| Headings (H1 + H2) | 9 |
| Tables | 2 (one with an emoji cell) |
| Lists | 2 (bulleted + numbered) |
| Quote | 1 |
| Code block | 1 |
| Divider | 1 |
| Super / subscript | 2 |
| Inline code, links, bold, italic | throughout |

Elements that **cannot** arrive through a paste — images, carousels, formulas, pull-quotes,
collapsible blocks, spoilers, highlight — are added in the editor by hand. The articles say so.

### What to verify when pasting

1. Do all six heading levels come through as distinct sizes?
2. **Do the tables survive?** Published guides say they cannot; our test says they do.
3. Does the checklist become a real checklist — and can a *reader* tick the boxes?
4. Does the nested list keep its second level, or flatten?
5. Does the code block keep its line breaks?

## Screenshots

Results of pasting `showcase.html` into the Telegram Article editor.

<!-- Add screenshots here as they are captured:
![English article in the editor](screenshots/en-editor.png)
![Russian article in the editor](screenshots/ru-editor.png)
![Published article as readers see it](screenshots/published.png)
-->

_Not captured yet._

## copy-button.html

A blank version of the same mechanism lives in
[`../skills/telegram-article/assets/copy-button.html`](../skills/telegram-article/assets/copy-button.html)
— use it as the starting point for a new article rather than editing the showcase.
