---
name: brain-habits
description: >-
  Non-negotiables and habits tracker — daily yes/no log with streak rollups and a weekly heatmap. Three sub-commands: log (today's checklist in one message), streak (current streaks + heatmap), edit (add/remove a habit). Use for tracking daily disciplines, non-negotiables, the 'did I do my X today' check, or weekly habit review. Designed to chain off brain-evening so you log habits as part of closing the day.
---

# Brain Habits — Track What You Said You'd Do

## Overview

Brain Habits turns your non-negotiables list into a daily yes/no checklist with persistent state in Cloud Brain. One message per day to log everything. One command to see your current streaks and a weekly heatmap. One command to add or remove a habit when your standards evolve. The whole skill is engineered around the principle that habits live or die at the friction point — so the log flow is a single message with checkboxes, not a 20-question interrogation. What gets measured gets done.

## When This Skill Applies

- User says "log my habits" or "log non-negotiables" or "habit check"
- User says "did I X today" or "show my streaks" or "habit streaks"
- User says "edit my habits" or "add a habit" or "remove a habit"
- User triggers `/brain-habits` (with or without sub-command)
- Chained automatically from `brain-evening` if user opted in
- Weekly review context (Sunday) wanting to see the heatmap

## Category

Cloud Brain Daily Discipline

## Pre-Flight — Habits Preferences + Seed List

### Preferences
1. Search Cloud Brain: `search_notes` with query `"brain coach habits preferences"` (folder: `brain/preferences`)
2. **If found:** Apply silently
3. **If not found:** Ask in ONE message:
   - Reminder time (when to nudge if not logged) — optional, default: none
   - Weekly review day (default: Sunday)
   - Streak grace policy: `strict` (any miss resets streak), `gracious` (one miss per 7 days allowed), `monthly-grace` (one miss per 30) — default: `gracious`
   - Display: `compact` (count + streaks only) or `full` (count + streaks + heatmap) for `streak` view — default: `full`
   - Save to Cloud Brain: `write_note` → title: `brain-coach-habits-preferences`, folder: `brain/preferences`

### First-Run Seed of Habit List

If `habits/habits.md` does not exist:

1. Tell the user: "You don't have a habits list yet. Want to seed it from non-negotiables in your CLAUDE.md, or start from scratch?"
2. If seeding from CLAUDE.md, surface candidates (look for declarative habits in a "Non-Negotiables" or "Standards" section). User confirms which to keep.
3. Otherwise: "Type your habits one per line. Examples: 'Bed by 9', 'Lift weights', 'Read 1 hour'. Don't overthink."
4. Save initial list to `habits/habits.md` with sort order = entry order.

## How It Works

### Sub-Command: `log` (default if no sub-command)

The single-message daily log.

1. Load `habits/habits.md` for the list
2. Load `habits/{YYYY-MM}.md` for the current month's log
3. Check whether today already has a log entry
4. If yes: ask "You already logged today at {time}. Update, or just show today's result?"
5. If no: present ALL habits as a checklist in ONE message:

```
✅ Habits log — {YYYY-MM-DD}

Mark each:
- [ ] 1. {habit 1}
- [ ] 2. {habit 2}
- [ ] 3. {habit 3}
…
- [ ] {N}. {habit N}

Reply with the numbers you DID (e.g., "1,2,5,8") or "all" or "none".
```

User replies. Parse. Save.

### Sub-Command: `streak`

Show current state:

```
🔥 Habits — Streaks as of {YYYY-MM-DD}

| # | Habit | Streak | Best | 7-Day | 30-Day |
|---|-------|--------|------|-------|--------|
| 1 | Bed by 9 | 12d | 47d | 6/7 | 27/30 |
| 2 | Lift weights | 4d | 32d | 5/7 | 22/30 |
…
```

If `display=full`, also render a heatmap:

```
Last 30 days (✅=yes, ⬜=no, ⏸=not yet today):

         M T W T F S S
W of 5/26 ✅✅✅⬜✅✅✅
W of 6/02 ✅✅✅✅✅⬜✅
W of 6/09 ✅✅⬜✅✅✅✅
W of 6/16 ✅✅✅✅⏸
```

(Per-habit heatmaps if user asks `--habit {N}`.)

### Sub-Command: `edit`

- **Add:** "What's the habit?" → ask phrasing ("'Lift weights' or 'I lift weights' — short form is better") → append → save
- **Remove:** Show numbered list → "Which to remove?" → confirm → soft-archive to `habits/habits-archived.md` (don't hard-delete — historical streak data references it)
- **Reorder:** Show numbered list → "New order? (e.g., '3,1,2,4')" → rewrite order → save

## Data Structure

### `habits/habits.md` — the list

```markdown
---
type: habits-list
last_updated: {YYYY-MM-DD}
streak_policy: {gracious|strict|monthly-grace}
tags: [habits, non-negotiables, brain-coach]
---

# My Habits

1. Bed by 9 PM
2. Journal, visualize, and write goals
3. Pray and read scriptures
4. Read 1 hour
5. Spanish lesson
6. Temple monthly
7. Lift weights (6×/week target)
8. Cardio
9. Live within budget
10. Live by the calendar
11. Pay tithing
12. Breathwork / meditate
13. Surround with high performers
```

### `habits/{YYYY-MM}.md` — the monthly log

```markdown
---
type: habits-monthly-log
month: {YYYY-MM}
tags: [habits, log, brain-coach]
---

# Habits Log — {Month YYYY}

| Date | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 |
|------|---|---|---|---|---|---|---|---|---|----|----|----|----|
| 2026-06-20 | ✅ | ✅ | ⬜ | ✅ | ✅ | ⬜ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⬜ |
| 2026-06-19 | ✅ | ✅ | ✅ | ✅ | ⬜ | ⬜ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 2026-06-18 | ✅ | ✅ | ✅ | ✅ | ✅ | ⬜ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
{…}

## Streak State (computed)

| Habit | Current Streak | Best Streak |
|-------|---------------|-------------|
| 1. Bed by 9 PM | 12 | 47 |
| 2. Journal/goals | 7 | 21 |
{…}
```

### `habits/habits-archived.md`

```markdown
---
type: habits-archived
tags: [habits, archived, brain-coach]
---

# Archived Habits

- **{YYYY-MM-DD}:** Removed "{habit text}" — reason: {one line if provided}
```

## Streak Computation Rules

For each habit, walk backwards from today:
- `strict`: streak ends at the first ⬜
- `gracious`: streak ends if the past 7 days have ≥2 ⬜
- `monthly-grace`: streak ends if the past 30 days have ≥2 ⬜

Today's box (if not yet logged) shows as ⏸ and does NOT break the streak — it only matters once the user has logged or the day ends.

## Output Format

### After `log`

```
✅ Logged — {YYYY-MM-DD} — {N}/{Total} hit
{habits in 'didnt-do' list, if any, comma-separated}

Streak highlights:
- 🔥 Bed by 9: 12 days
- 🔥 Lift weights: 4 days  
- ⚠️ Spanish lesson: streak broken (was 6 days)

{If chained from brain-evening: append "Day fully closed. Bed by 9. ✓"}
```

### After `streak`

The table + heatmap (see Sub-Command: streak above). Followed by:

```
Watching this week: {1-2 habits with shortest streak or worst 7-day score}
```

### After `edit`

```
✅ Habits list updated — now {N} total
```

## Example Usage

**User:** "/brain-habits log"

**AI:** Loads habits list. Renders single-message checklist with all 13 habits. User replies "1,2,4,7,8,9,10,11,12". AI parses, saves to `habits/2026-06.md`, returns:

```
✅ Logged — 2026-06-20 — 9/13 hit
Missed: scriptures, Spanish, Temple, breathwork, high-performers

Streak highlights:
- 🔥 Bed by 9: 12 days
- 🔥 Lift weights: 4 days
- ⚠️ Scriptures: streak broken (was 8 days)
```

---

**User:** "/brain-habits streak"

**AI:** Returns full table + 30-day heatmap. Highlights the habits trending down.

---

**User:** "/brain-habits edit"

**AI:** "Add, remove, or reorder?" → User: "Add 'Cold plunge'" → "Phrasing? 'Cold plunge' is good, or 'Daily cold plunge'?" → User: "'Cold plunge'" → AI saves → "✅ Habits list updated — now 14 total"

---

**Triggered automatically by brain-evening:**

After `brain-evening` finishes, if habit-chain is on, this skill runs `log` immediately with no extra preamble. User sees the checklist and replies once.

## Error Handling

- **If `habits/habits.md` doesn't exist:** Run first-run seed flow. Don't error.
- **If the monthly log file doesn't exist for the current month:** Create it with the right header structure and proceed.
- **If user replies "all" but some habits are physically impossible today (e.g., Temple is monthly):** Accept "all" as-is. Pattern over weeks is more useful than per-day perfectionism. If user wants exclusions, support `--exclude` flag in a later version.
- **If user logs habits twice in one day with different answers:** Confirm: "You logged earlier — overwrite, or which version is accurate?" Default to overwrite (most recent is most accurate).
- **If the user removes a habit that has streak data:** Soft-archive (move to archived). Don't lose history. Tell them: "Archived — streak history preserved at habits/habits-archived.md."
- **If today is in the future or past (clock issue):** Trust the system clock. Log to today's date.
- **If the user's habits list is >25 items:** Suggest once: "You have {N} habits — that's a lot to track daily. Want to split into 'daily non-negotiables' vs. 'weekly targets'?" Don't force.
- **If user logs 0/N for 3 days in a row:** Don't shame. Note it once in the closing line: "Three zero-logs in a row. Often that's a calendar problem, not a discipline problem. Worth a Sunday Fueling Station look."
- **If Cloud Brain is unreachable during `log`:** Save the day's log to `~/.claude/brain-coach-pending/habits-{YYYY-MM-DD}.md` and tell the user: "Saved locally — will sync when Cloud Brain is back."
- **If the `streak` view is requested but the monthly file has gaps (days with no log):** Render gaps as ⬜ (counted as miss) by default. Add a footer note: "{N} days had no log this month — those count as misses in streak math. Switch to `--policy log-aware` to ignore unlogged days." (future option.)
