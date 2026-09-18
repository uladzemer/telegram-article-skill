# Writing patterns for Telegram articles

The editor gives you structure. These patterns decide what to put in it.

Everything here assumes the reader is on a phone, scrolling, deciding within two seconds
whether to keep reading.

Patterns below: **launch**, **promo**, **digest**, **guide**, **longread**,
**technical writeup**, **changelog**. Then notes on media and on making parts of an
article clickable.

## Universal rules

1. **Hook first, mechanics later.** The first screen sells; the rest explains.
2. **Blocking requirements above the steps.** If the reader needs a desktop, a specific
   browser or an account, say so before they invest effort. A reader who fails silently
   never writes to support — they just leave.
3. **Steps go in a numbered list.** Never in prose. The reader is executing, not reading.
4. **One bold phrase per paragraph.** More than that and nothing stands out.
5. **Paragraphs of 2–3 sentences.** Walls of text lose the phone reader.
6. **Tables need 3+ comparable rows.** Below that, a list reads better.
7. **Name the exact UI labels** the reader will see — in quotes, matching the product
   character for character. Verify them; do not recall them.

## Pattern: product launch / announcement

```
H1  Hook — the offer in one line
    Lead paragraph: what launched, why they should care
    Continuity line (if relevant): what has NOT changed

H2  What's inside
    Table: item / what it gives   ← 3+ rows
    Disclaimer line if anything is excluded

H2  Before you start          ← blocking requirements, ABOVE the steps
    Platform, device, account prerequisites

H2  How to get access
    Numbered list: 3–5 steps, each one action
    Closing line: what they end up with

H2  What to know
    Table: condition / detail — browser, device limits, deadline, quantity

H2  Feedback
    Exact path to support, exact button names
    Why their feedback matters right now

    Closing line + link + code
```

The continuity line is underrated. If existing customers might think something changed
(pricing, plans, their access), say plainly that it did not. It prevents the single most
common support question after a relaunch.

## Pattern: promo / limited offer

```
H1  The offer, with the constraint in the title
    ("Free for 20 days", "Until 1 October")

    One paragraph: what it is, no preamble

H2  Before you start
    Requirements

H2  How to claim
    Numbered steps ending in the code

H2  Terms
    Table: what / detail — deadline, quantity, per-user limits

    Urgency line — honest, not manufactured
```

Scarcity claims must be true. "Limited slots" when nothing is limited is the fastest way
to lose a channel's credibility, and readers of small channels notice.

## Pattern: digest / roundup

```
H1  Period and theme

H2  Per item (repeating)
    2–3 sentences, bold the subject
    Link inline in the text, not as a bare URL

H2  In short
    Bulleted list — one line per item, for scanners
```

Put the summary at the **end**, not the start. Readers who scrolled through earned the recap;
readers who did not will scroll anyway.

## Pattern: guide / how-to

```
H1  What the reader will be able to do

    One paragraph: who this is for, what it assumes

H2  Before you start
    Checklist or list: prerequisites, versions, accounts

H2  Step 1 — <action>       ← one H2 per step for long guides
    Prose, then the commands in a code block
    Screenshot after the step it illustrates, not before

H2  Step N — <action>

H2  If something goes wrong
    Table: symptom / cause / fix     ← the most reused part of any guide

    Collapsible block: deeper background for the curious
```

Screenshots go **after** the step they show. A reader follows text, then confirms
against the image. Reversed, they compare an image to a state they have not reached.

Put troubleshooting in a table. It is what people return to the article for, and a table
is scannable in a way prose is not.

## Pattern: longread / analysis

```
H1  The claim, not the topic
    ("Why X stopped working", not "About X")

    Opening: the concrete detail that makes the question real

    Pull-quote: the sentence the piece rests on

H2  Section per movement of the argument
    Headings that read as an outline on their own

    Collapsible block: derivations, caveats, methodology

H2  What follows from this
    Bulleted list: conclusions

    Footer: sources
```

Headings are a table of contents. Read them top to bottom with nothing between —
they should still tell the story.

Use a pull-quote once. Twice and it stops being an accent.

## Pattern: technical writeup with math

```
H1  Result first

    What was measured or proven, in a sentence

H2  Setup
    Formula (block) for the model or definition

H2  Derivation
    Inline formulas inside prose; block formulas for the steps that matter

H2  Result
    Table: parameter / value / unit
```

Formulas are a native element — LaTeX source, rendered by the editor. Never paste an
image of a formula: it does not scale, does not adapt to theme, and cannot be copied.

Deliver the LaTeX in a copyable block so the user pastes it into the formula element.

## Pattern: changelog / update

```
H1  Version or period

    One paragraph: the single most important change

H2  What's new
    Checklist or bulleted list

H2  Fixed
    Bulleted list — user-visible symptoms, not internal ticket language

H2  Known issues        ← include it; omitting it costs more
    What is broken, what to do meanwhile
```

Write fixes as the user experienced them ("windows closed after 40 minutes"),
not as you tracked them ("session TTL desync in refresh handler").

## Worked example: a launch announcement

The structure below is drawn from a real announcement — a service relaunch offering a
free 20-day trial behind a promo code.

**H1** — `🎬 Netflix free for 20 days` — offer and constraint in the title.

**Lead** — what launched, one sentence on why feedback matters now.

**Continuity line** — "same service, same plans, only the platform changed" — because
existing customers' first fear is that their access or pricing moved.

**Table: what's inside** — seven services, each with one benefit phrase.
A list would have worked; the table wins because the second column is uniform.

**Disclaimer** — one line on what is excluded, immediately after the table,
before the reader imagines more than is offered.

**Blocking requirement** — desktop only, Windows or macOS, not phones. Placed *before*
the steps. In the earlier draft this lived near the bottom; readers on phones followed
three steps and hit a wall.

**Numbered steps** — four steps, each a single action, ending with the exact code.
Each UI label quoted exactly as it appears in the product.

**Table: what to know** — browser requirement, device limit, code scope, deadline.
Four short condition/detail rows: a list would have buried the deadline.

**Feedback section** — the exact navigation path and both button names, then one sentence
on why it matters. Vague "write to us" produces nothing; naming the button produces messages.

**Closing** — one line, the link, the code.

### What changed between drafts

- The support path was written from an older draft and named a menu that no longer existed.
  Checking the product's source corrected it. **Verify labels; never recall them.**
- Two lists became tables once they had uniform second columns.
- The platform requirement moved from the footer to above the steps.
- "Register" became the exact login-screen wording, including all three sign-in methods.

## Using media well

Media is added **in the editor**, not through the paste. Mark intended positions in the
draft so the user knows where each item goes.

- **One image per idea.** A stack of screenshots is a stack of screenshots; a carousel
  is one object the reader swipes.
- **Caption every image.** Each media item takes its own caption, and readers of scanned
  articles read captions before body text.
- **Screenshots after the step**, diagrams before the explanation. The first confirms,
  the second orients.
- **A carousel for alternatives or a sequence** — variants of a design, stages of a
  process. Not for unrelated images.
- **No images inside tables** — cells take text and emoji only. If a row needs a picture,
  the table is the wrong structure.
- Video and audio sit between paragraphs like images. Say what the reader will see or
  hear before it plays; autoplay decisions are not yours to make.

## Making parts of an article clickable

Within the editor an author has **hyperlinks** — but Telegram links can do more than open
a web page:

| Goal | Link |
|---|---|
| Open a bot | `t.me/<bot>` |
| Open a bot with a parameter | `t.me/<bot>?start=<param>` |
| **Launch a Mini App** | `t.me/<bot>?startapp=<param>` |
| Open a named Mini App | `t.me/<bot>/<short_name>?startapp=<param>` |
| Add bot to a group | `t.me/<bot>?startgroup=<param>` |
| Open app settings | `tg://settings/<path>` |

Written on a phrase like **"Open the app"**, a deep link reads as a button and behaves
like one. This covers most cases where an author wants a click to *do* something.

What it cannot do: react without leaving the message, change the article in place, or run
server logic. Those need a bot posting a Rich Message with real in-text buttons — a
different build, described in `editor-capabilities.md`.

## Language notes

- Write in the reader's language. For Russian-language channels, avoid untranslated
  English jargon — it reads as machine output.
- Avoid the announcement voice ("We are pleased to announce"). State what happened.
- Emoji as section markers work in Telegram; emoji inside sentences rarely do.
- Bold the noun phrase, not the verb: **"20 days free"**, not "is **now available**".
