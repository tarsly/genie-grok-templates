---
name: eos-scorecard
description: >-
  Define and manage your EOS Scorecard — the 5-15 weekly leading-indicator numbers that give a true pulse of your business. Log weekly numbers, render a 13-week trailing view with sparklines and trend arrows, auto-flag off-track misses as Issues for IDS at your next L10. Coach mode suggests measurables by department (Sales/Marketing/Ops/Finance/People) and pushes back against lagging-only scorecards. Use for setting up a scorecard, logging this week's numbers, reading the scorecard report, or needs to add a measurable, log this week's numbers, or pull a 13-week trailing scorecard report.
---

# EOS Scorecard — The Pulse of Your Business

## Overview

The Scorecard is 5-15 weekly, leading-indicator numbers that give an objective pulse of the business. Every measurable has one owner. Every week the owner reports the number. Misses become Issues for the next L10. This skill defines measurables, logs weekly numbers, renders the canonical 13-week trailing view, and auto-flags off-track items. Coach mode helps owners pick the right metrics — leading (predictive) over lagging (after-the-fact), activity over outcome alone, one owner per measurable.

## When This Skill Applies

- User wants to set up a scorecard for the first time
- User says "log my numbers", "update scorecard", "this week's numbers"
- User asks "show the scorecard", "scorecard report", "how are we doing"
- User asks "what should I measure for [department]?" — triggers coach mode
- `eos-level10` calls this skill during its 5-minute Scorecard review
- User invokes `/eos-scorecard`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"` — company name, industry
2. `search_notes "eos company preferences"`
3. `search_notes "EOS Accountability Chart"` — to know who owns what department (for owner suggestions)
4. `search_notes "EOS V/TO"` — for 1-Year Plan goals (the scorecard should connect to them)
5. `search_notes "EOS Scorecard"` — existing scorecard

**Banner:**
```
📊 EOS Scorecard | {Company} | Week of {YYYY-MM-DD} | Measurables: {N} | Onboarding: ✓
```

## How It Works

### Step 0: Determine Action

Parse to: `setup`, `log`, `report`, `coach`, `add-measurable`. Default: if no scorecard exists → `setup`; else → `report`.

### Step 1: Setup

If no scorecard, walk through measurable definition. Target: 5-15 measurables.

For each measurable, capture:
- **Name** — what the metric is called (e.g., "New Qualified Leads")
- **Owner** — one person (pull suggestions from Accountability Chart)
- **Goal** — the weekly target (e.g., 20)
- **Direction** — higher is better OR lower is better
- **Leading/Lagging** — is this predictive (leading) or after-the-fact (lagging)? Push for ≥70% leading.
- **Source** — where the number comes from (manual / CRM / analytics / etc.)
- **Department** — Sales / Marketing / Ops / Finance / People

**Coach mode — measurable suggestions by department:**

| Department | Leading Indicators | Lagging Indicators (use sparingly) |
|------------|-------------------|------------------------------------|
| Sales | Demos booked, proposals out, outreach touches, pipeline value | Closed revenue |
| Marketing | MQLs, content published, email CTR, ad spend efficiency | Brand mentions |
| Operations | On-time delivery %, error rate, NPS, response time | Customer churn |
| Finance | Cash on hand, AR > 30 days, invoice count | Revenue, profit |
| People | Open positions, time-to-hire, GWC pass rate, 1-on-1s completed | Turnover rate |

If the user proposes only lagging indicators, push back: *"Revenue and profit are lagging — by the time they move, the cause is already weeks old. Want to pair each with a leading indicator that predicts it?"*

### Step 2: Log

When user says "log my numbers" or it's L10 day:
- Identify the current ISO week (YYYY-WW)
- For each measurable, prompt the owner: *"{Measurable}: this week's number?"*
- Compare to goal → flag On Track / Off Track per cell
- If 3+ consecutive misses, auto-suggest: *"Add to `EOS Issues List` for IDS?"*

Save in the `EOS Scorecard` note's rolling 13-week table.

### Step 3: Report

Render the 13-week trailing view with:
- Row per measurable
- Columns: owner, goal, last 13 weeks (each cell ✅/❌), trend arrow, sparkline
- Off-track summary

### Step 4: Add Measurable

Add a new row to the existing scorecard. Same fields as Setup. Re-validate count: warn at >15.

### Step 5: Publish (optional)

On `--publish`:
- `taskName: "eos-scorecard-weekly"`
- `category: "research"`
- `content`: the rendered scorecard
- `summary`: *"Week of {date}: {N hit}/{total} on goal · {N off-track} flagged for L10."*

## Data Structure

`EOS Scorecard` note:

```markdown
# EOS Scorecard — {Company Name}

> **Last Updated:** {YYYY-MM-DD}
> **Measurables:** {N} · **Leading:** {N} · **Lagging:** {N}

## Definitions

| Measurable | Owner | Goal | Direction | Type | Source | Department |
|------------|-------|------|-----------|------|--------|------------|
| New Qualified Leads | Sarah | 20 | ↑ higher | leading | CRM | Sales |
| Demos Booked | Sarah | 8 | ↑ higher | leading | Calendar | Sales |
| Cash on Hand | Mark | $250K | ↑ higher | lagging | Bank | Finance |
| …

## 13-Week Trailing

(Numbers in weekly columns; ✅ on goal, ❌ off goal)

| Measurable        | Goal  | W{n-12} | W{n-11} | ... | W{n-1} | W{n} | Trend | Spark |
|-------------------|-------|---------|---------|-----|--------|------|-------|-------|
| New Qualified Leads | 20  | 18 ❌  | 22 ✅  | ... | 19 ❌ | 24 ✅ | ↑    | ▂▅▃▄▂▅▆▃▄▅▆▇ |
| Demos Booked      | 8     | 7 ❌   | 9 ✅   | ... | 6 ❌  | 5 ❌  | ↓    | ▆▇▅▆▄▃▄▃▄▂▃▁ |
| ...

## Off-Track Watch
- ⚠️ Demos Booked — 4 consecutive weeks off goal. Suggest: add to Issues List.
- ⚠️ Cash on Hand — first miss this quarter.
```

## Output Format

### Report

```
📊 EOS Scorecard | {Company} | Week of {YYYY-MM-DD} | 12 measurables

THIS WEEK:  8 on goal ✅  ·  4 off goal ❌
ALERTS:     2 measurables at 3+ consecutive misses ⚠️

(Render the 13-week trailing table.)

OFF-TRACK FOCUS (for L10 IDS):
  ⚠️ Demos Booked — Sarah — 4 weeks ❌ — trend ↓ — sparkline ▆▇▅▆▄▃▄▃▄▂▃▁
  ⚠️ On-Time Delivery — Jen — 3 weeks ❌ — trend ↓
```

### Log Confirmation

```
✅ Week {YYYY-WW} logged.
  New Qualified Leads: 24 (goal 20) ✅
  Demos Booked: 5 (goal 8) ❌  ⚠️ 4th consecutive miss — add to Issues?
  Cash on Hand: $245K (goal $250K) ❌
```

## Example Usage

**User:** "Set up my scorecard. We're a 12-person marketing agency."
**AI:** Coach mode. Suggests measurables by department drawing on Accountability Chart owners. Builds 8 starter measurables. Saves.

**User:** "Log this week's numbers."
**AI:** Walks each measurable, asks for the number. Updates the 13-week table. Surfaces off-track items.

**User:** "What should sales measure?"
**AI:** Returns the Sales row from the coach table + 2 industry-specific suggestions based on the blueprint.

**User:** "Scorecard report."
**AI:** Renders the 13-week trailing view.

**User:** "Add a measurable: weekly Instagram engagement rate, owner Annika, goal 4.5%."
**AI:** Adds row. Pushes back if it's lagging-heavy or if it'd put count >15.

**User:** "/eos-scorecard report --publish"
**AI:** Renders + publishes to dashboard.

## Error Handling

- **<5 measurables defined:** Warn: *"EOS recommends 5-15. With fewer than 5 you'll miss things. Want to add more?"*
- **>15 measurables:** Push back: *"More than 15 dilutes focus. EOS sweet spot is 5-15. Which can you cut?"*
- **>70% lagging:** Warn: *"This is a lagging-heavy scorecard. Leading indicators predict the future; lagging report the past. Want to swap a few?"*
- **Owner not in Accountability Chart:** Confirm: *"{Name} isn't in your accountability chart. Add as owner anyway?"*
- **3+ consecutive misses on a measurable:** Auto-suggest adding to `EOS Issues List`. Do not auto-create without confirmation.
- **User logs a number for a measurable that doesn't exist:** Suggest closest match by name; if no match, offer to add it as a new measurable.
- **User logs the same week twice:** Confirm overwrite: *"Week {YYYY-WW} already logged. Overwrite or skip?"*
- **Goal direction unclear** (e.g., "Open Bugs" — lower is better): Always confirm direction at definition; default to higher-is-better but ask.
- **Sparkline rendering fails** (rare — bad data): Drop the sparkline column; keep the rest of the table.
