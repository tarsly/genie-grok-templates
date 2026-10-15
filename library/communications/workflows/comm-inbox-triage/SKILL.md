---
name: comm-inbox-triage
description: >-
  Use when the user says '/inbox-triage', 'triage my inbox', 'process my emails', 'what needs attention today', or wants a structured email digest before starting the day. Categorizes every email into four urgency tiers, drafts responses for critical items, and surfaces a daily action-item list. Designed to run on a schedule as an Executive Inbox Agent so communications are handled before the user opens their laptop.
---

# COMM-Inbox-Triage
## AI Email Triage & Inbox Management

---

## Overview

This skill processes a batch of emails, categorizes every item into one of four urgency tiers, drafts responses for critical items, and surfaces action items — so the user starts every day knowing exactly what requires attention and what can be ignored.

Designed to run on a daily schedule as part of the **Executive Inbox Agent** workflow. When automated, it triages the inbox before the user opens their laptop and delivers a structured digest.

**No regulated disclaimer required for this skill.**

---

## Pre-Flight — Client Preferences

1. Search Cloud Brain: `search_notes` with query "comm preferences"
2. If found: confirm in ONE line — "Working as [Name] at [Business] — triage for [accounts], correct?"
   - If yes: proceed immediately
   - If no: ask what changed, update, save, proceed
3. If not found: collect setup in ONE message:
   - Your name and business
   - Email accounts to triage (personal / business / both)
   - Response time expectation (same day / 24h / 48h)
   - Anyone whose emails are always urgent (VIP list — clients, boss, key partners)
   - Anyone whose emails should always be ignored (mailing lists, newsletters to skip)
   - Preferred output format: digest summary OR full item-by-item breakdown
4. Save to `brain/preferences/comm-preferences.md`
5. Show a one-line preferences banner at top of output

---

## Job Inputs

Ask at the start of each run (if not automated):
- Email account being triaged (if multiple accounts are configured)
- Any specific context for today (e.g., "I'm traveling," "big deal closing today")

Do not save these answers to Cloud Brain.

---

## Triage Logic

Process every email in the batch and assign one of four tiers:

### Tier 1 — 🚨 URGENT / ACTION REQUIRED
Criteria: Requires a response or decision **today**. Includes:
- Emails from the VIP list
- Time-sensitive requests (deadlines, approvals, contracts)
- Client or customer issues requiring immediate resolution
- Payment or financial matters requiring action

### Tier 2 — ⚡ IMPORTANT / RESPOND THIS WEEK
Criteria: Needs attention but not today. Includes:
- Business inquiries and leads
- Vendor or partner follow-ups
- Meeting requests and scheduling
- Ongoing project updates requiring a reply

### Tier 3 — 📋 LOW / READ OR FILE
Criteria: Informational, no reply needed. Includes:
- Newsletters or content the user opted into
- Internal company updates
- Receipts and confirmations
- FYI threads with no action item

### Tier 4 — 🗑️ IGNORE / UNSUBSCRIBE CANDIDATE
Criteria: No value, no action, recurring noise. Includes:
- Marketing and promotional emails (unread)
- Notifications from tools (GitHub, Slack digests, app alerts)
- Mass emails with no personal relevance
- Items from the ignore list the user configured

---

## Output Format

### Triage Summary Banner
```
📬 INBOX TRIAGE — [Date]
Account: [email] | [N] items processed
🚨 [N] Urgent  ⚡ [N] Important  📋 [N] Low  🗑️ [N] Ignore
```

### Tier 1 — URGENT (with draft responses)
For each urgent item:
```
FROM: [sender]
SUBJECT: [subject]
SUMMARY: [1-2 sentence summary of what's needed]
ACTION: [what needs to happen]
DRAFT RESPONSE:
---
[draft email ready to send or edit]
---
```

### Tier 2 — IMPORTANT
List with: sender / subject / one-line summary / suggested action

### Tier 3 — LOW
Condensed list: sender / subject only, no summaries needed

### Tier 4 — IGNORE
Count only: "12 promotional / notification emails — no action needed"
Flag any that appear frequently: "⚠️ [Sender] has appeared 5x this week — unsubscribe candidate"

### Action Items
Pull and list any tasks or commitments mentioned across all tiers:
```
ACTION ITEMS FROM THIS TRIAGE:
- [ ] [action] — due [date if mentioned] — from [sender]
```

---

## Memory — Save Triage Log

After every run, save a triage log to Cloud Brain:

**Path:** `brain/communications/triage/triage-[YYYY-MM-DD].md`

**Contents:**
- Date and account
- Tier counts
- Names of urgent senders (for pattern detection)
- Action items surfaced
- Any unsubscribe candidates flagged

This log feeds the Executive Inbox Agent's weekly pattern analysis (which senders consistently generate urgent items, which are consistent noise).

---

## Ongoing Inbox Management — Executive Inbox Agent

When this skill is activated as a scheduled agent task, it runs automatically each morning. Setup notes for system-04:

- **Recommended schedule:** Daily, 6:00–7:00 AM (before the user's workday)
- **Delivery:** In-chat digest OR published to cloud dashboard
- **Pairs with:** `comm-email-drafter` (for drafts), `bizops-follow-up` (action items added to open loops), `bizops-daily-brief` (urgent items surface in morning briefing)
- **Weekly add-on:** Once per week, run a pattern summary: which senders are consistently noisy, which labels are growing, whether inbox volume is increasing

---

## Error Handling

- **No emails to process:** "Inbox is clear — nothing to triage today."
- **Email access not connected:** Ask the user to paste email subjects and senders, or connect their Gmail/Outlook MCP connector
- **VIP not recognized:** If a sender looks like it could be important but isn't on the VIP list, flag it: "⚠️ [Sender] is not on your VIP list — verify this is triaged correctly"
- **Ambiguous urgency:** When in doubt, bump up one tier rather than down — it is safer to over-surface than to miss something critical
