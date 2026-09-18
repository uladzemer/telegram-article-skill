# Carousel slides

Four 1080×1080 slides for the announcement post — a swipeable carousel that tells the
story without requiring the reader to read the text.

| File | Slide |
|---|---|
| `1-cover.png` | Cover — "articles for Telegram, written by an agent" |
| `2-problem.png` | The problem: pasted source ✕ vs copied rendered page ✓ |
| `3-features.png` | What the editor offers, and what it does not |
| `4-verified.png` | The two undocumented findings, plus the repository link |

Built from `slides.html` in this folder and captured at 1080×1080 with a headless browser.
Regenerate by serving the file over http and screenshotting each `.slide` at that viewport.

## Assembling the carousel in Telegram

1. Insert the first image
2. Use the **+** in its top-right corner to add the rest, in order
3. Press the **tiles button** next to the + to turn the set into a carousel
4. Caption each slide — captions are per image
