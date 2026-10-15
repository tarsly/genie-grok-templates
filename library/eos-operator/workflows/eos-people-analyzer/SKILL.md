---
name: eos-people-analyzer
description: >-
  Score a leader or team member against your EOS Core Values (using +, +/-, -) and GWC (Get it / Want it / Capacity — Yes/No each). Produces an Above The Bar or Below The Bar verdict, tracks the Three Strikes rule, and surfaces patterns across the team. Output is private and does NOT auto-publish to the dashboard. Use for rating the team, assessing if someone is in the right seat, or running a quarterly People Analyzer review.
---

# EOS People Analyzer — Right Person, Right Seat

## Overview

The People Analyzer is the EOS tool for honest team assessment. Two dimensions:
- **Core Values** — rated `+` (consistently demonstrates), `+/-` (sometimes), or `-` (consistently does not) for each value
- **GWC** — three Yes/No fields: Get it (cognitively grasps), Want it (genuinely wants), Capacity (mental/physical/emotional/time/skill)

Wickman's default **Bar**: must be `+` or `+/-` on every core value AND Yes on all three GWC. Anything below = wrong person OR wrong seat. The **Three Strikes** rule: feedback given 3 times with specific expectations; if still below the Bar after 3, they're out.

This skill captures the ratings, computes the verdict, tracks strikes, and surfaces team-wide patterns. Output is sensitive — it does NOT publish to the cloud dashboard.

## When This Skill Applies

- User says "people analyzer", "rate the team", "rate [name]"
- User asks "is [name] above the bar?", "is [name] in the right seat?", "should we keep [name]?"
- User wants a strikes update
- `eos-quarterly-pulsing` calls this in Segment 3 (Team Health) if scheduled
- User invokes `/eos-people-analyzer`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"`, `"eos company preferences"`
2. `search_notes "EOS Core Values"` — required (the analyzer needs them to rate against)
3. `search_notes "EOS Accountability Chart"` — for seat lookup
4. `search_notes "EOS People Analyzer"` — most recent ratings

**Banner:**
```
👥 EOS People Analyzer | {Company} | Q{N} {YYYY} | Core Values loaded: {N}
⚠️ Output is PRIVATE — does not publish to the dashboard.
```

If no `EOS Core Values` note exists: refuse and redirect: *"People Analyzer requires Core Values. Run `/eos-vto-builder section core-values` first."*

## How It Works

### Step 0: Determine Action

Parse to: `rate`, `view`, `team-summary`. Default: `view` if a person specified, `team-summary` otherwise.

### Step 1: Rate

For the named person:

1. Find their seat in `EOS Accountability Chart` (if any). Display it.
2. For EACH core value, ask Y/N twice or just `+/+-/-`:
   - *"{Value name} — does {Person} consistently demonstrate this? `+`, `+/-`, or `-`?"*
   - Capture one sentence of evidence per rating
3. For GWC on their current seat:
   - **Get it** — *"Does {Person} cognitively grasp this seat? Y/N"*
   - **Want it** — *"Do they genuinely want this work? Y/N"*
   - **Capacity** — *"Mental, physical, emotional, time, and skill capacity? Y/N"*
4. Compute verdict:
   - **Above The Bar** — every value `+` or `+/-`, all three GWC Yes
   - **Below The Bar** — any `-` value OR any No on GWC

5. If Below The Bar:
   - Check prior strikes count from most recent People Analyzer note
   - If this is strike 1, 2, or 3 → record
   - At strike 3, surface: *"This is strike 3 for {Person}. Per EOS Three Strikes, time for a decision. Want to draft the conversation?"*

6. Save individual note `EOS People Analyzer — {Person} — {Quarter}`.

### Step 2: View

For the named person, render most recent ratings with evidence and current strikes count.

### Step 3: Team Summary

Read all People Analyzer notes for the current quarter. Render:
- Roster with verdict per person (✅ Above / ⚠️ Below)
- Patterns: e.g., *"3 of 5 leaders are Below The Bar on Core Value 'Customer Obsession'"* — possible value drift OR rating-too-easy on that value
- Strikes outstanding: *"2 leaders at strike 2, 1 leader at strike 3"*

### Step 4: Three Strikes Tracking

Read prior quarter's note. If a person was Below The Bar in 3+ consecutive ratings (different feedback cycles), surface the EOS expectation: *"{Person} has been Below The Bar for {N} reviews. EOS prescribes a hard decision."*

Provide a draft conversation script (referencing *How to Be a Great Boss*):
- Lead with the specific gap (value or GWC)
- Cite the prior feedback rounds
- State the expectation
- Define what success looks like in the next 90 days
- Be clear about consequences if no change

## Data Structure

`EOS People Analyzer — {Person} — {Quarter}` note:

```markdown
# EOS People Analyzer — {Person Name} — Q{N} {YYYY}

> **Date:** {YYYY-MM-DD}
> **Seat:** {seat name from Accountability Chart}
> **Verdict:** {ABOVE / BELOW} the Bar
> **Strikes (cumulative):** {0 / 1 / 2 / 3}

## Core Values

| Value | Rating | Evidence |
|-------|--------|----------|
| {Value 1} | + | Closed Acme deal with full transparency |
| {Value 2} | +/- | Mostly, but skipped client follow-up last month |
| {Value 3} | - | Three missed deadlines this quarter, no proactive comms |
| ...

## GWC ({seat})

| Question | Yes/No | Note |
|----------|--------|------|
| Get it | Yes | Grasps the strategy clearly |
| Want it | Yes | Energized in 1-on-1s |
| Capacity | No | Stretched across 4 seats — can't sustain |

## Verdict

⚠️ **Below The Bar** — `-` rating on Value 3, and `No` on Capacity.

## Action

- **Strike:** 2 of 3
- **Prior feedback:** 2026-04-02, 2026-07-15
- **Expectation for next 90 days:** Consolidate from 4 seats to 2. Demonstrate Value 3 in monthly all-hands. Reassessed at next Quarterly Pulsing.
```

## Output Format

### Single Rating

```
👥 EOS People Analyzer | {Company} | Q3 2026

PERSON: Sarah Chen
SEAT:   Marketing (per Accountability Chart)

CORE VALUES:
  + Authenticity     — Always honest in feedback
  + Customer Focus   — Picks up support tickets unprompted
  +/- Discipline     — Mostly disciplined; missed two L10s
  + Growth Mindset   — Reads weekly, applies it
  + Team First       — Volunteers on others' Rocks

GWC (Marketing seat):
  Get it:    Yes — grasps the brand-narrative shift
  Want it:   Yes — visibly energized
  Capacity:  Yes — at fit

VERDICT: ✅ ABOVE THE BAR  ·  Strikes: 0  ·  Right person, right seat
```

### Team Summary

```
👥 EOS People Analyzer | {Company} | Q3 2026

ROSTER (8 leaders rated this quarter):
  ✅ Sarah    — Marketing       — Above The Bar     — Strikes: 0
  ✅ Mark     — Integrator      — Above The Bar     — Strikes: 0
  ✅ Jen      — Operations      — Above The Bar     — Strikes: 0
  ⚠️ Tom      — Sales           — Below The Bar     — Strikes: 2
  ⚠️ Avery    — Customer Svc    — Below The Bar     — Strikes: 1
  ✅ Casey    — Finance         — Above The Bar     — Strikes: 0
  ✅ Jordan     — Visionary       — Above The Bar     — Strikes: 0
  ✅ Mary     — Co-founder      — Above The Bar     — Strikes: 0

PATTERNS:
  ⚠️ Tom is at strike 2 on "Customer Focus" — decision is approaching
  ⚠️ 3 of 8 leaders rated +/- on Discipline — value drift, or scoring too easy?

NEXT ACTIONS:
  • Schedule Tom's strike-2 feedback conversation
  • Discuss the Discipline pattern at next leadership L10
```

## Example Usage

**User:** "/eos-people-analyzer rate --person Sarah"
**AI:** Walks each Core Value with +/+/-/-, then GWC. Computes verdict. Saves.

**User:** "Is Tom above the bar?"
**AI:** Reads most recent Tom note. Returns verdict + strikes + evidence.

**User:** "Rate the team."
**AI:** Walks each leader from the Accountability Chart one at a time. Saves each.

**User:** "Team summary."
**AI:** Renders the team roster + patterns + next actions.

**User:** "Tom is below the bar again — that's strike 3."
**AI:** Drafts a strike-3 conversation script using the specific gap + prior feedback dates.

**User:** "Publish this to dashboard."
**AI:** Refuses: *"People Analyzer output is private — not safe for the cloud dashboard. Share with the Integrator directly."*

## Error Handling

- **No `EOS Core Values` note:** Refuse. Redirect to `/eos-vto-builder section core-values`. Don't fake values.
- **All ratings `+` for everyone:** Push back: *"All-+ ratings are a red flag for honest assessment. EOS expects discomfort here. Want to revisit anyone?"*
- **GWC all Yes for everyone:** Same push-back. Honest People Analyzers expose at least one No somewhere.
- **No Accountability Chart:** Skip the seat lookup. Ask user: *"What seat is {Person} in?"*
- **`--publish` requested:** Refuse with a clear reason. *"This output is private. Share with the Integrator directly."*
- **Person not found in Accountability Chart and user insists on rating:** Allow but flag: *"{Name} isn't on the Accountability Chart. Adding them temporarily — want to update the chart?"*
- **Strike count disagrees with prior notes:** Trust the prior notes. Surface the discrepancy: *"Prior notes show {N} strikes. You said {M}. Going with {N} unless you correct."*
- **User asks for ratings to be deleted:** Allow (sensitive data) but warn: *"Deleting removes the strikes record. EOS guidance is to keep the record."*
