# Writing patterns for Telegram articles

The editor gives you structure. These patterns decide what to put in it.

Everything here assumes the reader is on a phone, scrolling, deciding within two seconds
whether to keep reading.

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

## Language notes

- Write in the reader's language. For Russian-language channels, avoid untranslated
  English jargon — it reads as machine output.
- Avoid the announcement voice ("We are pleased to announce"). State what happened.
- Emoji as section markers work in Telegram; emoji inside sentences rarely do.
- Bold the noun phrase, not the verb: **"20 days free"**, not "is **now available**".
