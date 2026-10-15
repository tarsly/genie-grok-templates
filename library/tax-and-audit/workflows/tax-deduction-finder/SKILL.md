---
name: tax-deduction-finder
description: >-
  Scan your QuickBooks Online transactions (via qbo-bookkeeper) for missed deductions, miscategorized expenses, and tax-planning opportunities. Categorizes findings by impact, surfaces commonly-overlooked deductions for your business type, and produces a CPA-ready report. Works without QBO too — accepts CSV exports from any bookkeeping system.
---

# Deduction Finder

> **NOT TAX ADVICE.** This skill flags deduction candidates. Whether each is actually deductible depends on substantiation, business use %, and your facts. Confirm with CPA.

## Overview

Scans transaction data for missed deductions. Three modes:

1. **QBO live** — uses `qbo-bookkeeper` plugin's QBO MCP if installed
2. **CSV import** — accepts exported CSV from any bookkeeping system
3. **Manual review** — walks through a deduction checklist with the user

Identifies three categories:
- **Probably-missed deductions** — transactions in personal/wrong categories that look like business
- **Miscategorized expenses** — booked deductions that may be limited or non-deductible (meals partial, entertainment generally not, capital vs. expense)
- **Planning opportunities** — strategies the data suggests but aren't fully captured yet (Solo 401(k), HSA, S-Corp medical reimbursement, Augusta Rule, etc.)

## When This Skill Applies

- Year-end tax planning ("find me missed deductions")
- Quarterly review ("what did I miss in Q2?")
- User just imported their QBO and wants a sanity check
- User mentions: missed deductions, tax savings, deduction review, expense audit
- Pre-filing review with CPA — get the list ready

## Pre-Flight — Preferences

Load `tax preferences`. Asks one additional question on first run:
- Business type (SMB / RE investor / agency / SaaS / consulting / mixed)
- Common deduction categories you take (home office / vehicle / cell phone / travel / meals — yes/no per category)
- Solo 401(k) / SEP-IRA / HSA active? (yes / no / unsure)

Banner:
```
🎯 Deduction Finder | Business: {type} | Year: {year} | Source: {QBO / CSV / manual}
```

## How It Works

### Step 1: Load Transaction Data

**QBO mode:** Use `mcp__qbo__*` to pull transactions for the period.
**CSV mode:** Parse the file. Expect columns: date, description, amount, category, account, memo (some variation OK — infer headers).
**Manual mode:** Walk through a checklist.

### Step 2: Scan for Probably-Missed Deductions

Pattern-match transaction descriptions and merchants against common-deduction patterns:

**Universal SMB:**
- Software subscriptions (SaaS tools, even if charged to personal card)
- Cell phone bills (business-use % deductible)
- Internet (home office portion)
- Professional development (courses, books, conferences)
- Subscriptions (industry publications, newsletters, paid Twitter, etc.)
- Bank fees on business accounts
- Mileage (if vehicle log exists)
- Home office (if eligible — see Section 280A rules)
- Health insurance (self-employed health insurance deduction above the line)
- Charitable contributions made by business (if structured correctly)

**RE-specific:**
- Property management fees
- Auto travel to properties
- Education on RE (books, courses, masterminds, podcasts that charge)
- Mortgage interest, prop tax, insurance
- Repairs (vs. improvements — important distinction)
- Depreciation
- Travel to acquire properties (deductible if you actually buy)
- Cost seg study fees (deductible the year completed)
- 1031 exchange QI fees

**Agency / consulting:**
- Subcontractor payments (1099-NECs needed)
- Software subscriptions (Notion, Slack, Figma, GitHub, etc.)
- Coworking memberships
- Client gifts (limit $25/recipient/year)
- Travel to client sites

**SaaS:**
- AWS/GCP/Azure
- Development tools, IDE licenses
- Domain registrations
- SSL certificates
- API costs (OpenAI, Anthropic, etc.)
- App store fees (Apple, Google) — net out of revenue or expense depending on accounting

### Step 3: Scan for Miscategorized Expenses

Common issues to flag:

- **Meals booked at 100%** → most meals are 50% deductible (some 100%: company events, samples)
- **Entertainment booked as deduction** → TCJA eliminated entertainment deduction entirely
- **Capital purchase booked as expense** → equipment > de minimis safe harbor ($2,500/item) should depreciate or §179
- **Personal expense routed through business** → distribution, not deduction; flag for owner education
- **Health insurance through W2 vs. self-employed deduction** → may need adjustment
- **Vehicle: both mileage AND actual expenses claimed** → must pick one method (with multi-year consistency rules)
- **Home office: regular method vs. simplified** → check which is being claimed
- **Charitable contributions on Schedule C** → wrong place, belongs on Schedule A (personal) for most SMB structures
- **Owner draws categorized as expense** → not deductible; flag bookkeeping issue

### Step 4: Surface Planning Opportunities

Based on the data and `tax preferences`:

- **Solo 401(k) / SEP-IRA contribution** — if not maxed, model the deduction at marginal rate
- **HSA contribution** — if HDHP, $4,150 single / $8,300 family (2026)
- **S-Corp Section 105 medical reimbursement plan** — if S-Corp and unreimbursed medical exists
- **Augusta Rule** — rent your personal residence to your business for ≤14 days/year, tax-free income to you, deductible to business
- **Hiring family** — kids under 18 can earn up to standard deduction tax-free; spouse can be on payroll with 401(k) match
- **Section 179 + bonus depreciation** — equipment placed in service this year
- **Pre-pay deductible expenses** — accelerate next year's deductions into this year (cash basis only, 12-month rule)
- **Defer income** — if cash basis, push December invoices to January
- **Section 199A QBI** — verify QBI calculation if income is below phaseout
- **Real estate Section 469 elections** — RE Pro, aggregating activities, grouping rentals
- **Cost segregation** — if you bought property this year and haven't done cost seg, append to the output: `See also: /tax-cost-segregation [property address] to estimate the tax savings.`

### Step 5: Categorize Findings by Impact

For each finding:
- **Impact:** estimated dollar deduction or savings
- **Confidence:** high (clear-cut) / medium (likely with substantiation) / low (worth asking CPA)
- **Action:** what to do (provide receipt, recategorize, file form, change going forward)

### Step 6: Save

- **title:** `Deduction Review — {year/quarter} — {YYYY-MM-DD}`
- **folder:** `brain/tax`
- **tags:** `["deduction-finder", "{year}", "{business-type}"]`

## Data Structure

```markdown
# Deduction Review — {Year/Quarter} — {YYYY-MM-DD}

> **Business Type:** {type}
> **Source:** {QBO live / CSV / manual}
> **Transactions reviewed:** {N}
> **Period:** {date range}

## Summary

| Category | Findings | Est. Impact |
|----------|---------|-------------|
| Probably-missed deductions | {N} | +${X} deduction → ~${X} tax savings |
| Miscategorized expenses | {N} | -${X} deduction (some not allowed) |
| Planning opportunities | {N} | +${X} potential savings |
| **Net Estimated Impact** | | **+${X}** |

## Probably-Missed Deductions

### High Confidence

| Date | Description | Amount | Why Missed | Action |
|------|-------------|--------|-----------|--------|
| {date} | {description} | ${X} | {personal card, business use} | {recategorize + provide receipt} |
| ... |

### Medium / Low Confidence (Ask CPA)

| Date | Description | Amount | Why Uncertain | Action |
|------|-------------|--------|--------------|--------|

## Miscategorized Expenses

| Date | Description | Booked As | Should Be | Impact | Action |
|------|-------------|-----------|-----------|--------|--------|
| {date} | {desc} | {wrong cat} | {right cat} | -${X} or limit | {action} |

## Planning Opportunities

### {Strategy 1 — e.g., Solo 401(k) max-out}
- **Status:** {current contribution / not started}
- **Opportunity:** Contribute up to ${X} (employee deferral + employer) for {year}
- **Est. tax savings:** ${X} at {marginal rate}%
- **Deadline:** {date}
- **Action:** Open Solo 401(k) at {Fidelity / Schwab / Vanguard}, contribute by {date}

### {Strategy 2}
...

## Patterns Worth Fixing Going Forward

- {pattern — e.g., Stop using personal card for SaaS subscriptions; route through business card}
- {pattern — e.g., Tag meals as "client meal" or "team meal" at transaction time}
- {pattern}

## CPA Discussion Points

1. {finding — what to ask about specifically}
2. {finding}

## Caveats

- This is a pattern-match scan. Actual deductibility requires receipts and business-purpose documentation.
- Some flagged items may already be deducted in a category this scan didn't see.
- Section 162 "ordinary and necessary" is the bar — keep substantiation that ties each deduction to a business purpose.
```

## Output Format (Chat)

```
🎯 DEDUCTION REVIEW — {period}
Reviewed {N} transactions • Source: {QBO/CSV/manual}

NET ESTIMATED IMPACT: +${X} tax savings

PROBABLY MISSED ({N} items, +${X})
1. {description} — ${X} ({date})
2. {description} — ${X} ({date})
... (showing top 5, full list saved)

MISCATEGORIZED ({N} items)
1. {description} — {issue}
2. {description} — {issue}

PLANNING OPPORTUNITIES (TOP 3)
1. {strategy} — est. savings ${X}, deadline {date}
2. {strategy} — est. savings ${X}, deadline {date}
3. {strategy} — est. savings ${X}, deadline {date}

PATTERNS TO FIX
- {pattern}
- {pattern}

Full report: brain/tax/Deduction Review — {period}
```

## Example Usage

**User:** "Run a deduction finder on my 2025 QBO data"

**AI:** Detects `mcp__qbo__*`. Pulls 2025 transactions. Scans. Finds 14 missed deductions ($3,200 total), 6 miscategorized items, 4 planning opportunities. Saves report. Estimates +$1,800 net tax savings.

**User:** "/tax-deduction-finder --csv ~/Downloads/transactions-2025.csv --business-type re"

**AI:** Parses CSV. RE-focused scan: looks for property travel, repair vs. improvement issues, mortgage interest splits, depreciation. Returns RE-specific findings.

**User:** "I don't have bookkeeping software — walk me through a manual deduction review"

**AI:** Mode: manual. Asks ~20 questions covering common deduction categories. Builds a list as user answers. Generates report.

**User:** "Year-end review — what should I do before December 31?"

**AI:** Pulls YTD data. Surfaces planning opportunities with December deadlines (Solo 401(k) timing, equipment §179, prepayments, Augusta Rule). Generates a prioritized December action list.

## Error Handling

- **If `qbo-bookkeeper` plugin isn't installed and user says they use QBO:** Recommend installing it. Offer CSV-mode in the meantime.
- **If CSV format is unrecognized:** Ask user to map columns. Save the mapping in preferences so future imports work.
- **If transaction count is huge (>5,000):** Note: "Scan will be slower. Sampling top 500 by amount for the report — full data still stored." Run on the high-leverage subset.
- **If user has commingled personal + business in one account:** Flag the practice itself — note that IRS expects separate accounts for clean substantiation. Recommend cleanup for next year.
- **If user wants to claim something this skill flags as low-confidence:** Don't approve or deny — just note: "Take this to your CPA with documentation. The position may be defensible, just not from a transaction scan alone."
- **If user's data shows 100% home office (all utilities, full rent):** Flag — only the business-use percentage qualifies. Check Section 280A rules.
- **If user has S-Corp and book shows $0 owner W2 wages:** CRITICAL flag — IRS will reclassify distributions to wages if no reasonable salary. Cross-link `/tax-entity-structure`.
- **If user is on cash basis and has December invoices:** Surface defer-income opportunity. Note: must be consistent — can't cherry-pick.
- **If user has multiple businesses on one set of books:** Recommend separating. Aggregated bookkeeping makes audits painful.

## See Also

- `/tax-quarterly-estimate` — fold findings into this quarter's payment (same plugin)
- `/tax-cost-segregation` — major missed deduction on RE properties (same plugin)
- `/tax-entity-structure` — surface findings flag S-Corp election threshold (same plugin)
- `/qbo-bookkeeper` — live transaction data source (from `qbo-bookkeeper`)
