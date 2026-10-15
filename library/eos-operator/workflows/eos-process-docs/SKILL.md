---
name: eos-process-docs
description: >-
  Document a Core Process at the EOS 'FBA' (Followed By All) 20% level — the Pareto 20% of steps that drive 80% of the outcome. Walks through major steps (7-12) and the sub-steps that matter, producing a 1-3 page '[Company] Way' document per process. Typical core processes: HR, Marketing, Sales, Operations, Accounting, Customer Retention. Use for documenting any core process, reviewing existing process docs, or listing what's documented vs missing.
---

# EOS Core Process Docs — Followed By All

## Overview

The EOS Process Component says: document the 20% of each core process that drives 80% of the outcome — then have it Followed By All (FBA). Not a 100-page SOP. A 1-3 page checklist that any new hire can pick up and execute. EOS lists typical core processes: HR (hire/fire/review/reward/recognize), Marketing, Sales, Operations (often split by function), Accounting, Customer Retention. This skill walks an owner through the Three-Step Process Documenter pattern and produces a `[Company] Way` document per process.

## When This Skill Applies

- User says "document my sales process", "build the HR FBA", "write a core process"
- User asks "what processes do I have documented", "what processes are missing"
- `eos-quarterly-pulsing` or `eos-annual-planning` reviews process docs
- User invokes `/eos-process-docs`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"`, `"eos company preferences"`
2. `search_notes "EOS Process"` — existing documented processes
3. `search_notes "EOS Accountability Chart"` — to suggest owner of each process

**Banner:**
```
📋 EOS Process Docs | {Company} | Documented: {N} · Missing: {N from typical 6}
```

## How It Works

### Step 0: Determine Action

Parse to: `document`, `view`, `list`, `review`. Default: `list`.

### Step 1: List

Show what's documented vs what's typically expected:

| Process | Documented? | Owner | Last Reviewed |
|---------|-------------|-------|---------------|
| HR | ❌ Missing | — | — |
| Marketing | ✅ Yes | Sarah | 2026-04-15 |
| Sales | ✅ Yes | Mark | 2026-07-02 |
| Operations | ❌ Missing | — | — |
| Accounting | ✅ Yes | Mark | 2026-01-10 |
| Customer Retention | ❌ Missing | — | — |

Surface missing ones with: *"You're missing {N} of the 6 typical core processes. Want to start with the highest-leverage one?"*

### Step 2: Document — The Three-Step Pattern

For the named process:

#### Step 2.1: Identify Major Steps (7-12)
Ask: *"At the highest level, what are the major steps of your {process} process? Aim for 7-12. We'll detail later."*

Examples (Sales process):
1. Lead capture
2. Qualification
3. Discovery call
4. Demo
5. Proposal
6. Negotiation
7. Close
8. Handoff to Onboarding

Push back if >15 steps: *"More than 15 is too much. Group some — what's the chunk that matters?"*

#### Step 2.2: Identify the 20% Per Major Step
For each major step, ask: *"What are the 2-5 sub-steps in {Step} that actually drive 80% of the outcome? Skip the obvious. We want the 20% that matters."*

Example for "Demo":
- Send pre-call brief 24 hrs ahead
- Open with the prospect's #1 stated outcome
- Demo only what they asked for + one "wow" tied to it
- Confirm next step before ending call
- Send recap email within 2 hours

#### Step 2.3: Compile to "[Company] Way"
Produce a 1-3 page document. Add a header for company name, owner, review cycle.

### Step 3: Save

`mcp__cloud-brain__write_note` (or `edit_note`):
- **title:** `EOS Process — {Process Name}`
- **folder:** `eos`
- **tags:** `["eos","process","fba"]`

### Step 4: Review

Annual review trigger from `eos-annual-planning`:
- For each documented process, ask the owner: *"Still followed by all? Any sub-steps that drift or no longer matter?"*
- Update as needed
- Bump `Last Reviewed` date

### Step 5: Publish (optional)

On `--publish`:
- `taskName: "eos-process-docs-list"`
- `category: "briefing"`
- `content`: list of all processes with status + last-reviewed dates
- `summary`: *"{Documented}/{Total typical} core processes documented. {N} reviewed in last 90 days."*

## Data Structure

`EOS Process — {Process Name}` note:

```markdown
# EOS Process — {Process Name} — {Company} Way

> **Owner:** {name}
> **Status:** Active · FBA: Yes
> **Created:** {YYYY-MM-DD}
> **Last Reviewed:** {YYYY-MM-DD}
> **Review Cycle:** Annual

---

## Why This Process Exists
{One paragraph — what outcome it produces}

## Major Steps (the 20%)

### 1. {Major Step Name}
**Outcome:** {what success looks like for this step}
**Sub-Steps (the 20%):**
- {sub-step 1}
- {sub-step 2}
- {sub-step 3}
**Owner:** {role/seat from Accountability Chart}
**Tools:** {systems used}

### 2. {Major Step Name}
…

(7-12 total)

## What This Process Does NOT Do
{Boundaries — what's intentionally out of scope}

## Common Failure Modes
- {failure mode + how we catch it}
- {failure mode + how we catch it}

## Followed By All
This process is FBA — every member of {Team / Department} follows it. Variance requires {Owner} approval.
```

## Output Format

### List

```
📋 EOS Process Docs | {Company} | Documented: 3 · Missing: 3

DOCUMENTED:
  ✅ Marketing      — Sarah   — Reviewed 2026-04-15  (159 days ago) ⚠️ overdue
  ✅ Sales          — Mark    — Reviewed 2026-07-02  (79 days ago) ✅
  ✅ Accounting     — Mark    — Reviewed 2026-01-10  (262 days ago) ⚠️ overdue

MISSING (typical EOS core processes):
  ❌ HR              — no doc — recommended next
  ❌ Operations      — no doc
  ❌ Customer Retention — no doc

REVIEWS OVERDUE: 2 — Marketing, Accounting

NEXT ACTION SUGGESTION:
  Start with HR — highest leverage for a team growing through hiring.
```

## Example Usage

**User:** "Document my sales process."
**AI:** Walks the 3-step pattern: 7-12 major steps → 2-5 sub-steps each → compile. Saves as `EOS Process — Sales`.

**User:** "What processes do I have?"
**AI:** Renders the list.

**User:** "Show me the sales process."
**AI:** Reads `EOS Process — Sales` and renders.

**User:** "Review our marketing process — it hasn't been updated in 5 months."
**AI:** Reads existing doc. Walks the owner through each major step asking "still followed?". Updates and bumps Last Reviewed.

**User:** "/eos-process-docs document --process HR"
**AI:** Walks HR-specific 3-step (hire/fire/review/reward/recognize sub-streams).

**User:** "What process should I document next?"
**AI:** Renders missing list with one-line suggestions per process and recommends the highest-leverage given the business blueprint.

## Error Handling

- **<7 major steps:** Push back: *"Fewer than 7 usually means you're missing chunks. Walk through end-to-end once more — what happens between {Step N} and {Step N+1}?"*
- **>15 major steps:** Push back: *"Too detailed — this is no longer the 20%. Group sub-streams."*
- **Sub-step list is the full workflow (>10 per step):** Push back: *"That's the full SOP — we want the 20% that matters."*
- **Owner not in Accountability Chart:** Confirm: *"{Name} isn't in the chart. Add as owner anyway?"*
- **Process name conflicts with existing doc:** Suggest editing the existing one. If user insists on a new one, append `(v2)`.
- **Annual review skipped 12+ months:** Flag aggressively: *"Marketing process hasn't been reviewed in 14 months — likely drifted. Schedule a review block?"*
- **User wants to delete a process doc:** Confirm: *"Deleting an FBA doc means it stops being Followed By All. Sure? Archive instead?"*
