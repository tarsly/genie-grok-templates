---
name: eos-issues
description: >-
  Manage your EOS Issues List — the open, candid catalog of obstacles, opportunities, and ideas the team will solve. Two tiers: Short-Term (weekly L10) and Long-Term (quarterly+). Add, prioritize top-3 for the next L10, view, and close. Sources include scorecard misses, off-track Rocks, headlines, and ad-hoc captures. Use for capturing an issue, listing issues, picking top-3 for the next L10, or closing an issue after IDS.
---

# EOS Issues List — Open and Honest

## Overview

Issues are the obstacles, opportunities, and ideas your team will solve. The EOS principle is "open and honest" — capture them as they appear, prioritize the top 3 weekly, and IDS them at the L10. This skill manages the Issues List itself — the storage and prioritization layer. The conversational root-cause work happens in `eos-ids`. Two tiers: **Short-Term** (this quarter's stuff, worked in the weekly L10) and **Long-Term** (more than 90 days out, reviewed quarterly).

## When This Skill Applies

- User says "add an issue", "log this issue", "we have a problem with [X]"
- User asks "show our issues", "what are the top issues", "issues for L10"
- User says "prioritize the issues" or "pick top 3"
- User says "close this issue" after IDS
- `eos-scorecard` auto-suggests adding a 3+ consecutive miss
- `eos-rocks` auto-suggests adding a 3+ week off-track Rock
- `eos-level10` opens this during the IDS segment
- User invokes `/eos-issues`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"`, `"eos company preferences"`
2. `search_notes "EOS Issues List"` — existing list

**Banner:**
```
🧩 EOS Issues | {Company} | Short-Term: {N} · Long-Term: {N} | Top 3: {set/unset}
```

## How It Works

### Step 0: Determine Action

Parse to: `add`, `list`, `prioritize`, `close`. Default: `list`.

### Step 1: Add

Capture:
- **Issue** — one-line statement. Push for specificity: *"'We have a culture problem' is too vague — what specifically?"*
- **Tier** — Short-Term (default) or Long-Term
- **Source** — scorecard / rock / headline / ad-hoc / customer / employee
- **Owner Hint** (optional) — who'd most likely own this if it surfaces
- **Created** — today
- **Context** — 1-3 sentences (optional)

Append to the corresponding section of `EOS Issues List`.

### Step 2: List

`read_note` on `EOS Issues List`. Render both tiers, highlight the priority top-3 if set.

### Step 3: Prioritize (for L10)

When user says "pick top 3" or before an L10:
- Read all Short-Term issues
- Ask the team to rank by importance (vote or assign #1, #2, #3)
- Mark in the list with `[#1]`, `[#2]`, `[#3]` flags
- Save

### Step 4: Close

After an IDS in `eos-ids`, the issue is closed:
- Resolution type: To-Do / Rock / Decision / Escalation
- Owner
- Closed date
- Brief outcome summary

Move from active list to a `## Closed` section (keeps history without deleting). Tag the issue note for searchability.

### Step 5: Cross-Skill Sources

When called by `eos-scorecard` (3+ misses) or `eos-rocks` (3+ off-track weeks):
- Auto-draft an issue with full source context
- Confirm with the user before saving (don't silently add)

## Data Structure

`EOS Issues List` note:

```markdown
# EOS Issues List — {Company}

> **Last Updated:** {YYYY-MM-DD}
> **Short-Term:** {N} active · **Long-Term:** {N} active · **Closed:** {N}

---

## Short-Term Issues (for weekly L10)

**Top 3 for next L10:**
1. [#1] {issue} — {source} — {context}
2. [#2] {issue} — {source} — {context}
3. [#3] {issue} — {source} — {context}

**All Active:**
- {issue} — {source} — Added {date}
- {issue} — {source} — Added {date}
- …

---

## Long-Term Issues (for Quarterly Pulsing)

- {issue} — {source} — Added {date}
- …

---

## Closed (last 90 days)

- ✅ {YYYY-MM-DD} {issue} → {resolution-type}: {summary} — Owner: {name}
- ✅ …
```

## Output Format

### List

```
🧩 EOS Issues | {Company}

SHORT-TERM (12 active)
  TOP 3 FOR NEXT L10:
   1. Demos booked off 4 weeks — Scorecard — Sarah's pipeline drying up
   2. Mark's $150K ARR Rock at 40% with 47 days left — Rock — Pipeline thin
   3. Two key clients renewing — need plan — Headline — Jen flagged Tue

  OTHER (9 active): {comma list}

LONG-TERM (5 active)
  • Replace QuickBooks Online (cap reached) — Long-Term — Mark, target Q4
  • New office lease (current expires June 2027) — Long-Term — Mark
  • {3 more}

CLOSED (last 30 days): 8 issues resolved.
```

## Example Usage

**User:** "Add issue: our two biggest clients are renewing next month and we have no plan."
**AI:** Confirms tier (Short-Term), source (ad-hoc), saves to top of Short-Term list.

**User:** "Pick top 3 for L10."
**AI:** Lists Short-Term active issues. Asks team to rank. Marks `[#1]`/`[#2]`/`[#3]` and saves.

**User:** "Show issues."
**AI:** Renders the full list with top 3 highlighted.

**User:** "Close the demos-booked issue."
**AI:** Asks: resolution type? owner? outcome? Saves to `## Closed`.

**User:** "Add a long-term issue: we need to replace QBO before our cap."
**AI:** Tags as Long-Term. Notes "review at next Quarterly Pulsing."

## Error Handling

- **Vague issue ("things feel off"):** Push: *"Let's get specific — what's the observable behavior? Who's involved? What changed?"* Capture once specific.
- **Issue is actually a To-Do or a Rock:** Redirect: *"'Send Bob the proposal' is a To-Do, not an Issue. Add to `/eos-todos` instead."* Or *"'Build a new product line' is a Rock — let's set it in `/eos-rocks`."*
- **Top 3 not set going into L10:** `eos-level10` will prompt prioritization. Don't block.
- **Duplicate issue:** Suggest merging. *"This looks similar to '{existing}'. Update the existing one with new context, or keep separate?"*
- **>20 active short-term issues:** Warn: *"You have {N} open short-term issues. Most teams plateau around 10-15. Consider closing stale ones or moving to long-term."*
- **Closed-section grows past ~100 entries:** Archive oldest to `EOS Issues List Archive — YYYY` to keep the live note readable.
