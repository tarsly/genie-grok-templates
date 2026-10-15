---
name: eos-rocks
description: >-
  Manage quarterly EOS Rocks — the 3-7 most important priorities every leader commits to for the next 90 days. Add, update, complete, view status, or get coaching to take a vague goal and produce a SMART (Specific, Measurable, Attainable, Relevant, Time-bound) Rock. Targets ≥80% Rock completion per quarter. Use for setting Q rocks, rewriting a vague rock, checking rock status, marking a rock done, or needs to set Q rocks, rewrite a vague rock, check rock status, or mark a rock complete.
---

# EOS Rocks — Your 90-Day Priorities

## Overview

Rocks are the 3-7 most important things each leader will accomplish in the next 90 days. The metaphor is Stephen Covey's: big rocks go in the jar first; sand and water (small tasks) fill in around them. This skill is the CRUD layer for Rocks — add, update, complete, view status — plus a **coach mode** that takes a vague idea ("improve sales") and produces a SMART Rock. Status tracking is binary at quarter end: Done or Not Done. EOS health target: **≥80% completion across all Rocks** every quarter.

## When This Skill Applies

- User wants to set Quarterly Rocks (at Quarterly Pulsing or just starting EOS)
- User says "add a rock", "set my Q rocks", "rewrite this rock"
- User asks "what are my rocks?", "rock status", "how am I doing on rocks"
- User wants to mark a rock On Track / Off Track / Done
- User says "we need a rock for [vague idea]" — triggers coach mode
- `eos-level10` calls this skill during its 5-minute Rock Review
- `eos-quarterly-pulsing` calls this skill to set the next quarter's Rocks
- User invokes `/eos-rocks`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"` — company name, industry, team size
2. `search_notes "goal-hierarchy"` — long-horizon vision (for relevance check)
3. `search_notes "quarterly-priorities"` — pre-existing quarterly commitments from system 01 (seed candidates for Rocks)
4. `search_notes "language-and-frameworks"` — EOS terminology preference
5. `search_notes "eos company preferences"` — saved EOS prefs
6. `search_notes "EOS V/TO"` — for Marketing Strategy + 1-Year Plan (Rocks should align)
7. `search_notes "EOS Rocks — {current quarter}"` — current quarter's rock index

**Reconcile preferences** as in `eos-vto-builder`.

**Determine current quarter** from today's date + fiscal year start in prefs.

**Banner:**
```
🎯 EOS Rocks | {Company} | Q{N} {YYYY} | Days Remaining: {D} | Rocks: {N} | Onboarding: ✓
```

If `quarterly-priorities` exists and no rocks yet: *"I see {N} priorities from your goal-architect session — want me to propose those as your starting Rocks?"*

## How It Works

### Step 0: Determine Action

Parse to: `add`, `update`, `complete`, `status`, `coach`, `list`. Default: `list`.

### Step 1: Coach Mode (vague idea → SMART Rock)

When the user gives a fuzzy input like "improve sales" or "fix marketing":

Ask 4 quick questions in ONE message:
1. **Specific** — What exactly does "done" look like? (e.g., "a hired Marketing Manager onboarded and running campaigns")
2. **Measurable** — What's the binary metric? (e.g., "hired, started, and onboarded by 9/30")
3. **Owner** — Who owns this Rock end-to-end?
4. **Relevant** — Which 1-Year Plan goal does this advance?

Synthesize: *"Here's the SMART version: 'Marketing Manager hired, started, and onboarded by 9/30/2026 — Owner: Sarah.' Sound right? Save as a Rock for Q3?"*

**SMART validation rules** (refuse to save until met):
- Specific — has a clear concrete deliverable, not a continuous activity
- Measurable — done/not done is binary at quarter end
- Attainable — stretch but achievable (push back if owner self-reports <50% confidence)
- Relevant — explicitly ties to a 1-Year Plan goal or 3-Year Picture bullet
- Time-bound — due date within the quarter

Examples to call out:
- BAD: "Improve sales." → No metric, no date.
- GOOD: "Close $150K in new ARR by 9/30/26."
- BAD: "Hire a marketer."
- GOOD: "Marketing Manager hired, started, and onboarded by 9/30/26."

### Step 2: Add Rock

Required: owner, title (SMART), quarter, due date. Optional: 2-4 milestones for tracking.

Create note:
- **title:** `Rock — {Quarter} — {Owner} — {Short Title}` (e.g., `Rock — 2026 Q3 — Sarah — Hire Marketing Manager`)
- **folder:** `eos`
- **tags:** `["eos","rock","on-track","{owner-slug}"]`

Update `EOS Rocks — {Quarter}` index (create if missing) with one row per Rock.

If adding the 8th Rock for a single owner, warn: *"You're at 8 Rocks for {owner}. EOS sweet spot is 3-5. More than 7 and nothing is a priority. Want to consolidate?"*

### Step 3: Update / Complete

`search_notes` with the rock title or owner → `read_note` → update fields:
- **Status:** On Track / Off Track / Done / Not Done
- **Notes:** timestamped progress note
- **Milestones:** check off if hit

Save via `edit_note`. Update tags to reflect new status. Update the `EOS Rocks — {Quarter}` index.

**Auto-escalation:** If a Rock is marked Off Track for 3+ consecutive weeks (read prior L10 minutes via `search_notes "EOS L10"`), surface: *"This Rock has been off-track for {N} weeks. Per EOS, push to the Issues List and IDS root cause."* Optionally auto-create an issue in `EOS Issues List`.

### Step 4: Status / List

Read `EOS Rocks — {current quarter}` index. Render the dashboard.

### Step 5: Quarter-End

Trigger when user says "close the quarter" or invoked from `eos-quarterly-pulsing`:
- For each Rock, ask: *"{Title} — Done or Not Done?"* (EOS rule: "mostly done" = Not Done.)
- Compute completion rate.
- Surface vs 80% target.
- Archive index note: rename to `EOS Rocks — {Quarter} (closed)`.
- Tag closed rocks `["eos","rock","done"]` or `["eos","rock","not-done"]`.

### Step 6: Publish (optional)

On `--publish`:
- `taskName: "eos-rocks-status"`
- `category: "research"`
- `content`: rendered rock dashboard with per-Rock progress bars
- `summary`: *"Q{N} {YYYY}: {N} Rocks · {done}/{total} done · {pct}% (target ≥80%) · {N} off-track."*

## Data Structure

### Individual Rock note

```markdown
# Rock — {Quarter} — {Owner} — {Short Title}

> **Owner:** {name}
> **Quarter:** Q{N} {YYYY}
> **Due Date:** {YYYY-MM-DD}
> **Status:** {on-track / off-track / done / not-done}
> **Created:** {YYYY-MM-DD}
> **Last Updated:** {YYYY-MM-DD}
> **Relevance:** {which 1-Year Plan goal}

## SMART Statement
{One sentence — Specific, Measurable, Attainable, Relevant, Time-bound}

## Milestones
- [ ] {milestone 1} — by {date}
- [ ] {milestone 2} — by {date}
- [ ] {milestone 3} — by {date}

## Status Log
- {YYYY-MM-DD}: {status} — {note}
- {YYYY-MM-DD}: {status} — {note}
```

### Index note

```markdown
# EOS Rocks — {Quarter}

> **Last Updated:** {YYYY-MM-DD}
> **Quarter Start:** {YYYY-MM-DD} · **Quarter End:** {YYYY-MM-DD}
> **Days Remaining:** {N}
> **Total Rocks:** {N} · **On Track:** {N} ✅ · **Off Track:** {N} ⚠️ · **Done:** {N} 🎯

## Dashboard

| Owner | Title | Status | Due | Progress |
|-------|-------|--------|-----|----------|
| Sarah | Marketing Manager hired, started, onboarded | ✅ On Track | 9/30 | `███████░░░ 70%` |
| Mark  | Close $150K new ARR | ⚠️ Off Track | 9/30 | `████░░░░░░ 40%` |
| Jen   | Roll out client onboarding SOP company-wide | 🎯 Done | 8/15 | `██████████ 100%` |

## Completion Forecast
- On pace for {N}/{Total} = {pct}% (target ≥80%)
```

## Output Format

### Status

```
🎯 EOS Rocks | {Company} | Q3 2026 | Days Remaining: 47 | Rocks: 8

OVERALL:  4 ✅  ·  2 ⚠️  ·  2 🎯  ·  0 ❌
FORECAST: On pace for 6/8 = 75% (target ≥80%)  ⚠️ Below benchmark

BY OWNER:
  Sarah  (3 rocks): 2 ✅ · 1 🎯           — 100% pace
  Mark   (2 rocks): 0 ✅ · 2 ⚠️           — 0% pace — RISK
  Jen    (3 rocks): 2 ✅ · 1 🎯           — 100% pace

OFF-TRACK FOCUS (raise at next L10):
  ⚠️ Mark — Close $150K new ARR — 40% with 47 days left
  ⚠️ Mark — Launch enterprise pricing page — 25% with 47 days left
```

## Example Usage

**User:** "I need a rock for fixing our customer churn problem."
**AI:** Coach mode. Asks 4 questions. Synthesizes: *"'Reduce monthly churn from 8% to 5% by 9/30/26 — Owner: Mark.' Add as Q3 Rock?"*

**User:** "/eos-rocks add --owner Sarah --title 'Marketing Manager hired by 9/30' --quarter 2026-Q3"
**AI:** Validates SMART. Creates Rock note + updates index. Confirms.

**User:** "Mark Sarah's marketing rock as on track."
**AI:** Finds the rock, updates status, adds a timestamped note, updates index.

**User:** "Rock status."
**AI:** Renders the dashboard (see Output Format).

**User:** "Mark — close $150K — done. We hit it."
**AI:** Marks Done. Adds final-value note. Updates index. Says: *"🎯 Done. You're now at 5/8 = 62% Done with 47 days left."*

**User:** "Close the quarter."
**AI:** Walks each Rock for Done/Not Done. Computes completion. Archives index. Surfaces lessons learned for `eos-quarterly-pulsing`.

**User:** "/eos-rocks list --publish"
**AI:** Renders dashboard, then publishes to cloud dashboard with category `research`.

## Error Handling

- **User tries to add a Rock with no SMART markers:** Refuse. Trigger coach mode automatically.
- **User adds 8+ Rocks for one owner:** Warn but allow with confirmation. *"More than 7 is a signal you don't have priorities. Want to consolidate to 3-5?"*
- **Rock title duplicates an existing one for the same owner this quarter:** Suggest editing the existing one instead. If the user insists, append a suffix `(2)`.
- **User marks a Rock Done before the due date:** Allow. Celebrate. *"🎯 Done {N} days early. Capture the learning for the next quarterly?"*
- **User tries to set a Rock with a due date outside the current quarter:** Push back: *"Rocks are 90-day priorities. {Due date} is in Q{X}, not Q{current}. Save as a long-term issue instead, or compress scope to fit this quarter?"*
- **No `EOS Rocks — {quarter}` index exists yet:** Create it on first add. Don't error.
- **Off-track for 3+ weeks (auto-detected from L10 history):** Surface and offer to auto-create a matching issue in `EOS Issues List`.
- **User asks to delete a Rock mid-quarter:** Confirm — *"Deleting will lower your completion denominator. EOS recommends keeping the Rock and marking it Not Done at quarter end so you can learn from it. Sure?"*
