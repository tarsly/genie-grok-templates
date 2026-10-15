---
name: rei-due-diligence-tracker
description: >-
  Contract-to-close deadline tracker for real estate investors. Sets up, tracks, and updates all contingency deadlines (inspection, financing, appraisal, title, insurance binder, walk-through) after a property goes under contract. Saves live status to cloud brain. Flags RED deadlines approaching within 3 days. Use after an offer is accepted or when user says 'we're under contract', 'add a new deal to track', or 'what's due this week'.
---

# REI Due Diligence Tracker

> **Disclaimer:** Contingency deadlines in real estate contracts are legally binding. Missing a deadline may result in loss of earnest money, inability to exit the contract, or legal liability. Always confirm deadlines directly from your executed Purchase and Sale Agreement. This tool is a tracking aid — it does not replace a licensed attorney's review of your contract terms.

## Overview

Turns a signed contract into a live deadline calendar. Accepts contract close date and contingency periods → calculates all key dates → saves to cloud brain → reports status with traffic-light urgency on every run. Reads across all active transactions when the user asks "what's due this week" or "give me my transaction dashboard."

## When This Skill Applies

- User says "we're under contract" or "offer accepted"
- User wants to set up contract-to-close tracking for a property
- User asks "what's due this week?" or "any deadlines coming up?"
- User wants to mark a contingency complete or waived
- User needs a transaction status dashboard across all active deals
- User says "add a new deal to track" or "show my open transactions"
- Prompted by `rei-offer-generator` after a seller accepts an LOI

---

## Pre-Flight — Load Active Transactions

1. Use `mcp__cloud-brain__search_notes` with query `due diligence tracker` to find all active transaction files.
2. **If user asks for status/dashboard:** Load all open transactions, build summary table, flag any RED deadlines (≤3 days).
3. **If user is adding a new deal:** Gather inputs below, build the deadline calendar, save new file.
4. **Investor preferences:** Use `mcp__cloud-brain__search_notes` with query `REI preferences` to load investor name (for file headers).

---

## How It Works

### Step 0: Intake — New Transaction Setup

Ask in a single message if not already provided:

> "Let's set up contract-to-close tracking. Tell me:
> - Property address
> - Contract acceptance date (today?)
> - Close date (or target close date)
> - Inspection / due diligence period: how many days?
> - Financing contingency: how many days? (or cash — skip)
> - Appraisal contingency: how many days? (or N/A)
> - Title review period: how many days?
> - Seller name (for reference)
> - EMD amount and due date (if not already wired)
> - Any other special contingencies?"

**Default periods if not specified (note to user these are defaults — always check the PSA):**
| Contingency | Typical Default |
|-------------|----------------|
| EMD due | 3 business days from acceptance |
| Inspection / Due diligence | 10 days |
| Financing contingency | 21 days |
| Appraisal | 21 days (often tied to financing) |
| Title review | 5 days after title commitment received |
| Final walk-through | 3–5 days before close |
| Close of escrow | Per contract |

### Step 1: Calculate All Deadlines

From the contract acceptance date and close date, calculate:

| # | Milestone | Calculation | Date | Status |
|---|-----------|-------------|------|--------|
| 1 | EMD Wire / Delivery | Acceptance + [X] business days | | ⬜ Pending |
| 2 | Inspection Period Ends | Acceptance + [X] days | | ⬜ Pending |
| 3 | Financing Contingency Deadline | Acceptance + [X] days | | ⬜ Pending |
| 4 | Appraisal Deadline | Acceptance + [X] days | | ⬜ Pending |
| 5 | Title Commitment Received | Estimate or user-provided | | ⬜ Pending |
| 6 | Title Review / Objection Deadline | Title received + [X] days | | ⬜ Pending |
| 7 | HOA Docs Received | Per contract or request date | | ⬜ Pending |
| 8 | HOA Review / Objection Deadline | HOA docs + [X] days | | ⬜ Pending |
| 9 | Final Walk-Through | Close date − 3 days | | ⬜ Pending |
| 10 | Close of Escrow | Per contract | | ⬜ Pending |

**Custom contingencies:** Add rows for any deal-specific items (soil test, zoning verification, survey, lease assignment, etc.).

### Step 2: Urgency Classification

Classify every open milestone daily based on days remaining:

| Days to Deadline | Status | Symbol |
|-----------------|--------|--------|
| Past due | OVERDUE | 🚨 RED — OVERDUE |
| 0–3 days | Critical | 🚨 RED — ACT NOW |
| 4–7 days | Warning | ⚠️ YELLOW |
| 8+ days | On track | 🟢 GREEN |
| Completed | Done | ✅ COMPLETE |
| Waived / N/A | Skipped | ⬛ WAIVED |

### Step 3: Transaction Status Output

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 TRANSACTION TRACKER — [Property Address]
Seller: [Name]   |   Close Date: [Date]   |   Days to Close: [X]
EMD: $[Amount]   |   Contract Date: [Date]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DEADLINE CALENDAR

[STATUS] MILESTONE                         DUE DATE        DAYS LEFT
──────────────────────────────────────────────────────────────
🚨 RED    EMD Wire                         [Date]          1 day
⚠️ YEL   Inspection Period Ends            [Date]          5 days
🟢 GRN   Financing Contingency            [Date]          18 days
🟢 GRN   Appraisal Deadline               [Date]          20 days
🟢 GRN   Title Review Deadline            [Date]          TBD (awaiting title)
🟢 GRN   Final Walk-Through               [Date]          27 days
🟢 GRN   CLOSE OF ESCROW                  [Date]          30 days
──────────────────────────────────────────────────────────────

NEXT ACTIONS
🚨 EMD must be wired by [Date] — confirm wire instructions with title company
⚠️ Schedule inspection NOW — 5 days remaining. Inspector name / number if saved: [Pref]

CONTACTS (add as known)
Title Company:   [Name / Phone]
Lender:          [Name / Phone]
Inspector:       [Name / Phone]
Seller Agent:    [Name / Phone]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 4: Update a Milestone

When user says "[milestone] is done" or "waive inspection" or "got the title commitment today":

1. Search brain for the transaction file.
2. Update the milestone status to ✅ COMPLETE (or ⬛ WAIVED).
3. If title commitment received — calculate and set the title review deadline.
4. Recalculate urgency across remaining open items.
5. Re-display the updated tracker.

### Step 5: Dashboard View (All Active Transactions)

When user asks "what's due this week" or "show all my open deals":

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 TRANSACTION DASHBOARD — [Date]
[X] Active deals   |   [X] RED deadlines   |   [X] closing this month
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Property 1]                              CLOSE: [Date] — [X] days
  🚨 EMD Wire due [Date] — 1 day
  ⚠️ Inspection ends [Date] — 5 days

[Property 2]                              CLOSE: [Date] — [X] days
  🟢 All deadlines on track. Next: Appraisal [Date] — 12 days.

[Property 3]                              CLOSE: [Date] — [X] days
  ✅ Inspection ✅ Financing ✅ Appraisal — pending final walk-through
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

THIS WEEK'S PRIORITY LIST
1. 🚨 [Property 1] — Wire EMD by [Date]
2. ⚠️ [Property 1] — Book inspection (5 days left)
3. ⚠️ [Property 2] — Respond to title objections by [Date]
```

### Step 6: Close Out a Transaction

When user says "this deal closed" or "deal fell through":

1. Mark all milestones complete or update with outcome (closed / cancelled / fell out of contract).
2. Note reason if fell through (inspection issue, financing denied, title issue, etc.).
3. Archive to `brain/closed-deals/[slug]-closed-[date].md` or `brain/cancelled-deals/[slug]-cancelled-[date].md`.
4. Remove from active dashboard.
5. Prompt: "Run `rei-deal-analyzer` on the next property in your pipeline."

### Step 7: Save / Update Cloud Brain

**New transaction:**
```
Path: brain/due-diligence/[address-slug]-active.md
Tool: mcp__cloud-brain__write_note
```

**Updated transaction:**
```
Tool: mcp__cloud-brain__write_note (overwrite same path)
```

**Closed/archived:**
```
Path: brain/closed-deals/[address-slug]-[outcome]-[YYYY-MM-DD].md
```

---

## Milestone Notes Reference

Include these notes in the output where relevant to help investors who may be new to the process:

| Milestone | Investor Note |
|-----------|---------------|
| EMD | Wire or deliver per PSA. Get wire confirmation. Never wire without confirming instructions via phone — wire fraud is common. |
| Inspection | Hire a licensed inspector. Attend the inspection. Results may trigger a repair addendum or exit. |
| Financing contingency | Lender must issue commitment letter by this date. Chase your loan officer starting 5 days before. |
| Appraisal | Ordered by lender. If value comes in low, you may have room to renegotiate. |
| Title commitment | Title company's preliminary report on ownership and liens. Review carefully — easements, judgments, and liens must be cleared. |
| HOA docs | Review carefully: budget, reserves, pending assessments, rules, litigation. These can kill a deal or reveal hidden costs. |
| Final walk-through | Confirm property condition matches contract (agreed repairs done, nothing removed that was included). |
| Close of escrow | Bring cashier's check or wire funds. Sign docs. Get keys. |

---

## Error Handling

- **No close date provided:** Estimate based on typical 30–45 day close; flag as estimated.
- **Contingency periods not specified:** Use defaults (listed above); flag prominently and tell investor to verify against the PSA.
- **Transaction not found in brain:** Create new tracker; ask for inputs.
- **Multiple transactions for same address:** Prompt to confirm — update existing or create new (re-negotiated deal)?
- **User asks to mark a deadline complete retroactively:** Mark complete with date note; no penalty — this is for tracking, not enforcement.
- **Deal cancelled / fell through during DD:** Log reason, archive, and offer to run `rei-seller-outreach` for the next lead.
- **User doesn't know some dates yet:** Create tracker with TBD fields; prompt to update as dates become known.
- **WebSearch or internet needed for legal date calculations:** Calculate using calendar arithmetic; note any jurisdiction-specific rules the user should verify (e.g., some states count only business days for contingency periods).
