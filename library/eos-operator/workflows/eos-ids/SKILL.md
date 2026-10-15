---
name: eos-ids
description: >-
  Conversational IDS facilitator — drives an issue through Identify (root cause), Discuss (open candid, one-pass speaking), and Solve (concrete next action with one owner). Spawns a To-Do, Rock, decision, or escalation on solve. Use when the user says 'IDS this', 'we have an issue with X', 'drill into this', 'solve this', or any request to work an issue to root cause. Called by /eos-level10 during the 60-minute IDS segment.
---

# IDS — Identify, Discuss, Solve

## Overview

IDS is EOS's single most-used tool inside an L10: walk an issue from headline → root cause → permanent solve. Most teams discuss symptoms forever and "solve" with no owner. This skill prevents that. Three phases, in order: **Identify** (peel layers until you hit root cause), **Discuss** (open, candid, everyone speaks once before anyone speaks twice — no solutioning yet), **Solve** (concrete next action: To-Do, Rock, decision, or escalation, with exactly one owner).

## When This Skill Applies

- User says "IDS this", "let's solve this", "drill into this", "work this issue"
- User says "we have an issue with [X]" — automatically enters Identify
- `eos-level10` calls during the 60-minute IDS segment for each top-3 issue
- User invokes `/eos-ids`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"`, `"eos company preferences"`
2. `search_notes "EOS Issues List"` — to pick which issue to work
3. `search_notes "EOS V/TO"` — to check Core Values for tie-ins (if the issue is people-related)

**Banner:**
```
🔧 EOS IDS | Issue: {one-line} | Time-box: {N} min
```

## How It Works

### Step 0: Pick the Issue

If invoked with `--issue` text or `--from-list <issue-id>`, use it. Otherwise:
- If `EOS Issues List` exists, show top-3 and ask which to work
- If no list, ask: *"What's the issue? One sentence."*

### Step 1: IDENTIFY — Get to the Root Cause

Phase goal: separate the headline from the real issue. The headline is rarely the real problem.

Ask up to 3 layered questions, one at a time, in tight, direct language:
1. *"What's actually happening? Be specific."*
2. *"What's underneath that? Why is this happening?"*
3. *"And what's underneath THAT? What's the real root cause?"*

Continue until the team agrees on the root cause. Stop when:
- The team says "that's it" or "now we're at the root"
- 3 questions deep with no new layer surfaces — pause, ask: *"Are we at the root, or are we avoiding something?"*

Capture the root cause one-line.

**Common patterns to surface:**
- Headline: "Sales are down" → Root: "Lost our biggest rep, haven't replaced"
- Headline: "Team is burned out" → Root: "Sales committed to delivery dates Ops never agreed to"
- Headline: "Customer complaints up" → Root: "Onboarding skipped Step 4 since the SOP change"

### Step 2: DISCUSS — Open and Candid

Phase goal: every voice heard, no solutioning yet.

Rules to enforce:
- *"Everyone speaks once before anyone speaks twice."*
- *"No 'yes-buts' — just say what you see."*
- *"No solutions yet. Just discuss the root."*

For an interactive facilitation, prompt each attendee by name (from prefs leadership roster). Capture each comment.

When all voices heard OR the time-box (default 7-10 min per issue) is hit, move to Solve.

### Step 3: SOLVE — Concrete, with One Owner

Phase goal: a single, specific action that permanently kills the issue.

Ask: *"What's the solve? It needs to be one of:*
- *A 7-day To-Do (action item)*
- *A 90-day Rock (significant initiative)*
- *A decision (clear yes/no/policy)*
- *An escalation (Visionary / board / external)*

*Whatever it is, ONE owner."*

Validate:
- Concrete (verb-led, specific, measurable)
- Single owner (named person, not a department)
- Has a due date (7 days for To-Do, 90 for Rock, immediate for decision)

If the solve is fuzzy ("we should improve the handoff"), refuse: *"That's not a solve, that's a wish. What action makes that real, by when, and who owns it?"*

### Step 4: Write Through

On Solve, spawn the right downstream note:
- **To-Do:** Call `eos-todos` add semantics — write/edit `EOS To-Dos` note
- **Rock:** Call `eos-rocks` add semantics — create a new Rock note (validate SMART first)
- **Decision:** Log in the meeting minutes (the L10 or quarterly note) under Decisions
- **Escalation:** Add to long-term issues list with `[ESCALATED]` tag

Close the source issue in `EOS Issues List`:
- Mark as closed
- Record resolution type + owner + outcome
- Move to `## Closed` section

### Step 5: Confirm and Move On

Output a 3-line recap:
```
✅ Solved: {issue}
   Root cause: {one-line}
   Action: {to-do / rock / decision} — {owner} — by {date}
```

For L10 use, hand control back to `eos-level10` to pick the next issue.

## Data Structure

IDS does not save its own note — it writes through to `EOS To-Dos`, `EOS Rocks`, or the active meeting minutes note. It does update `EOS Issues List` to close the source issue with the resolution recorded.

Optional: if invoked outside an L10 (standalone), write an `EOS IDS — {YYYY-MM-DD-HHMM} — {short-slug}` note in the `eos` folder, tagged `["eos","ids","standalone"]`, containing the full Identify/Discuss/Solve transcript.

## Output Format

```
🔧 EOS IDS | Issue: Demos booked off 4 weeks straight

─── IDENTIFY ───
Q: What's actually happening?
   "Sarah's demo count dropped from 8/wk to 4/wk starting late June."

Q: What's underneath that?
   "She's been pulled into RFP responses for two big enterprise deals."

Q: And underneath THAT?
   "We never staffed RFP work — it falls on Sarah by default. ← ROOT"

─── DISCUSS ───
  Mark: "Agreed — we said we'd hire a sales engineer in Q2. Slipped."
  Jen:  "RFPs eat 8-10 hours each. Two RFPs = a full week of Sarah."
  Sarah: "I can't do both. Need a decision."

─── SOLVE ───
  Action: Hire a Sales Engineer by 9/30/26 → NEW ROCK (Mark owns)
  Stopgap: RFP #2 handed to Mark personally → TO-DO (Mark, due Friday)

  ✅ Solved.
     Issue: Demos booked off 4 weeks
     Root cause: No RFP staffing — falls to Sarah by default
     Actions:
       • Rock: Sales Engineer hired by 9/30 — Mark
       • To-Do: Mark takes RFP #2 by Friday — Mark
```

## Example Usage

**User:** "IDS this: our team is burned out."
**AI:** Walks Identify (3 layers) → finds root ("Sales commits dates Ops never agrees to"). Discusses. Solves with a To-Do for a weekly Sales/Ops sync + a decision (Ops has veto on delivery dates).

**User:** "/eos-ids --from-list 1"
**AI:** Reads top-1 issue from `EOS Issues List`. Walks IDS. Writes through. Closes the issue.

**User:** "We have an issue with customer complaints."
**AI:** Auto-enters Identify mode. Drives to root. Discusses. Solves.

**User (from inside an L10):** "Next issue. Demos off 4 weeks."
**AI (as part of /eos-level10):** Runs the IDS skill in time-boxed mode. Returns to L10 when done.

**User:** "The solve is 'improve communication.'"
**AI:** Refuses: *"That's not a solve. Concrete action, single owner, due date. What's the actual next step?"*

## Error Handling

- **Identify stalls (5+ "I don't know"):** Pause: *"Sounds like we don't have enough information. Want to make 'gather data on X' a To-Do and IDS this next week?"*
- **Discuss spirals (>10 min, no new info):** Time-box hit. Force-move to Solve: *"We're past the time-box. What's the best solve we can name with what we know?"*
- **Solve is fuzzy:** Reject. Re-ask. Do NOT save until concrete.
- **Solve has no owner or 'we' as owner:** Refuse: *"'We' isn't an owner. Who specifically?"*
- **Two people insist on different root causes:** Capture both. *"You disagree on the root. Best path: name the loudest constraint first, solve that, see if the other resolves. Pick one."*
- **Issue is actually 3 issues:** Surface: *"This looks like 3 issues — A, B, C. Want to IDS them one at a time?"* Split and queue.
- **Source issue not found in Issues List:** Allow standalone IDS. Save the transcript to `EOS IDS — …` and offer to add to Issues List retrospectively.
- **Solve duplicates an existing To-Do or Rock:** Surface: *"This solve overlaps with existing {to-do/rock}. Merge or keep separate?"*
