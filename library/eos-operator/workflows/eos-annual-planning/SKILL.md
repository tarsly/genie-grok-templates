---
name: eos-annual-planning
description: >-
  Run the 2-day EOS Annual Planning offsite — Day 1 reviews the prior year and refreshes the 3-Year Picture and 1-Year Plan; Day 2 sets Q1 Rocks, IDS's top long-term issues, and finalizes the 1-Year Plan goals. Pulls every artifact (4 quarterly recaps, full V/TO, scorecard 52-week trends, accountability chart, process docs) from cloud-brain so the team can focus on thinking, not data entry. Use for prepping annual planning, running the 2-day offsite, or capturing the recap.
---

# EOS Annual Planning — 2 Days, Once a Year

## Overview

Annual Planning is the biggest meeting on the EOS calendar — 2 full days off-site once a year. Day 1 looks back (year recap), looks forward (refreshed 3-Year Picture, 1-Year Plan), and reconnects the team. Day 2 sets Q1 Rocks, IDS's top long-term issues, and finalizes 1-Year Plan goals. This skill pulls every relevant artifact, walks the 2-day agenda, and writes the annual recap.

## When This Skill Applies

- User says "annual planning", "yearly offsite", "year-end planning", "prep annual"
- It's December (or whatever month the user runs annual per prefs)
- User invokes `/eos-annual-planning`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"`, `"eos company preferences"`
2. `search_notes "EOS V/TO"`
3. `search_notes "EOS Quarterly"` — 4 quarterly recaps from this year
4. `search_notes "EOS Scorecard"` — for 52-week trends
5. `search_notes "EOS Accountability Chart"`
6. `search_notes "EOS Process"` — to review core processes
7. `search_notes "EOS Annual"` — prior annual recap
8. `search_notes "EOS Health Check"` — most recent

**Banner:**
```
📆 EOS Annual Planning | {Company} | Closing {YYYY} → Opening {YYYY+1} | Day {1/2}
```

## How It Works

### Step 0: Determine Action

`prep` / `run-day1` / `run-day2` / `recap`. Default: `prep` before offsite, `run-day1` on day one, `run-day2` on day two.

### Step 1: Prep — Pre-Offsite Workbook

Generate a workbook with:
- **Year-in-Review** — all 4 quarterly recaps summarized (Rock completion, scorecard trends, key learnings, big wins/misses)
- **Rock Completion** for the year — overall %, per-leader %
- **Scorecard 52-week summary** — best/worst measurables, biggest improvements
- **L10 Health** — annual avg rating, to-do completion avg
- **V/TO** — full current Vision + Traction sides
- **Accountability Chart** — current
- **Health Check** — most recent score per component
- **Long-Term Issues** — full list

Save as `EOS Annual Workbook — {Year}` in the `eos` folder.

### Step 2: Day 1 — Look Back, Look Forward (8 hours)

#### Segment 1: Check-In (30 min)
Each attendee: best moment of the year + biggest learning.

#### Segment 2: Year-in-Review (90 min)
Walk through all 4 quarters. Celebrate Done Rocks. Diagnose Not Done patterns. Read 52-week scorecard trends. Capture the year's 3 biggest wins, 3 biggest misses, 3 biggest learnings.

#### Segment 3: Team-Building (60 min)
Outside the EOS toolkit but EOS prescribes it. The skill captures the activity (lunch, walk, exercise — user defines). Just log that it happened.

#### Segment 4: V/TO Vision Refresh (180 min, with breaks)
For each section of the V/TO Vision side:
- Read it aloud
- Discuss: still true?
- Update via `eos-vto-builder section <name>`

Focus areas:
- **Core Values** — usually unchanged (revisit only if a value has been gamed or violated)
- **Core Focus** — usually unchanged
- **10-Year Target** — usually unchanged
- **Marketing Strategy** — review Three Uniques, Proven Process, Guarantee
- **3-Year Picture** — REFRESH — push the future date 3 years forward from today; reset revenue/profit/measurables/picture bullets

Save updated V/TO.

#### Segment 5: 1-Year Plan Refresh (90 min)
- Set the future date (end of next fiscal year)
- Set revenue and profit targets
- Set 3-7 SMART goals for the year (these drive next year's Quarterly Rocks)

#### Segment 6: Day-1 Close (15 min)
Rate the day 1-10. Capture key questions for Day 2.

### Step 3: Day 2 — Set the Course (8 hours)

#### Segment 1: Reconnect (15 min)
Quick re-orient. Share overnight thoughts.

#### Segment 2: Long-Term Issues IDS (90 min)
Read `EOS Issues List` → Long-Term. Prioritize top 5. For each, call `eos-ids`. Decide: solve now, becomes Q1 Rock, stays long-term, or kill.

#### Segment 3: 1-Year Goals — Final (45 min)
Validate yesterday's 1-Year Plan goals against the issues IDS'd. Adjust if needed.

#### Segment 4: Q1 Rock Setting (180 min)
- Brainstorm: *"To deliver on this 1-Year Plan, what must we accomplish in Q1?"* — go wide
- Consolidate → 3-7 Q1 Company Rocks
- Owners
- SMART-validate via `eos-rocks` coach mode
- Save `EOS Rocks — {next year} Q1` index

#### Segment 5: Cascade Plan (45 min)
Draft the State-of-the-Company message + the rollout sequence (when do team leads share with their teams?).

#### Segment 6: Day-2 Close (30 min)
Each attendee:
- One word that describes the offsite
- Rate the 2 days 1-10
- One commitment for the year ahead

### Step 4: Save

`mcp__cloud-brain__write_note`:
- **title:** `EOS Annual — {YYYY}`
- **folder:** `eos`
- **tags:** `["eos","annual","meeting"]`

### Step 5: Publish (optional)

On `--publish`:
- `taskName: "eos-annual-recap"`
- `category: "briefing"`
- `content`: rendered annual recap (refreshed 3-Year Picture, 1-Year Plan, Q1 Rocks, top decisions, cascade message)
- `summary`: *"{Year} closed. {pct}% annual rock completion. New 1-Year Plan: {revenue} / {profit}. {N} Q1 Rocks set. Top decision: {one line}."*

## Data Structure

`EOS Annual — {YYYY}` note:

```markdown
# EOS Annual Planning — {YYYY}

> **Dates:** {start} – {end}
> **Attendees:** {names}
> **Closing:** {prior year} → **Opening:** {new year}
> **Offsite Rating:** {avg}/10

---

## Year-in-Review ({prior year})
- **Annual Rock Completion:** {N}/{total} = {pct}%
- **Q1-Q4 Recap:** {one line per quarter}
- **Year's 3 Biggest Wins:** {list}
- **Year's 3 Biggest Misses:** {list}
- **Year's 3 Biggest Learnings:** {list}
- **Scorecard 52-Week Highlights:** {best + worst measurables}

## V/TO Vision Refresh
- **Refreshed Sections:** {list}
- **New 3-Year Picture (future date {date}):** {key bullets}
- **Notes:** {vision-level shifts}

## 1-Year Plan ({new year})
- **Future Date:** {YYYY-MM-DD}
- **Revenue:** ${X}
- **Profit:** ${X}
- **Goals (SMART):**
  1. {goal}
  2. {goal}
  …

## Long-Term Issues IDS'd
### Issue 1: {one-line}
- Root cause / Discussion / Solve / Conversion (to Rock? to decision? to kill?)

…

## Q1 Rocks ({new year} Q1)
| Owner | Rock | Due |
|-------|------|-----|
| Sarah | {rock} | {date} |
…

## Cascade Plan
- {message draft}
- {rollout sequence — when do team leads share?}

## Offsite Rating
- Day 1 average: {N}/10
- Day 2 average: {N}/10
- Combined: {N}/10
- Commitments for the year ahead: {one per attendee}
```

## Output Format

### Prep workbook excerpt

```
📆 EOS Annual Workbook | {Company} | Closing 2026 → Opening 2027

YEAR-IN-REVIEW
  Rock Completion: 22 of 32 = 69% ⚠️ (target ≥80%)
  Q-by-Q:
    Q1: 7/8 = 88% ✅
    Q2: 5/8 = 63% ⚠️
    Q3: 4/8 = 50% ❌
    Q4: 6/8 = 75% ⚠️
  Pattern: We slip in Q2-Q3. Worth IDS'ing on Day 2.

  Scorecard Best:  New Leads ↑ 43% YoY · NPS ↑ 12 pts
  Scorecard Worst: Cash on Hand → flat (target was up 25%)
  L10 Health: 7.6 avg rating ⚠️ · 84% to-do completion ⚠️

V/TO VISION (current)
  (full V/TO rendered)

LONG-TERM ISSUES (8 candidates for Day 2 IDS)
  1. ...
  ...

HEALTH CHECK (most recent)
  Vision: 80% · People: 70% · Data: 55% · Issues: 75% · Process: 40% · Traction: 65%
  WEAKEST: Process (40%) — Day 2 candidate for a Rock.
```

## Example Usage

**User:** "Prep my annual planning offsite — we're meeting Dec 12-13."
**AI:** Generates the full workbook.

**User:** "Run day 1 of annual planning."
**AI:** Walks Segments 1-6. Calls `eos-vto-builder section <name>` for refreshes. Captures inline. Saves day-1 progress.

**User:** "Run day 2."
**AI:** Walks Segments 1-6. Calls `eos-ids` for each top issue, `eos-rocks add` for new Q1 Rocks. Saves final annual recap.

**User:** "/eos-annual-planning run-day2 --publish"
**AI:** Runs Day 2 then publishes the annual recap to the dashboard.

**User:** "What was our annual rock completion last year?"
**AI:** Reads the prior `EOS Annual — {YYYY}` note and returns the % + one-line lessons.

## Error Handling

- **Fewer than 4 quarterly recaps from this year:** Run with available data. Note gap: *"Only {N} quarterlies on record — some prior data missing."*
- **V/TO doesn't exist:** Allocate extra time on Day 1; run `eos-vto-builder build` end-to-end during Segment 4.
- **No long-term issues:** Skip Day 2 Segment 2 IDS. Note: *"No long-term issues at offsite — good sign."*
- **Day rated <7:** Add as Issue for first Q1 L10: *"Annual rated {N}. What broke and how do we fix for next year?"*
- **User wants to skip Day 1 or Day 2:** Strong push-back. EOS specifies 2 days. *"Compressing this to 1 day usually fails. Keep both days?"*
- **Refreshed 3-Year Picture revenue lower than current 3-Year Picture (regression):** Confirm intentionally — *"You're lowering the 3-Year revenue target from ${X} to ${Y}. Is this a strategic compression or did the prior target prove unrealistic?"* Save with a note in either case.
- **>7 Q1 Rocks proposed:** Refuse — *"More than 7 = nothing is a priority. Cut to 3-7 before saving."*
