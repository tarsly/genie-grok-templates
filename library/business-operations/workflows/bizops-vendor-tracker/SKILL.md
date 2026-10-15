---
name: bizops-vendor-tracker
description: >-
  Track vendors and supplier relationships — add vendors, log contract terms, track renewal dates, monitor costs, flag upcoming renewals and unused subscriptions, and get a full vendor spend summary, or needs to log a vendor contract, flag an upcoming renewal, or audit subscription spend.
---

# Vendor Tracker — Supplier & Contract Management

## Overview

Every business has vendors — software subscriptions, contractors, suppliers, service providers. Most business owners have no idea what they're actually paying in total, which contracts are renewing next month, or which subscriptions nobody is using anymore. Vendor Tracker fixes this. Log your vendors once, and get renewal alerts, spend summaries, and cost audits that keep you in control of your vendor relationships and your money.

## When This Skill Applies

- User says "add a vendor" or "track this vendor" or "log a supplier"
- User says "what vendors do I have?" or "show me my vendors"
- User says "what's renewing soon?" or "vendor renewals" or "upcoming contracts"
- User says "how much am I spending on vendors?" or "vendor spend" or "vendor costs"
- User says "cancel [vendor]" or "remove a vendor"
- User says "vendor audit" or "review my subscriptions"
- User mentions a software subscription, contractor, or supplier
- User says "update [vendor]'s contract" or "renew [vendor]"
- User says "which subscriptions am I not using?"
- User asks "what's my total SaaS spend?" or "what am I paying for software?"

## Pre-Flight — Vendor Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops vendor preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Business name
   - Vendor categories relevant to your business (e.g., software/SaaS, contractors, suppliers, utilities, marketing services, professional services)
   - Renewal alert window (how many days ahead to flag upcoming renewals — default: 30 days)
   - Monthly vendor spend threshold to flag as high cost (e.g., anything over $500/month gets a flag)
   - Save to Cloud Brain: `write_note` → title: `bizops-vendor-preferences`, folder: `brain/preferences`
4. Apply throughout: use saved categories for all vendor entries, alert window for renewal flags
5. Show banner at top of every output:
   ```
   🏢 Vendor Tracker | {Business Name} | Renewal alert: {X} days | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my vendor preferences' to change settings."*

## How It Works

### Vendor Note Template

Each vendor is stored as a separate note in Cloud Brain. Use `write_note` with:
- **title:** `vendor-{vendor-name-slug}`
- **folder:** `brain/vendors`
- **tags:** `["vendor", "{category}", "active"]`

```markdown
# {Vendor Name}

> **Category:** {software / contractor / supplier / service / subscription / utility}
> **Status:** Active / Cancelled / Under Review
> **Added:** {YYYY-MM-DD}

## Contract Details

| Field | Value |
|-------|-------|
| Cost | ${amount} / {month/year/project} |
| Billing cycle | Monthly / Annual / One-time / Per-project |
| Contract start | {YYYY-MM-DD} |
| Contract end / renewal | {YYYY-MM-DD} |
| Auto-renews | Yes / No / Unknown |
| Notice required to cancel | {X days / Not applicable} |
| Payment method | {card / ACH / invoice} |

## Contact

| Field | Value |
|-------|-------|
| Account rep | {name, if known} |
| Support email | {email} |
| Account/login | {username or "see password manager"} |
| Contract/agreement | {location of contract document} |

## What We Use This For

{Brief description of what this vendor provides and which team/function uses it}

## Notes

{Any relevant context: past issues, negotiation history, alternatives considered}

## Interaction Log

| Date | Note |
|------|------|
| {today} | Vendor added to tracker |
```

---

### Operations

#### Add a Vendor

1. Collect: vendor name, category, cost, billing cycle, renewal date, what it's used for
2. Check for duplicates: `search_notes` with vendor name before creating
3. Create note using template above
4. Confirm: "✓ {Vendor} added. Renewal flagged for {date} (in {X} days)."

#### List All Vendors

1. `search_notes` with query `"vendor"` (folder: `brain/vendors`)
2. Output summary table:

```
VENDOR ROSTER — {Business Name}
================================

| Vendor | Category | Cost | Billing | Renewal | Status |
|--------|----------|------|---------|---------|--------|
| {name} | {cat} | ${X}/mo | Monthly | {date} | ✅ Active |

TOTAL MONTHLY SPEND: ${X}
TOTAL ANNUAL SPEND:  ${X}

Upcoming Renewals (next {X} days):
  ⚠️ {Vendor} — renews {date} (in {X} days) — ${cost}
  ⚠️ {Vendor} — renews {date} (in {X} days) — ${cost}
```

#### Flag Upcoming Renewals

1. `search_notes` with folder `brain/vendors`
2. For each active vendor, compare renewal date to today
3. Flag any renewing within the saved alert window:
   - 🚨 Renewing in ≤7 days
   - ⚠️ Renewing in 8–30 days (or saved threshold)
4. Output:

```
RENEWAL ALERTS
==============
🚨 {Vendor} — renews in {X} days ({date}) — ${cost}/year — Auto-renews: Yes
   Action: Review before {date} or it will renew automatically.

⚠️ {Vendor} — renews in {X} days ({date}) — ${cost}/year — Auto-renews: No
   Action: Decide whether to renew. Contact: {rep name / support email}
```

#### Vendor Spend Audit

Full cost audit across all vendors:

1. Load all vendor notes from `brain/vendors`
2. Calculate totals by category
3. Flag high-cost vendors (above threshold in preferences)
4. Output:

```
VENDOR SPEND AUDIT — {Business Name}
=====================================

BY CATEGORY
| Category | # Vendors | Monthly | Annual |
|----------|-----------|---------|--------|
| Software | {X} | ${X} | ${X} |
| Contractors | {X} | ${X} | ${X} |
| Services | {X} | ${X} | ${X} |
| **TOTAL** | **{X}** | **${X}** | **${X}** |

TOP 5 BY COST
1. {Vendor} — ${X}/mo — {category}
2. {Vendor} — ${X}/mo — {category}

HIGH-COST FLAGS (>${threshold}/mo)
🚨 {Vendor} — ${X}/mo — Consider: {renegotiate / consolidate / audit usage}

RECOMMENDATIONS
1. {Specific action — e.g., "3 software tools overlap in function — consider consolidating"}
2. {Specific action — e.g., "{Vendor} cost increased 40% at last renewal — benchmark alternatives"}
3. {Specific action — e.g., "{X} vendors on annual plans — stagger renewals to avoid Q1 cash crunch"}
```

#### Cancel / Deactivate a Vendor

1. `search_notes` → `read_note` to find the vendor
2. Update the note: change Status to `Cancelled`, add cancellation date to interaction log
3. `write_note` with `overwrite: true`
4. Confirm: "✓ {Vendor} marked as cancelled as of {date}. Note updated in Cloud Brain."

#### Update Vendor Details

1. Find via `search_notes`, load with `read_note`
2. Apply changes, update interaction log with today's date and what changed
3. `write_note` with `overwrite: true`

## Memory Paths

| Content | Path |
|---------|------|
| Preferences | `brain/preferences/bizops-vendor-preferences.md` |
| Vendor records | `brain/vendors/vendor-{slug}.md` |

## Error Handling

- **Vendor not found:** "I don't have {vendor} on file. Say 'add {vendor}' to create a record."
- **Duplicate detected:** "I already have a vendor named {name}. Show you the existing record, or create a new one?"
- **No renewal date on file:** Proceed, but flag: "⚠️ No renewal date on file for {vendor}. Add one to get renewal alerts."
- **No vendors in brain:** "No vendors on file yet. Say 'add a vendor' and I'll set up the tracker."
- **User says "cancel everything" or bulk action:** Confirm each cancellation individually: "You said cancel all [category] vendors. That's {X} vendors: {list}. Confirm?"
