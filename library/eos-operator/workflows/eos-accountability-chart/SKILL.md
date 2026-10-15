---
name: eos-accountability-chart
description: >-
  Build, view, and update your EOS Accountability Chart — functional seats (not titles), top-5 roles per seat, the Visionary/Integrator structure, GWC (Get it / Want it / Capacity) ratings per seat, and staffing tags (human / AI / mixed / open). Hands off open or AI-eligible seats to /agent-designer. Use for org chart design, seat definition, right person / right seat decisions, GWC assessments, or needs to define seats, assign people, run a GWC assessment, or hand off open seats to the agent designer.
---

# Accountability Chart — Right People, Right Seats

## Overview

The Accountability Chart replaces the traditional org chart. Every box is a **functional seat** (not a title) with 3-5 top roles and exactly one accountable person. The top is always Visionary + Integrator. This skill builds the chart, adds/edits seats, assigns people, GWC-rates each seat (Gets it / Wants it / Capacity), and tags whether the seat is staffed by a human, an AI agent, a mix, or open. When you have open or AI-eligible seats, the skill hands them off to `/agent-designer` so your EOS structure and your AI agent team stay in sync.

## When This Skill Applies

- User wants to build their accountability chart for the first time
- User says "add a seat", "place [name] in the [seat] seat", "is [name] in the right seat?"
- User asks "show our org chart", "who has the [role] seat?", "what are the open seats?"
- User wants to GWC a seat or person
- User asks about Visionary, Integrator, or the Rocket Fuel V/I pairing
- User wants to design AI agents for open seats (hands off to `/agent-designer`)
- User invokes `/eos-accountability-chart`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"` — extract company name, industry, team size
2. `search_notes "eos company preferences"` — saved EOS prefs
3. `search_notes "EOS Accountability Chart"` — existing chart
4. `search_notes "Agent Team"` — existing AI agent team (from `ai-agents` plugin)
5. `search_notes "EOS Core Values"` — values used in any GWC discussion

**Reconcile:** Same pattern as `eos-vto-builder`. Confirm prefs in one message if loading from blueprint.

**Banner:**
```
🎯 EOS Accountability Chart | {Company Name} | Team Size: {N} | Onboarding: ✓ loaded
```

If an Agent Team note exists, also show: *"Found {N} active AI agents — I'll mark those seats with 🤖."*

## How It Works

### Step 0: Determine the Action

Parse to: `build`, `view`, `add-seat`, `place-person`, `gwc`, `handoff`. Default to `view` if a chart exists, `build` if not.

### Step 1: Build Mode

If no chart exists, propose a starter chart by team size:

| Team Size | Starter Seats |
|-----------|---------------|
| 1-5 | Visionary, Integrator, Sales/Marketing, Operations, Finance (often one person in multiple) |
| 6-15 | Visionary, Integrator, Sales, Marketing, Operations, Finance |
| 16-25 | + Customer Service, HR, Product/Engineering |
| 25+ | Custom — usually splits Sales/Marketing/Ops further |

Walk each seat:
1. Seat name (functional, e.g., "Sales" not "VP of Sales")
2. Top 5 roles
3. Person assigned (or "OPEN")
4. Staffing tag: `human` | `ai` | `mixed` | `open`
5. GWC (if a person is assigned): Get it (Y/N), Want it (Y/N), Capacity (Y/N)

**Auto-flag from Agent Team:** When reading `Agent Team`, match agent roles to seat names (e.g., agent "Marketing Agent" → seat "Marketing"). Pre-fill `staffing: ai` and the person field with the agent persona name. Mark with 🤖 in the rendered chart.

### Step 2: View Mode

`read_note` → render as indented tree:

```
👁  Visionary            — {name}             [staffing: human]
🎯  Integrator           — {name}             [staffing: human]
├── 💰 Sales             — {name}             [staffing: human]  GWC: Y/Y/Y
├── 📣 Marketing         — {agent name} 🤖    [staffing: ai]
├── 🛠  Operations        — {name}             [staffing: human]  GWC: Y/Y/N ⚠️
├── 🧮 Finance           — OPEN 🔓            [staffing: open]
└── 👥 Customer Service  — {name} + {agent} 🤖 [staffing: mixed]
```

### Step 3: Add Seat

Capture: name (functional), top-5 roles, where it reports, person (or OPEN), staffing tag. Save via `edit_note`.

### Step 4: Place Person

Update an existing seat's `person` and `staffing`. Prompt for GWC if human/mixed.

### Step 5: GWC

For a seat with a human, ask three Y/N questions:
- **Get it** — Do they cognitively grasp the role?
- **Want it** — Do they genuinely want this work?
- **Capacity** — Mental, physical, emotional, time, skill capacity?

Save ratings. Any "No" = wrong seat for them. Surface as a finding: *"⚠️ {name} is below GWC on Capacity for {seat}. EOS says one No = wrong seat. Want to discuss?"*

### Step 6: Handoff to /agent-designer

When the chart has any `staffing: open` or `staffing: ai-eligible` seats, offer:

*"You have {N} open or AI-eligible seats. Want to design AI agents for them? I'll hand off to `/agent-designer` pre-loaded with these seats."*

On yes:
1. Build a list of candidate seats: name, top-5 roles, recommended persona based on agent-designer's starting-lineup list (Executive Assistant, Marketing Agent, Sales Agent, Customer Support, Project Manager, etc.)
2. `write_note`:
   - **title:** `EOS Accountability Chart — Agent Candidates`
   - **folder:** `eos`
   - **tags:** `["eos","accountability","agent-candidate-seats"]`
3. Tell the user: *"Saved {N} candidate seats. Run `/agent-designer` and it'll pick these up automatically."*

### Step 7: Save

`mcp__cloud-brain__write_note` (or `edit_note`):
- **title:** `EOS Accountability Chart`
- **folder:** `eos`
- **tags:** `["eos","accountability","org-chart"]`

### Step 8: Publish (optional)

On `--publish`:
- `taskName: "eos-accountability-chart"`
- `category: "briefing"`
- `content`: rendered tree + GWC table + open-seats list
- `summary`: *"{Total seats}. {Filled}/{Total} filled. {N open}. {N below GWC}. {N AI-staffed}."*

## Data Structure

`EOS Accountability Chart` note:

```markdown
# EOS Accountability Chart — {Company Name}

> **Last Updated:** {YYYY-MM-DD}
> **Total Seats:** {N} · **Filled:** {N} · **Open:** {N} · **Below GWC:** {N} · **AI-Staffed:** {N}

## Org Tree

(Indented tree as in Output Format)

## Seats

### Visionary
- **Person:** {name}
- **Staffing:** human
- **Top 5 Roles:**
  1. {role}
  2. {role}
  3. {role}
  4. {role}
  5. {role}
- **GWC:** {Get: Y/N} / {Want: Y/N} / {Capacity: Y/N}
- **Reports To:** —

### Integrator
…

(One H3 section per seat)
```

## Output Format

### View

```
🎯 EOS Accountability Chart | {Company} | Team Size: {N} | Onboarding: ✓ loaded

Total Seats: 8  ·  Filled: 6  ·  Open: 1 🔓  ·  Below GWC: 1 ⚠️  ·  AI-Staffed: 2 🤖

(Indented tree)

OPEN SEATS:
  🔓 Finance — needs hire by {date}

BELOW GWC:
  ⚠️ Operations — {Name} is "No" on Capacity. EOS says one No = wrong seat.

NEXT ACTIONS:
  • Hand off open seats to /agent-designer? (Y/n)
  • Discuss the Operations GWC finding with the Integrator
```

## Example Usage

**User:** "Build my accountability chart. We're a 12-person agency."
**AI:** Proposes the 6-15 starter chart. Walks each seat: name, top-5 roles, person, GWC. Saves. Renders tree.

**User:** "Add a Customer Success seat below the Integrator."
**AI:** Captures top-5 roles. Asks who's in the seat. Updates chart.

**User:** "/eos-accountability-chart gwc --seat Marketing --person Sarah"
**AI:** Walks Sarah through GWC for the Marketing seat. Saves. If any "No" — flags it.

**User:** "We just hired a Marketing Agent — show our chart."
**AI:** Reads `Agent Team`. Auto-flags the Marketing seat as `staffing: ai` with the agent persona name. Renders tree with 🤖.

**User:** "Hand off our open seats."
**AI:** Writes `EOS Accountability Chart — Agent Candidates`. Tells user to run `/agent-designer`.

**User:** "Is Marcus in the right seat?"
**AI:** Reads chart, finds Marcus, displays his seat + roles + GWC. If GWC has a No, surfaces the conversation.

## Error Handling

- **No Visionary or Integrator seat defined:** Push back: *"Every accountability chart has a Visionary and an Integrator at the top. Sometimes one person sits in both — that's fine for early-stage. Who is your Visionary? Your Integrator?"*
- **Same person in 4+ seats:** Warn: *"{Name} is in 4 seats. That's a sign you need to hire — or you're building a structure that depends on a single point of failure."*
- **GWC scored Yes/Yes/Yes for everyone:** Push back: *"All-yes ratings are a red flag for honest assessment. The People Analyzer is meant to surface uncomfortable truths. Want to run /eos-people-analyzer for a deeper look?"*
- **User adds a title-flavored seat name** (e.g., "VP of Sales"): Coach the rename: *"EOS seats are functional, not titular. 'Sales' is the seat; 'VP of Sales' is the title someone in that seat might hold. Want me to use 'Sales'?"*
- **User assigns an AI agent name to a seat but `Agent Team` doesn't list it:** Save with `staffing: ai` and tag `["unverified-agent"]`. Suggest running `/agent-designer` to make the agent real.
- **`Agent Team` note exists but no seats match by name:** Don't auto-link. Tell the user: *"Found {N} agents in your team but no matching seat names. Want to add seats for them?"*
- **User asks to delete the Visionary or Integrator seat:** Refuse, explain why: *"EOS requires both Visionary and Integrator seats — even if one person sits in both. Want to change who's in the seat instead?"*
