---
name: eos-todos
description: >-
  Manage EOS To-Dos — the 7-day verb-led action items that come out of every L10. Each has one owner, one due date (≤7 days). Target ≥90% completion week-over-week. Auto-rolls 2+ week stalls into the Issues List. Use for adding a to-do, listing open to-dos, marking complete, weekly recap, or needs to add a to-do, list open items, mark one complete, or run a weekly recap from an L10.
---

# EOS To-Dos — 7 Days, One Owner

## Overview

To-Dos are EOS's 7-day action items. They come out of L10 IDS, Headlines, or Scorecard review. Every to-do is verb-led, has one owner, and is due before the next L10. The team aims for **≥90% completion week-over-week** — anything below that is a meeting/team health signal. To-Dos that stall 2+ weeks auto-roll into the Issues List per EOS convention.

## When This Skill Applies

- User says "add a to-do", "follow up on [X]", "I need to [verb]"
- User says "mark [X] as done", "close that to-do"
- User asks "what to-dos do I have", "open to-dos", "weekly to-do recap"
- `eos-level10` opens this during the 5-minute To-Do review and during IDS Solves
- `eos-ids` writes a To-Do as its Solve output
- User invokes `/eos-todos`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"`, `"eos company preferences"`
2. `search_notes "EOS To-Dos"` — existing list
3. `search_notes "EOS L10"` (recent) — for prior-week reference

**Banner:**
```
✅ EOS To-Dos | {Company} | Open: {N} | Due This Week: {N} | Overdue: {N}
```

## How It Works

### Step 0: Determine Action

Parse to: `add`, `list`, `done`, `recap`. Default: `list`.

### Step 1: Add

Capture:
- **Action** — verb-led, specific. *"Call Bob about renewal"* not *"follow up with Bob"*.
- **Owner** — one person
- **Due** — within 7 days (default: next L10 day from prefs)
- **Source** — IDS solve / headline / scorecard / ad-hoc / `--source-issue <id>`
- **Created** — today

Append to `EOS To-Dos`. Tag with current ISO week.

### Step 2: List

`read_note` → render two tables:
- Open (sorted by due date)
- Overdue (highlighted ⚠️)

### Step 3: Done

Find the to-do by partial match. Mark complete with date. Move to `## Completed (last 30 days)`.

### Step 4: Recap (called by L10)

For the upcoming L10:
- Count open to-dos from last 7 days
- Count completed
- Compute completion % vs 90% target
- Identify any to-do open for 2+ weeks → auto-suggest rolling to Issues List

Output:
```
TO-DO RECAP — Week of {date}
  Created last week: 12
  Completed: 11 ✅ → 92% (target ≥90%)
  Still open: 1
  ROLLOVER: 1 to-do open 14+ days → recommend adding to Issues List for IDS
```

### Step 5: Auto-Rollover

For any to-do open >14 days:
- Surface to the user
- Offer: *"This to-do has been open 16 days. Per EOS, roll into Issues List for IDS?"*
- On confirm: write to `EOS Issues List` (Short-Term) with `Source: stalled-todo` and close the original to-do with resolution `rolled-to-issue`.

## Data Structure

`EOS To-Dos` note:

```markdown
# EOS To-Dos — {Company}

> **Last Updated:** {YYYY-MM-DD}
> **Open:** {N} · **Overdue:** {N} · **Completed (30d):** {N}
> **Weekly Completion Rate (4-wk avg):** {pct}% (target ≥90%)

---

## Open

| Due | Owner | Action | Source | Created | Age (days) |
|-----|-------|--------|--------|---------|------------|
| 9/12 | Sarah | Call Bob re: renewal | IDS | 9/5 | 2 |
| 9/12 | Mark  | Send Q3 numbers to board | Headline | 9/5 | 2 |
| 9/12 | Jen   | Draft new onboarding email | Scorecard | 9/5 | 2 |

## Overdue ⚠️

| Was Due | Owner | Action | Source | Age |
|---------|-------|--------|--------|-----|
| 8/29 | Mark | Hire Marketing Manager outreach | IDS | 9 days ⚠️ |

## Completed (last 30 days)

| Completed | Owner | Action |
|-----------|-------|--------|
| 9/4 | Sarah | Sent proposal to ABC Corp |
| 9/3 | Mark | Filed Q2 |
| …
```

## Output Format

### List

```
✅ EOS To-Dos | {Company} | Open: 7 · Due this week: 5 · Overdue: 1 ⚠️

OPEN (sorted by due date):
  9/12  Sarah  Call Bob re: renewal              [IDS]
  9/12  Mark   Send Q3 numbers to board          [Headline]
  9/12  Jen    Draft new onboarding email        [Scorecard]
  9/13  Sarah  Update sales deck                 [IDS]
  9/14  Mark   Prep Q4 budget draft              [Quarterly]

OVERDUE:
  ⚠️ 8/29  Mark   Marketing Manager outreach     [IDS, 9 days overdue]
      → Roll to Issues List for IDS?

THIS WEEK'S PACE: 5 of 7 to-dos due by next L10 → expect ~71% — below 90% target ⚠️
```

### Recap (called by L10)

```
TO-DO RECAP — Week ending {date}
  Created last week: 12
  Completed: 11 ✅ → 92%   (target ≥90% — ✅ on pace)
  Still open: 1 (Mark — Marketing Manager outreach)
  Open >14 days: 1 — recommend Issues List rollover
```

## Example Usage

**User:** "Add to-do: Sarah, call Bob about the renewal, due Friday."
**AI:** Captures. Saves. Confirms.

**User:** "Open to-dos."
**AI:** Renders open + overdue tables.

**User:** "Mark done — Sarah's call to Bob."
**AI:** Finds by partial match. Marks complete. Moves to Completed section.

**User:** "/eos-todos recap"
**AI:** Renders the weekly recap with completion % vs 90% target.

**User:** "Roll Mark's marketing outreach to-do to issues."
**AI:** Writes to `EOS Issues List`, closes the to-do with resolution `rolled-to-issue`.

## Error Handling

- **No verb in action ("the proposal"):** Push: *"To-dos are verb-led. Should it be 'send the proposal'? 'review the proposal'? Be specific."*
- **Owner is a department or "we":** Refuse: *"To-dos have one named owner. Who specifically?"*
- **Due date >7 days out:** Warn: *"To-dos are 7-day actions. Anything longer is a Rock or a long-term item. Compress, or move to /eos-rocks?"*
- **Same action duplicates an open to-do:** Suggest editing the existing one.
- **Completion rate <90% (4-wk avg):** Surface as a meeting health issue in the recap. Suggest IDS at next L10.
- **To-do open >14 days:** Always surface for Issues rollover. Do not auto-roll without confirmation.
- **`EOS To-Dos` note doesn't exist:** Create on first add. Don't error.
- **User marks done a to-do that doesn't exist:** Fuzzy-match by action text and owner. If multiple matches, ask which.
