---
name: pro-daily-gift
description: >-
  Generate a daily gift — a quote, a curated article link, a brief story, or an AI-generated illustration — tailored to your goals and current focus. Reads your active goals, current projects, and recent journal entries from Cloud Brain to pick something relevant, not generic. Designed to slot into your morning briefing or evening reflection.
---

# Daily Gift

## Overview

A small, well-targeted bit of content delivered once per day. Designed for solopreneurs who don't have time for a "morning newsletter" but want something to chew on. Four types:

1. **Quote** — short, ideally relevant to a current goal or struggle
2. **Article** — link to a single high-signal piece, with a 2-3 sentence preview
3. **Story** — short anecdote or case study (50-200 words)
4. **Illustration** — AI-generated image based on a current theme (uses pro-article-illustrate under the hood)

The pick is conditioned on Cloud Brain context — what you're working on, what you just journaled about, who you're meeting today. Not a generic "Quote of the Day" feed.

## When This Skill Applies

- User asks for a daily gift, morning gift, daily inspiration
- User wants a quick "thinking starter" for the day
- Calendar/routine trigger: morning brief, evening reflection
- User says: "give me something good", "what should I think about today", "gift me a quote"

## Pre-Flight — Preferences

Load `daily gift preferences` from `brain/preferences`:
- Preferred type rotation (quote / article / story / illustration — equal mix, or weighted)
- Themes you care about (entrepreneurship / family / fitness / Spanish / faith / RE — multi-select)
- Themes to avoid (politics / hype / news / etc.)
- Default send channel (display only / iMessage / WhatsApp / email)
- Quote sources you like (specific authors, books — pulls preferentially)

If not set, ask in ONE message and save.

Banner:
```
🎯 Daily Gift | Type: {auto / specified} | Themes: {list} | Channel: {channel}
```

## How It Works

### Step 1: Read Context from Cloud Brain

Pull recent context:
- `recent_activity` last 7 days
- `read_note` on active goals (`goals/` or `brain/goals.md`)
- `read_note` on current pulse / journal entries (`daily/`, `journal/`)
- Today's calendar focus (if available)

Identify 1-3 active themes: what's on the user's mind this week?

### Step 2: Choose Gift Type

If `--type` specified, use it.

Otherwise rotate per preferences. Default rotation: Mon: quote, Tue: article, Wed: story, Thu: quote, Fri: article, Sat: illustration, Sun: story.

### Step 3: Generate the Gift

**Quote:**
- Use WebSearch with theme-relevant query (e.g., "quote about persistence Charlie Munger")
- Filter for primary-sourced (book, talk, interview) — not "quote site" garbage
- Match against user's preferred authors if listed
- Surface ONE quote + brief context (source, when said)

**Article:**
- WebSearch for high-signal content on the theme published in last 7-14 days
- Prefer: known publications, authors user follows, sources Cloud Brain has prior notes from
- Fetch the URL via WebFetch
- Generate 2-3 sentence preview

**Story:**
- Generate a relevant anecdote (historical, case study, parable) — 50-200 words
- Tie to a current theme
- End with a question the user can chew on

**Illustration:**
- Invoke `pro-article-illustrate` with the active theme as the prompt seed
- Display the generated image inline (if MCP supports it) or save and report path

### Step 4: Optional Delivery

If `--send-as` is set:
- iMessage: use `comm-imessage-send` (from communications plugin)
- WhatsApp: use `comm-whatsapp-notifier`
- Email: use `comm-email-drafter` to create draft

### Step 5: Save

- **title:** `Daily Gift — {YYYY-MM-DD}`
- **folder:** `knowledge-base/gifts`
- **tags:** `["daily-gift", "{type}", "{theme}"]`

## Data Structure

```markdown
# Daily Gift — {YYYY-MM-DD}

> **Type:** {quote / article / story / illustration}
> **Theme:** {theme}
> **Tied to:** {goal or current focus from Cloud Brain}

## The Gift

{Quote / article preview / story / illustration here}

### Source
{source / author / link}

### Why this today
{1-2 sentence justification — tie to current Cloud Brain context}

### Prompt to reflect
{1 question the user can sit with}

---

**Send via:** {channel or "display only"}
```

## Output Format (Chat)

For a quote:
```
🎁 DAILY GIFT — {date}
Theme: {theme}

"{quote text}"
— {author}, {source}

WHY TODAY
{1-2 sentences}

REFLECT ON
{1 question}

Saved: knowledge-base/gifts/Daily Gift — {date}
```

For an article:
```
🎁 DAILY GIFT — {date}
Theme: {theme}

📖 {article title}
{author / publication / date}
{url}

PREVIEW
{2-3 sentence preview}

WHY TODAY
{1-2 sentences}

Saved: knowledge-base/gifts/Daily Gift — {date}
```

For a story:
```
🎁 DAILY GIFT — {date}
Theme: {theme}

{story body, 50-200 words}

REFLECT ON
{1 question}

Saved: knowledge-base/gifts/Daily Gift — {date}
```

For an illustration:
```
🎁 DAILY GIFT — {date}
Theme: {theme}

🎨 [illustration displayed inline or saved to path]
"{caption}"

WHY TODAY
{1-2 sentences}

Saved: knowledge-base/gifts/Daily Gift — {date}
```

## Example Usage

**User:** "Give me a daily gift"

**AI:** Reads recent journal entries — user has been working on Warrior Thoughts launch. Rotates to "story" today per default. Generates a 150-word anecdote about a similar founder's launch. Saves.

**User:** "/pro-daily-gift --type quote --theme persistence"

**AI:** WebSearches for persistence-themed quotes from user's preferred sources. Picks one. Provides context. Saves.

**User:** "Send today's gift to Mary as an iMessage"

**AI:** Generates gift. Sends via iMessage (if communications plugin installed). Confirms send. Saves with channel logged.

**User:** "Daily gift — illustration only, theme is 'morning launch routine'"

**AI:** Invokes pro-article-illustrate with the theme. Returns image + caption. Saves.

## Error Handling

- **If Cloud Brain has no goals / no journal entries:** Use defaults from preferences themes, not personal context. Note: "Daily Gift personalizes from your goals + journal — set those up via `/gsd:add-todo` and `/brain-journal` for sharper picks."
- **If WebSearch returns mostly low-quality quote sites for quote mode:** Fall back to a curated set from Cloud Brain (if user has saved favorite quotes) or general well-known philosophers / strategists.
- **If article fetch fails:** Try alternate sources for the same theme. If all fail, fall back to quote.
- **If illustration generation isn't available (no image gen MCP):** Fall back to a different type with explanation.
- **If communications plugins aren't installed and user wants to send:** Display gift; tell user to copy/paste.
- **If user wants the same gift twice (dedup):** Check `knowledge-base/gifts/` for prior gifts within 30 days; pick a different one if duplicate.
- **If a gift would conflict with avoid-themes preferences:** Re-roll. Don't push politics / hype / news if user said avoid.
- **If user wants longer or shorter gift:** Override length. Don't deliver a 500-word "quote".

## See Also

- `/pro-article-illustrate` — generate the illustration when gift type is image (same plugin)
- `/brain-mantras` — affirmations counterpart from your own brain (from `brain-coach`)
- `/comm-imessage` — deliver the gift to family / partner (from `communications`)
- `/comm-whatsapp-notifier` — WhatsApp delivery (from `communications`)
