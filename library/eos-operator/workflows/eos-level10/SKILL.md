---
name: eos-level10
description: >-
  Run your weekly EOS Level 10 (L10) meeting — the 90-minute heartbeat of EOS. Walks the team through the standard agenda with per-segment timing (Segue 5 · Scorecard 5 · Rock Review 5 · Headlines 5 · To-Do List 5 · IDS 60 · Conclude 5), pulls live data from your Scorecard, Rocks, To-Dos, and Issues, captures everything as the meeting runs, and writes the meeting minutes including the cascading message and 1-10 meeting rating. Target ≥8/10 average. Use for prepping an L10, running an L10 live, or capturing notes during/after.
---

# EOS Level 10 Meeting — The 90-Minute Heartbeat

## Overview

The Level 10 (L10) is EOS's core weekly meeting. 90 minutes, same day/time every week, one L10 per team (leadership L10, sales L10, etc.). The agenda is fixed — Segue (5), Scorecard (5), Rock Review (5), Customer/Employee Headlines (5), To-Do List Review (5), IDS (60), Conclude (5). The goal is consistently rating it 8+ out of 10. This skill walks the team through every segment with a running timer, pulls live data from `eos-scorecard` / `eos-rocks` / `eos-todos` / `eos-issues`, drives `eos-ids` for each top-3 issue, and writes the full meeting minutes plus the cascading message.

## When This Skill Applies

- User says "run L10", "start L10", "weekly meeting", "prep my L10", "L10 notes"
- It's the L10 day/time per `eos-company-preferences`
- The meeting is mid-flight and user wants to capture an item
- After the meeting — user wants the cascading message drafted
- User invokes `/eos-level10`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"`, `"eos company preferences"`
2. `search_notes "EOS Scorecard"`
3. `search_notes "EOS Rocks — {current quarter}"`
4. `search_notes "EOS To-Dos"`
5. `search_notes "EOS Issues List"`
6. `search_notes "EOS L10"` then most-recent — for last week's recap

**Banner:**
```
🟢 EOS Level 10 | {Team} | {YYYY-MM-DD HH:MM} | Attendees: {N} | Onboarding: ✓
```

## How It Works

### Step 0: Determine Action

Parse to: `prep`, `run`, `capture`, `recap`. Default: `prep` if before meeting day/time; `run` if at/just after.

### Step 1: Prep — Pre-Meeting Digest

Before the meeting, generate a one-page brief:

- **Scorecard summary** — # on-goal, # off-goal, mini-trend per measurable
- **Rock status** — count by status, off-track Rocks named
- **To-Do completion** — last week's % complete
- **Top 3 Issues** — for IDS
- **Headlines** — none yet; live during meeting

Output as `L10 PRE-MEETING DIGEST` block. The Integrator reads this before the meeting.

### Step 2: Run — Live Meeting Mode

Walk the team through each segment with a timer. Capture inline.

#### Segment 1: Segue (5 min)
Prompt each attendee one at a time: *"{Name} — one personal best and one professional best from the past week. 30 seconds."*

Capture the lines.

#### Segment 2: Scorecard (5 min)
Read `EOS Scorecard`. For each off-goal measurable, prompt the owner: *"{Owner}, {Measurable} was off goal — drop in Issues for IDS, or known and recovering?"*

Off-goal items the owner can't recover same-week → add to `EOS Issues List` Short-Term with source `scorecard`.

#### Segment 3: Rock Review (5 min)
Read `EOS Rocks — {quarter}`. For each Rock owner, prompt: *"{Owner}, Rock '{title}' — On Track or Off Track?"*

Update `eos-rocks` status. Off Track items with 3+ weeks off → flag for Issues per `eos-rocks` rules.

#### Segment 4: Customer/Employee Headlines (5 min)
*"Anyone have a customer or employee headline — good or bad — worth a 30-second share?"*

Capture each headline. Any action items → To-Dos. Any unresolved issues → Issues List.

#### Segment 5: To-Do List Review (5 min)
Call `eos-todos recap`. For each open to-do from last week, mark Done or Not Done. Compute % complete vs 90% target.

#### Segment 6: IDS (60 min)
Pull top-3 Issues from `eos-issues`. For each (allocating ~20 min — but use time-box, often <10 min each):

- Call `eos-ids` for that issue
- Capture root cause, discussion, solve (to-do/rock/decision)
- Close the issue

If team blows through top-3 in <60 min, work the next-ranked issues until time-box hits.

#### Segment 7: Conclude (5 min)
Three steps:
1. **Recap to-dos** — read each new to-do back with owner + due date
2. **Cascading message** — draft 2-3 sentences: *"What does the rest of the company need to know from this meeting?"*
3. **Rate the meeting** — each attendee: 1-10. Compute average vs 8/10 target.

If average <8, capture as an Issue for next L10.

### Step 3: Save — Meeting Minutes

`mcp__cloud-brain__write_note`:
- **title:** `EOS L10 — {YYYY-MM-DD}` (or `EOS L10 — {Team} — {YYYY-MM-DD}` if multiple teams)
- **folder:** `eos`
- **tags:** `["eos","l10","meeting"]`

Use the Data Structure template.

### Step 4: Publish (optional)

On `--publish`:
- `taskName: "eos-l10-recap"`
- `category: "briefing"`
- `content`: the rendered one-pager
- `summary`: *"L10 {date}: rating {N}/10 · scorecard {hit}/{total} · rocks {ot}/{total} on track · todos {done}% · {N} issues IDS'd · {N} new todos."*

### Step 5: Capture Mode

When the user calls `capture` mid-meeting, append the captured item to the appropriate section of the in-progress L10 note. Recognize: "to-do — …", "issue — …", "headline — …", "decision — …".

## Data Structure

`EOS L10 — {YYYY-MM-DD}` note:

```markdown
# EOS L10 — {Date} — {Team}

> **Date:** {YYYY-MM-DD HH:MM}
> **Duration:** 90 minutes (target)
> **Attendees:** {names}
> **Meeting Rating:** {avg}/10 · Individual: {name1: N1, name2: N2, ...}

---

## 1. Segue
- **{Name}:** {personal best} · {professional best}
- …

## 2. Scorecard
- **On Goal:** {N}/{total}
- **Off Goal (to Issues):** {list of measurables added to issues}
- See `EOS Scorecard` for full numbers

## 3. Rock Review
| Owner | Rock | Status | Notes |
|-------|------|--------|-------|
| Sarah | Marketing Mgr hired by 9/30 | ✅ On Track | Interviews scheduled |
| Mark  | $150K new ARR by 9/30 | ⚠️ Off Track | Pipeline thin; → IDS |

## 4. Headlines
- {Customer/Employee headline 1}
- {Customer/Employee headline 2}

## 5. To-Do Review
- Created last week: {N}
- Completed: {N} = {pct}% (target ≥90%)
- Still open: {list with owner}

## 6. IDS
### Issue 1: {one-line}
- **Root Cause:** {one-line}
- **Discussion:** {key points}
- **Solve:** {to-do / rock / decision} — Owner: {name} — Due: {date}

### Issue 2: {one-line}
…

### Issue 3: {one-line}
…

## 7. Conclude

### New To-Dos
| Due | Owner | Action |
|-----|-------|--------|
| {date} | {name} | {verb-led action} |
…

### Cascading Message
{2-3 sentence summary for the rest of the company}

### Meeting Rating
{avg}/10 — Individuals: {breakdown}
```

## Output Format

### Prep (pre-meeting digest)

```
🟢 L10 PRE-MEETING DIGEST | {Team} | {Date} {Time}

SCORECARD
  ✅ 8 on goal  ·  ❌ 4 off goal  ·  ⚠️ 2 at 3+ consecutive misses

ROCKS (Q3 2026, 47 days remaining)
  ✅ 4 on track  ·  ⚠️ 2 off track  ·  🎯 2 done
  ON-TRACK ALERT: 2 off-track Rocks need IDS

TODOS LAST WEEK
  Completed 11/12 = 92% ✅ (above 90% target)

TOP 3 ISSUES FOR IDS
  1. Demos booked off 4 weeks straight (Scorecard)
  2. Mark's ARR Rock at 40% with 47 days left (Rock)
  3. Two key clients renewing — no plan (Headline)

EXPECTED HEADLINES: Sarah mentioned the Acme renewal. Mark mentioned hiring.
```

### Live Meeting Segment Header

```
─── 🕐 SEGUE (5 min) ───────────────────────────
{prompts each attendee one by one}

─── 🕐 SCORECARD (5 min) ───────────────────────
{walks each measurable}
…
```

### Conclude

```
─── 🕐 CONCLUDE (5 min) ────────────────────────

NEW TO-DOS:
  Fri 9/12  Sarah  Call Bob re: renewal
  Fri 9/12  Mark   Send Q3 numbers to board
  Fri 9/12  Jen    Draft new onboarding email
  Fri 9/12  Mark   Hire Marketing Mgr — start outreach

CASCADING MESSAGE (draft):
  "We IDS'd the slowdown in demos this week — root cause was unstaffed RFP work,
  not a pipeline problem. We're hiring a Sales Engineer (new Q3 Rock) and Mark is
  personally taking the second active RFP this week."

MEETING RATING: 8.6 / 10  ✅  (target ≥8.0)
  Sarah: 9 · Mark: 8 · Jen: 9 · Tom: 8 · Avery: 9
```

## Example Usage

**User:** "Prep my L10."
**AI:** Reads all sources. Renders pre-meeting digest.

**User:** "Run L10."
**AI:** Walks all 7 segments with per-segment timer, capturing inline.

**User (mid-meeting):** "Capture — Sarah committed to call Bob about renewal by Friday."
**AI:** Appends to the in-progress L10 note's To-Dos section.

**User (mid-meeting):** "Next issue — demos booked off 4 weeks."
**AI:** Hands off to `eos-ids` for that issue. Returns with the solve. Records.

**User:** "/eos-level10 run --publish"
**AI:** Runs the meeting then publishes the recap.

**User:** "What was last week's rating?"
**AI:** Reads most-recent `EOS L10` note. Returns the rating + drivers.

## Error Handling

- **Pre-meeting data missing (no Scorecard yet, no Rocks set):** Run the meeting anyway. Surface the gap: *"No scorecard set up yet — run /eos-scorecard before next L10 to get the pulse working."*
- **Attendees list empty in prefs:** Ask: *"Who's at this L10? Just for capture."*
- **Time-box hit during IDS top-3:** Warn but keep going on current issue. Cut the next one short if needed. Capture untouched issues as "carry-over" in the minutes.
- **Average rating <8:** Add to Issues List for next week's IDS: *"Meeting rated {N}. EOS target is 8. What would make next week's L10 a 10?"*
- **No top-3 prioritized in Issues:** Call `eos-issues prioritize` inline before IDS segment.
- **User wants to skip a segment:** Allow but log it. Skipping >2 weeks = surface as a discipline issue.
- **Note collision (two L10s same date):** Append team slug: `EOS L10 — Leadership — 2026-09-08` vs `EOS L10 — Sales — 2026-09-08`.
- **Dashboard publish requested but `mcp__cloud-dashboard__publish_report` fails:** Save minutes successfully. Warn about publish failure. Don't lose data.
