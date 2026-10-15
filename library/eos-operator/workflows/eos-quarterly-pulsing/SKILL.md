---
name: eos-quarterly-pulsing
description: >-
  Run a full-day EOS Quarterly Pulsing meeting — the off-site that closes the prior quarter, refreshes the V/TO Vision side, and sets the next quarter's Rocks. Pulls live data from Scorecard trends, Rock completion, prior L10s, and long-term Issues for IDS. Use for prepping a Q-pulse, running the full-day agenda, or capturing the recap afterward.
---

# EOS Quarterly Pulsing — Reset Every 90 Days

## Overview

The Quarterly Pulsing meeting is the off-site, full-day reset that happens at the start of every quarter. The leadership team closes the prior quarter (Rock completion, scorecard trends), reviews the V/TO Vision side, IDS's long-term issues, and sets the next quarter's Company Rocks. This skill walks the full-day agenda, pulling every artifact from cloud-brain so the team isn't re-entering data, then writes the quarterly minutes.

## When This Skill Applies

- User says "quarterly pulse", "Q-pulse", "review the quarter", "set new rocks"
- It's the first business day of a new quarter (per fiscal year start in prefs)
- User invokes `/eos-quarterly-pulsing`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"`, `"eos company preferences"`
2. `search_notes "EOS V/TO"`
3. `search_notes "EOS Rocks — {prior quarter}"` — to recap
4. `search_notes "EOS Scorecard"` — for trends
5. `search_notes "EOS Issues List"` — long-term section
6. `search_notes "EOS L10"` — recent L10 minutes (read 13 of them)
7. `search_notes "EOS Quarterly"` — prior quarterly recap

**Banner:**
```
🗓 EOS Quarterly Pulsing | {Company} | Closing {prior Q} → Opening {new Q} | Attendees: {N}
```

## How It Works

### Step 0: Determine Action

`prep` / `run` / `recap`. Default: `prep` if before the offsite day, `run` if same-day.

### Step 1: Prep — Pre-Meeting Workbook

Generate a workbook with:
- **Prior Quarter Rock Recap** — every Rock with Done/Not Done verdict (close the quarter via `eos-rocks`)
- **Rock Completion %** vs ≥80% target
- **Scorecard Trends** — 13-week view per measurable, identify the 3 strongest and 3 weakest
- **L10 Health** — 13 weeks of meeting ratings, average vs ≥8 target; To-Do completion 13-wk avg
- **V/TO Vision** — current Core Values, Core Focus, 10-Year Target, Marketing Strategy, 3-Year Picture
- **Long-Term Issues** — list

Save as `EOS Quarterly Workbook — {Quarter}` in the `eos` folder for distribution before the offsite.

### Step 2: Run — Full-Day Agenda

Walk the standard EOS Quarterly Pulsing agenda. Time-box each segment.

#### Segment 1: Check-In (15 min)
Each attendee shares: best of last quarter (personal + professional), expectations for today.

#### Segment 2: Review Previous Quarter (60 min)
- Walk every Rock — Done / Not Done with one-line lessons
- Compute completion % vs 80%
- Walk scorecard top-3 wins / top-3 misses
- Walk L10 health (rating + to-do completion averages)
- Capture key learnings

#### Segment 3: Team Health (45 min)
- Quick read on team mood / dynamics
- Run `eos-people-analyzer` snapshot if scheduled (separate skill)
- Capture team-health issues into Long-Term Issues if needed

#### Segment 4: V/TO Review (90 min)
- Read each section of the V/TO Vision side aloud
- Discuss: still true? Need to refresh anything?
- Refresh sections as needed via `eos-vto-builder section <name>`
- Save updated V/TO

#### Segment 5: Lunch (60 min)
Just lunch. EOS prescribes a real break.

#### Segment 6: EOS Tools Review (30 min)
Rate each tool the team uses (Scorecard, Rocks, L10, IDS, V/TO, Accountability Chart) on use + effectiveness 1-5. Surface what's weak.

#### Segment 7: IDS Long-Term Issues (90 min)
Read `EOS Issues List` → Long-Term section. Prioritize top issues. For each, call `eos-ids`. Decide what becomes a new Rock vs what stays on the list.

#### Segment 8: Set Next Quarter's Rocks (90 min)
- Brainstorm: *"What MUST we accomplish in the next 90 days?"* — go wide, capture all ideas
- Consolidate → 3-7 Company Rocks
- Assign owners
- SMART-validate each via `eos-rocks` coach mode
- Save the new `EOS Rocks — {next quarter}` index

#### Segment 9: Cascade and Close (30 min)
- Draft the State-of-the-Company message for the rest of the team
- Rate the day 1-10
- Capture what to do differently next quarter

### Step 3: Save

`mcp__cloud-brain__write_note`:
- **title:** `EOS Quarterly — {YYYY QN}`
- **folder:** `eos`
- **tags:** `["eos","quarterly","meeting"]`

### Step 4: Publish (optional)

On `--publish`:
- `taskName: "eos-quarterly-recap"`
- `category: "briefing"`
- `content`: rendered recap with rock-completion donut, scorecard trend grid, new Rocks list
- `summary`: *"Q{prior} closed at {pct}% rock completion. {N} new Rocks set for Q{new}. Top 3 long-term issues IDS'd."*

## Data Structure

`EOS Quarterly — {YYYY QN}` note:

```markdown
# EOS Quarterly — {YYYY QN}

> **Date:** {YYYY-MM-DD}
> **Attendees:** {names}
> **Closing:** Q{prior} {YYYY} → **Opening:** Q{new} {YYYY}
> **Day Rating:** {avg}/10

---

## 1. Check-In Highlights
- {name}: {highlights}
- …

## 2. Previous Quarter Review
- **Rock Completion:** {N}/{total} = {pct}% (target ≥80%)
- **Done Rocks:** {list}
- **Not Done Rocks:** {list with one-line lesson each}
- **Scorecard Top Wins:** {3 measurables}
- **Scorecard Top Misses:** {3 measurables}
- **L10 Avg Rating:** {N}/10 across 13 weeks
- **To-Do Completion Avg:** {pct}% across 13 weeks

## 3. Team Health
- {notes}

## 4. V/TO Review
- **Sections Refreshed:** {list}
- **Notes:** {any vision-level shifts}

## 5. EOS Tools Health
| Tool | Use | Effectiveness | Notes |
|------|-----|---------------|-------|
| Scorecard | 5 | 4 | Need leading-indicator overhaul |
| Rocks | 5 | 3 | Too many off-track — too vague at start |
| ...

## 6. Long-Term Issues IDS'd
### Issue 1: {one-line}
- Root cause / Discussion / Solve

### Issue 2: …

## 7. New Quarterly Rocks (Q{new} {YYYY})
| Owner | Rock | Due |
|-------|------|-----|
| Sarah | Marketing Mgr hired by {date} | {date} |
| Mark  | $150K new ARR by {date} | {date} |
| …

## 8. State-of-the-Company Message (draft)
{2-paragraph cascade for full team}

## 9. Day Rating
- Average: {avg}/10
- Individuals: {breakdown}
- What we'd do differently: {bullets}
```

## Output Format

### Prep workbook

A printable pre-read with:
```
🗓 EOS Quarterly Workbook | {Company} | Q{prior} → Q{new}

CLOSING THE QUARTER
  Rock Completion: 6 of 8 = 75% ⚠️ (target ≥80%)
    ✅ Done:     Sarah's Marketing hire · Jen's SOP rollout · ...
    ❌ Not Done: Mark's $150K ARR (closed $98K) · Avery's price page
  Scorecard 13-Wk Trends:
    BEST:  New leads ↑ · NPS ↑ · On-time delivery ↑
    WORST: Demos booked ↓ · Cash on hand → · Pipeline value ↓
  L10 Health:
    Avg rating: 7.8 ⚠️ (target ≥8.0)
    To-Do completion: 87% ⚠️ (target ≥90%)

V/TO VISION (current)
  Core Values: ...
  Core Focus: ...
  10-Year Target: ...
  Marketing Strategy: ...
  3-Year Picture: ...

LONG-TERM ISSUES (5 to consider for IDS)
  1. Replace QBO before cap
  2. New office lease (June 2027)
  3. ...

YOUR JOB BEFORE THE OFFSITE: Read this. Think about new Rocks. Bring your top 3 brainstorm.
```

## Example Usage

**User:** "Prep my Q-pulse for next Tuesday."
**AI:** Generates the full workbook, saves it, displays a preview.

**User:** "Run quarterly pulsing."
**AI:** Walks the full-day agenda segment by segment, calling `eos-rocks close`, `eos-vto-builder refresh`, `eos-ids`, and `eos-rocks add` as needed.

**User:** "Review prior quarter."
**AI:** Just Segment 2 — Rock recap + scorecard + L10 health.

**User:** "/eos-quarterly-pulsing run --publish"
**AI:** Runs the meeting, then publishes recap to dashboard.

**User:** "What were our learnings last quarter?"
**AI:** Reads most-recent `EOS Quarterly` note and returns Section 2 + the "what we'd do differently" bullets.

## Error Handling

- **No prior `EOS Quarterly` note (first time running):** Run without comparison data. Skip prior-quarter recap. Note in the minutes: *"First Quarterly Pulsing — no prior comparison."*
- **`EOS Rocks — {prior quarter}` not closed:** Auto-call `eos-rocks close-quarter` before Segment 2. Don't skip.
- **V/TO doesn't exist:** Run `eos-vto-builder build` inline during Segment 4.
- **Long-Term Issues empty:** Skip Segment 7 IDS — note in minutes: *"No long-term issues this quarter."*
- **>7 new Rocks proposed for next quarter:** Push back: *"More than 7 dilutes focus. Pick 3-7."*
- **Day rating <7:** Surface as an Issue for next quarter: *"Day rated {N}. What broke today and what would make next quarter's a 10?"*
- **Workbook can't be saved (cloud-brain unavailable):** Render in-line but warn the user it's not persisted. Retry on user signal.
