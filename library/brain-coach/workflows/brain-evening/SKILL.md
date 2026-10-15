---
name: brain-evening
description: >-
  Evening reflection ritual — close the day with 3 gratitudes, Win of the Day, what worked / what didn't, and tomorrow's top 3. Appends to today's daily note (does not clobber morning entries). Use for end-of-day debriefs, the Re-Entry phase of a structured day, before-bed reflection, or any 'wind down the day' moment. Distinct from brain-journal (intentional reflection writing) and bizops-daily-brief (morning intelligence briefing).
---

# Brain Evening — Close the Day, Prep the Next

## Overview

Brain Evening is your end-of-day ritual driver. It walks you through five (sometimes six) prompts in 5-10 minutes and writes the result to today's daily note — appending, not overwriting, so morning entries and brain dumps from earlier in the day stay intact. The ritual is engineered around one principle: a day ends well when you name what worked, name what didn't, capture one win you're taking with you, set the bar for tomorrow, and turn the light off without an open loop running in your head. Optionally chains into `brain-habits` so you log today's non-negotiables in the same pass.

## When This Skill Applies

- User says "evening reflection" or "end of day" or "wind down" or "close the day"
- User says "Re-Entry" (matches the user's own framing) or "evening ritual"
- User says "let's review today" or "how did today go" (after 5pm)
- User says "before bed reflection" or "shutdown ritual" or "PM journal"
- User says "Win of the Day" or "what worked today"
- User triggers `/brain-evening`
- Time-of-day signal: any reflection request after 5pm local time

## Category

Cloud Brain Daily Rhythm

## Pre-Flight — Evening Preferences

1. Search Cloud Brain: `search_notes` with query `"brain coach evening preferences"` (folder: `brain/preferences`)
2. **If found:** Apply silently — banner is optional for evening (one-time daily flow, no menu fatigue)
3. **If not found:** Ask in ONE message:
   - Which sections to include: `gratitudes` (3 by default), `win`, `worked-didnt`, `tomorrow-top-3`, `scripture-reflection`, `reading-reflection` (default: first four ON, scripture/reading OFF)
   - Default tomorrow-priority count (default: 3)
   - Chain into `/brain-habits log` at the end? (default: yes)
   - Prompt verbosity: `terse` (one prompt at a time) or `full` (all visible at once) (default: terse)
   - Save to Cloud Brain: `write_note` → title: `brain-coach-evening-preferences`, folder: `brain/preferences`

## How It Works

### Step 1: Open Today's Daily Note

1. `search_notes` with query `daily/{today}` to find an existing daily note (e.g., `daily/2026-06-20.md`)
2. If found → `read_note` to load existing content (often there's a morning entry, a few brain-dumps, etc.)
3. If not found → prepare to create a new one

**Critical:** This skill APPENDS. It never overwrites. Anything already in today's daily note must be preserved.

### Step 2: Run the Ritual

Walk the user through the configured sections (terse or full per preference):

**A. Three Gratitudes**
> "Three gratitudes from today. Specific. Not 'my family' — what specifically about your family today?"

**B. Win of the Day**
> "What's your one Win of the Day? The thing you'd tell someone about at dinner."

**C. What Worked / What Didn't**
> "One sentence each. What worked today? What didn't?"

**D. Tomorrow's Top 3**
> "Three priorities for tomorrow. Not a to-do list — three things, max. What are they?"

**E. Optional — Scripture Reflection** (if enabled)
> "Anything from scripture or reading today that's still sitting with you?"

**F. Optional — Reading Reflection** (if enabled)
> "One sentence from your hour of reading that you want to remember?"

### Step 3: Append to Today's Daily Note

Build an `## Evening Reflection — {HH:MM}` section and append to today's daily note using `edit_note` (or create new note with `write_note` if none exists).

### Step 4: Chain into Habits (if enabled)

If preference allows, end with:
> "Want to log your non-negotiables for today? (yes/no)"

If yes, invoke `brain-habits log` flow.

### Step 5: Closing

End with one observation (not a summary). Examples:
- "You wrote 'kids' in two of your three gratitudes today. The signal is loud."
- "What didn't work today was scheduling-related — third evening in a row. Worth a Sunday Fueling Station look."
- "Tomorrow's #1 priority is the same as yesterday's #1. Either it's worth doing — or it's worth killing."

Then sign off cleanly:
> "Day closed. Bed by 9. ✓"

## Data Structure

### When appending to an existing daily note

The note may already have morning entries, dump entries, etc. Find the right insertion point — usually at the bottom — and append:

```markdown
---

## Evening Reflection — {HH:MM}

### Gratitudes
1. {response}
2. {response}
3. {response}

### Win of the Day
{response}

### What Worked
{response}

### What Didn't
{response}

### Tomorrow's Top 3
1. {response}
2. {response}
3. {response}

### Scripture / Reading _(if enabled)_
**Scripture:** {response}
**Reading:** {response}

> *Evening closed at {HH:MM}.*
```

### When creating a new daily note

If no daily note exists for today, create one with this minimal structure (the Evening Reflection section gets added at the end):

```markdown
---
date: {YYYY-MM-DD}
day_of_week: {Mon/Tue/etc.}
tags: [daily, brain-coach]
---

# {YYYY-MM-DD} — {Day of Week}

> *Day note. Sections will accumulate throughout the day from brain-dump, brain-evening, etc.*

---

## Evening Reflection — {HH:MM}

{… as above …}
```

## Output Format

After saving:

```
🌙 Evening reflection saved → daily/{YYYY-MM-DD}.md
{One-sentence observation — not a summary.}

Day closed. Bed by 9. ✓
```

If habit-chaining is on:
```
🌙 Evening reflection saved.
Now logging today's non-negotiables…
[hands off to brain-habits log]
```

## Example Usage

**User:** "/brain-evening"

**AI:**
1. Opens `daily/2026-06-20.md` (or creates it)
2. Walks through gratitudes → win → worked/didn't → tomorrow top 3 (terse, one at a time)
3. Appends `## Evening Reflection — 20:15` to the daily note
4. Asks if habits should be logged
5. Closes:

```
🌙 Evening reflection saved → daily/2026-06-20.md
Tomorrow's #1 ("finish Warrior Thoughts wireframes") is the same as yesterday's #1. Either it's the most important — or it's stuck. Worth naming which.

Day closed. Bed by 9. ✓
```

---

**User:** "Close the day. Three gratitudes only — I'm tired."

**AI:** Runs gratitudes section only (override). Saves. Returns clean confirmation. Does NOT push to do the rest. Brevity is a valid choice.

---

**User:** "/brain-evening --include scripture"

**AI:** Runs the standard flow plus the scripture reflection prompt. Saves with scripture quote in frontmatter for searchability.

## Error Handling

- **If today's daily note already has an Evening Reflection section:** Don't overwrite. Ask: "You already logged an evening reflection today at {time}. Add a second one, or overwrite the first?"
- **If the user gives one-word answers to every prompt:** Honor it. Save what they gave. Add a line in the entry: "Note: low-energy entry, brevity preserved." This is data, not a problem to fix.
- **If the user starts evening reflection at 1am or 5am (off-hours):** Proceed but ask once: "Reflecting on yesterday or today?" then route to the correct daily note.
- **If `daily/` folder doesn't exist:** Cloud Brain creates it on first write — proceed silently.
- **If the user wants to skip a section ("skip gratitudes"):** Skip it. Don't fight it. Save what they gave.
- **If the user opens this skill before noon:** Confirm: "It's morning — are you reflecting on yesterday's end-of-day? Or do you want `bizops-daily-brief` (CEO morning briefing) or `brain-daily-goals` (write today's goals) instead?"
- **If Cloud Brain is unreachable:** Save the entry locally to `~/.claude/brain-coach-pending/evening-{YYYY-MM-DD}.md`. Tell the user: "Cloud Brain unreachable — saved locally. Will sync on retry."
- **If the user's tomorrow-top-3 contradicts known constraints in their goal hierarchy:** Don't moralize. Note it once in your closing observation: "Heads up — none of tomorrow's top 3 connect to your keystone goal. Intentional or drift?"
