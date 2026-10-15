---
name: bizops-financial-snapshot
description: >-
  Executive financial summary — P&L, revenue by stream, expense breakdown, cash flow projection, burn rate, runway, margin analysis, profitability, financial forecast, and actionable recommendations, or needs a P&L summary, cash flow projection, burn rate, or margin analysis across one or more businesses.
---

# Financial Snapshot — Executive Financial Intelligence

## Overview

> **Disclaimer:** This skill produces financial analysis for informational and planning purposes only. It is not a substitute for advice from a licensed financial advisor, CPA, or lender. All figures should be verified by qualified professionals before making financial decisions.

Financial Snapshot generates an executive-level financial summary for entrepreneurs running one or multiple businesses. The user provides their numbers (revenue, expenses, accounts, debts) or references saved financial data, and AI produces a structured analysis: revenue by stream, expense categories, profit margins, cash flow projection, burn rate, runway estimate, and 3 actionable recommendations to improve financial health. This is not an accounting tool — it is an intelligence tool that helps the user see the full picture and make better financial decisions.

## When This Skill Applies

- User says "financial snapshot" or "financial summary"
- User says "how's my money?" or "where's my money going?"
- User says "revenue report" or "P&L" or "profit and loss"
- User says "cash flow analysis" or "cash flow projection"
- User says "burn rate" or "runway" or "how long can I last?"
- User says "financial health check" or "financial review"
- User says "expense breakdown" or "revenue breakdown"
- User says "am I profitable?" or "what are my margins?"
- User says "financial forecast" or "financial projection"
- User says "how to improve my finances" or "financial recommendations"
- User says "quarterly financials" or "monthly financials"
- User mentions wanting to understand their financial position across businesses
- User asks "can I afford [something]?" — run the snapshot to contextualize the answer


## Pre-Flight — Business Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops financial preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm with user ("These are your saved settings — proceed, or say 'update' to change any"), then proceed
3. **If not found:** Ask in ONE message:
   - Business name(s) and industry
   - Number of businesses to track (1 or multiple)
   - Fiscal year (calendar year / custom)
   - Primary currency
   - Preferred reporting period (monthly / quarterly / annual)
   - Save to Cloud Brain: `write_note` → title: `bizops-financial-preferences`, folder: `brain/preferences`
4. Apply throughout: use business name in headers, reporting period in all date ranges
5. Show banner at top of every output:
   ```
   📊 Financial Snapshot | {Business Name} | {Reporting Period} | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my financial preferences' to change settings."*

## How It Works

### Step 1: Gather Financial Data

Determine where the numbers come from:

1. **User provides directly:** The user shares revenue figures, expenses, account balances, debts, etc. in the conversation
2. **Saved in brain:** Use `search_notes` with keywords like "financial", "revenue", "snapshot" to find prior financial data in Cloud Brain
3. **Referenced by file:** The user points to a spreadsheet, CSV, or file
4. **Partial data:** The user gives some numbers — AI works with what is available and flags what is missing

Collect these data points (whatever is available):

| Data Point | What to Capture |
|-----------|----------------|
| **Revenue streams** | Each source of income, monthly/annual amount |
| **Expenses** | Major expense categories, monthly/annual amount |
| **Bank balances** | Operating account, savings, reserves |
| **Debts/Liabilities** | Loans, credit lines, outstanding payments |
| **Receivables** | Money owed to the user (invoices, pending payments) |
| **Payables** | Money the user owes (upcoming bills, commitments) |
| **Time period** | What period does this data cover? (Month, quarter, year) |
| **Multiple businesses** | Does the user run multiple entities? Capture each separately. |

If the user has multiple businesses, track each revenue stream and expense category separately to show the full portfolio view.

### Step 2: Build Revenue Analysis

```markdown
## Revenue Analysis

### Revenue by Stream

| # | Revenue Stream | Monthly | Annual (Projected) | % of Total | Trend |
|---|---------------|--------:|-------------------:|:----------:|:-----:|
| 1 | [Stream Name] | $X,XXX | $XX,XXX | XX% | [Up/Down/Flat] |
| 2 | [Stream Name] | $X,XXX | $XX,XXX | XX% | [Up/Down/Flat] |
| 3 | [Stream Name] | $X,XXX | $XX,XXX | XX% | [Up/Down/Flat] |
| | **TOTAL** | **$XX,XXX** | **$XXX,XXX** | **100%** | |

### Revenue Concentration Risk
- **Highest concentration:** [Stream] at [X]% of total revenue
- **Risk level:** [Low (<30% in any one stream) / Medium (30-50%) / High (50%+)]
- **Recommendation:** [Diversify if concentrated, or double down if the stream is growing]

### Revenue Quality Assessment
| Metric | Value | Assessment |
|--------|------:|:----------:|
| **Recurring revenue** | $X,XXX/mo | [Good/Needs work] |
| **One-time revenue** | $X,XXX/mo | [Stable/Volatile] |
| **Average deal size** | $X,XXX | [Growing/Shrinking/Stable] |
| **Revenue predictability** | [High/Medium/Low] | [Assessment] |
```

### Step 3: Build Expense Analysis

```markdown
## Expense Analysis

### Expenses by Category

| # | Category | Monthly | Annual (Projected) | % of Revenue | Essential? |
|---|----------|--------:|-------------------:|:------------:|:----------:|
| 1 | [Category — e.g., Payroll/Contractors] | $X,XXX | $XX,XXX | XX% | Yes/No |
| 2 | [Category — e.g., Software/SaaS] | $X,XXX | $XX,XXX | XX% | Yes/No |
| 3 | [Category — e.g., Marketing/Ads] | $X,XXX | $XX,XXX | XX% | Yes/No |
| 4 | [Category — e.g., Rent/Office] | $X,XXX | $XX,XXX | XX% | Yes/No |
| 5 | [Category — e.g., Insurance/Legal] | $X,XXX | $XX,XXX | XX% | Yes/No |
| 6 | [Category — e.g., Travel/Meals] | $X,XXX | $XX,XXX | XX% | Yes/No |
| 7 | [Category — e.g., Subscriptions] | $X,XXX | $XX,XXX | XX% | Yes/No |
| | **TOTAL EXPENSES** | **$XX,XXX** | **$XXX,XXX** | **XX%** | |

### Expense Efficiency
- **Expense-to-revenue ratio:** [X]% — [Good (<70%) / Moderate (70-85%) / Concerning (85%+)]
- **Non-essential expenses:** $X,XXX/mo — [X]% of total expenses
- **Potential savings identified:** $X,XXX/mo (see recommendations below)

### Subscription Audit Flag
[List any subscriptions or recurring expenses that should be reviewed — tools not being used, overlapping services, contracts up for renegotiation]
```

### Step 4: Calculate Profit Margins

```markdown
## Profitability

### Profit & Loss Summary

| Metric | Monthly | Annual (Projected) |
|--------|--------:|-------------------:|
| **Gross Revenue** | $XX,XXX | $XXX,XXX |
| **Cost of Goods/Services** | ($X,XXX) | ($XX,XXX) |
| **Gross Profit** | $XX,XXX | $XXX,XXX |
| **Operating Expenses** | ($X,XXX) | ($XX,XXX) |
| **Net Operating Income** | $XX,XXX | $XXX,XXX |
| **Taxes (estimated)** | ($X,XXX) | ($XX,XXX) |
| **Net Profit** | **$XX,XXX** | **$XXX,XXX** |

### Margin Analysis

| Margin Type | Percentage | Assessment |
|------------|:----------:|:----------:|
| **Gross Margin** | XX% | [Healthy (>50%) / Moderate (30-50%) / Low (<30%)] |
| **Operating Margin** | XX% | [Healthy (>20%) / Moderate (10-20%) / Thin (<10%)] |
| **Net Margin** | XX% | [Strong (>15%) / Acceptable (8-15%) / Thin (<8%)] |

### Profitability by Business (if multiple entities)

| Business | Revenue | Expenses | Net Profit | Margin |
|----------|--------:|---------:|-----------:|:------:|
| [Business 1] | $XX,XXX | $XX,XXX | $XX,XXX | XX% |
| [Business 2] | $XX,XXX | $XX,XXX | $XX,XXX | XX% |
| **Combined** | **$XX,XXX** | **$XX,XXX** | **$XX,XXX** | **XX%** |
```

### Step 5: Cash Flow Projection

```markdown
## Cash Flow Projection

### Current Position

| Account | Balance |
|---------|--------:|
| Operating Account | $XX,XXX |
| Savings/Reserve | $XX,XXX |
| Outstanding Receivables | $XX,XXX |
| Outstanding Payables | ($XX,XXX) |
| **Net Cash Position** | **$XX,XXX** |

### 90-Day Cash Flow Forecast

| Month | Starting Cash | Cash In | Cash Out | Ending Cash | Net Change |
|-------|-------------:|--------:|---------:|------------:|-----------:|
| Month 1 | $XX,XXX | $XX,XXX | ($XX,XXX) | $XX,XXX | +/- $X,XXX |
| Month 2 | $XX,XXX | $XX,XXX | ($XX,XXX) | $XX,XXX | +/- $X,XXX |
| Month 3 | $XX,XXX | $XX,XXX | ($XX,XXX) | $XX,XXX | +/- $X,XXX |

### Cash Flow Health

| Metric | Value | Assessment |
|--------|------:|:----------:|
| **Monthly cash burn** | $XX,XXX | |
| **Monthly cash generation** | $XX,XXX | |
| **Net monthly cash flow** | +/- $X,XXX | [Positive/Negative] |
| **Cash conversion cycle** | X days | [How long from service delivery to cash received] |

### Scenario Analysis

| Scenario | Assumption | Impact on Cash |
|----------|-----------|:--------------:|
| **Best case** | [Revenue increases X%, expenses hold] | +$X,XXX/mo |
| **Base case** | [Current trends continue] | +/- $X,XXX/mo |
| **Worst case** | [Revenue drops X%, unexpected expense of $Y] | -$X,XXX/mo |
```

### Step 6: Burn Rate and Runway

```markdown
## Burn Rate & Runway

### Burn Rate Calculation

| Metric | Amount |
|--------|-------:|
| **Monthly fixed costs** | $XX,XXX |
| **Monthly variable costs (avg)** | $X,XXX |
| **Total monthly burn** | $XX,XXX |
| **Daily burn rate** | $XXX |

### Runway Estimate

| Scenario | Available Cash | Monthly Burn | Runway |
|----------|---------------:|-------------:|:------:|
| **Zero revenue** | $XX,XXX | $XX,XXX | X months |
| **50% revenue drop** | $XX,XXX | $XX,XXX (net) | X months |
| **Current pace** | $XX,XXX | +$X,XXX (net) | Indefinite (cash positive) |

### Runway Assessment
- **Current runway:** [X months at zero revenue]
- **Recommended reserve:** [3-6 months of operating expenses = $XX,XXX]
- **Reserve gap:** [How much more is needed to hit recommended reserve, or "Fully funded"]
- **Runway risk level:** [Low (6+ months) / Moderate (3-6 months) / High (<3 months) / Critical (<1 month)]
```

### Step 7: Generate Actionable Recommendations

Produce exactly 3 high-impact, specific recommendations:

```markdown
## Top 3 Financial Recommendations

### 1. [Action Title] — [Expected Impact: $X,XXX/month]

**The Problem:** [What is costing money, leaving money on the table, or creating risk]

**The Fix:** [Specific, step-by-step action. Not "cut costs" — say exactly what to cut and by how much.]

**Implementation:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Timeline:** [This week / This month / This quarter]
**Difficulty:** [Easy / Moderate / Requires effort]
**Impact:** [How much this could save or generate per month]

---

### 2. [Action Title] — [Expected Impact: $X,XXX/month]

**The Problem:** [Specific financial issue]
**The Fix:** [Specific action]
**Implementation:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Timeline:** [Timeframe]
**Difficulty:** [Level]
**Impact:** [Dollar amount]

---

### 3. [Action Title] — [Expected Impact: $X,XXX/month]

**The Problem:** [Specific financial issue]
**The Fix:** [Specific action]
**Implementation:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Timeline:** [Timeframe]
**Difficulty:** [Level]
**Impact:** [Dollar amount]

---

### Combined Impact of All 3 Recommendations
**Potential monthly improvement:** $X,XXX/month
**Potential annual impact:** $XX,XXX/year
```

Recommendation categories to consider:
- **Revenue increase:** Raise prices, add a revenue stream, upsell existing clients, collect faster
- **Cost reduction:** Cancel unused subscriptions, renegotiate contracts, automate manual work, reduce overhead
- **Cash flow optimization:** Invoice faster, require deposits, adjust payment terms, build reserves
- **Risk reduction:** Diversify revenue, build emergency fund, get insurance, reduce debt
- **Tax optimization:** Maximize deductions, adjust entity structure, accelerate/defer income, retirement accounts

### Step 8: Compile and Save

Assemble everything into a single document:

```markdown
# Financial Snapshot: [Business Name or "Personal Finances"]

> **Date:** YYYY-MM-DD
> **Period Covered:** [Month, Quarter, or Year]
> **Businesses Included:** [List of entities]
> **Overall Financial Health:** [Strong / Healthy / Moderate / Concerning / Critical]
> **Prepared By:** AI OS

---

## Executive Summary

[3-5 bullet points — the most important financial takeaways. What is going well, what needs attention, and the single most impactful action to take.]

---

## Revenue Analysis
[From Step 2]

## Expense Analysis
[From Step 3]

## Profitability
[From Step 4]

## Cash Flow Projection
[From Step 5]

## Burn Rate & Runway
[From Step 6]

## Top 3 Recommendations
[From Step 7]

---

## Key Financial Metrics Dashboard

| Metric | Value | Status |
|--------|------:|:------:|
| Monthly Revenue | $XX,XXX | [Up/Down/Flat] |
| Monthly Expenses | $XX,XXX | [Up/Down/Flat] |
| Net Profit (Monthly) | $XX,XXX | [Positive/Negative] |
| Net Margin | XX% | [Healthy/Moderate/Thin] |
| Cash on Hand | $XX,XXX | [Sufficient/Low/Critical] |
| Monthly Burn Rate | $XX,XXX | |
| Runway (zero revenue) | X months | [Safe/Moderate/Low] |
| Revenue Concentration | XX% in top stream | [Diversified/Concentrated] |
| Receivables Outstanding | $XX,XXX | [Current/Aging] |

---

## Data Gaps & Assumptions

[List any data that was not provided and assumptions made to fill gaps. Be transparent about what is estimated vs. confirmed.]

---

*Financial intelligence prepared by AI OS. This is not tax advice, legal advice, or professional accounting. Consult a CPA for tax-specific decisions and an attorney for legal/entity structure questions. Refresh this snapshot monthly for best results.*
```

Save to Cloud Brain using `mcp__cloud-brain__write_note` with:
- **title:** `Financial Snapshot {YYYY-MM-DD}`
- **folder:** `research`
- **tags:** `["financial", "snapshot", "research"]`

### Step 9: Present to User

Give the user:
1. The overall financial health verdict (one line)
2. Monthly net profit (or loss) and margin
3. Runway at current pace
4. The #1 recommendation (highest impact)
5. Confirmation that the snapshot was saved to Cloud Brain
6. Prompt: "Want me to dig deeper into any section? Or should I run this again next month?"

## Quality Standards

1. **Math must be correct.** Every calculation is verified. Revenue minus expenses equals profit. Margins are computed accurately. If numbers do not add up, flag it.
2. **Conservative estimates.** When projecting, use conservative assumptions. Better to be pleasantly surprised than dangerously optimistic.
3. **No fabricated data.** If the user did not provide a number, do not invent it. Mark it as "Not provided" and explain what is needed for a complete picture.
4. **Actionable, not academic.** The recommendations must be things the user can do THIS MONTH. Not "consider diversifying your revenue portfolio" — but "launch a $49/month subscription tier for your existing audience by March 15."
5. **Multi-business aware.** Many entrepreneurs run multiple businesses. The snapshot should handle this gracefully — individual P&Ls plus a consolidated view.
6. **Disclaimer included.** This is AI-generated financial intelligence, not professional accounting or tax advice. Always include the disclaimer.

## Output Format

A single comprehensive markdown document containing the full financial snapshot (revenue analysis, expense analysis, profitability, cash flow projection, burn rate/runway, recommendations, and metrics dashboard). Typically 2,000-4,000 words depending on complexity. Saved to Cloud Brain in the `research` folder.

## Example Usage

**User:** "Financial snapshot — I made $18K last month from deals, $3K from connections, $2K from TCs. Expenses are about $4K/month. I have $12K in the bank."

**AI executes:**
- Revenue analysis: $23K/month across 3 streams, deals = 78% concentration
- Expense analysis: $4K/month, 17% expense-to-revenue ratio (excellent)
- Profitability: $19K/month net, 83% margin (strong)
- Cash flow: positive, but low reserves relative to income
- Runway: 3 months at zero revenue (moderate — should build to 6 months)
- Recommendations: (1) Build $24K reserve (6 months of expenses), (2) Diversify away from deal-dependent revenue, (3) Invoice faster or require deposits
- Saves to Cloud Brain as `Financial Snapshot {date}` in the `research` folder

**User:** "/financial-snapshot Q1 2026"

**AI executes:**
- Searches Cloud Brain for Q1 financial data or asks for numbers
- Builds quarterly view with monthly breakdown
- Includes year-over-year comparison if prior data exists
- Saves to Cloud Brain as `Financial Snapshot {date}` in the `research` folder

**User:** "I run {YOUR_PLATFORM} and {YOUR_SERVICE}. {YOUR_PLATFORM} is pre-revenue. {YOUR_SERVICE} does about $15K/month. Combined expenses are $6K. How do things look?"

**AI executes:**
- Separate P&L for each business: {YOUR_PLATFORM} ($0 revenue, X% of shared expenses), {YOUR_SERVICE} ($15K revenue, X% of shared expenses)
- Consolidated view: $15K revenue, $6K expenses, $9K net
- Cash flow projection showing pre-revenue {YOUR_PLATFORM} as a cost center
- Recommendations focused on: getting {YOUR_PLATFORM} to revenue, controlling pre-launch costs, maximizing {YOUR_SERVICE} while {YOUR_PLATFORM} ramps
- Saves to Cloud Brain as `Financial Snapshot {date}` in the `research` folder

## Error Handling

- **If the user provides no financial data:** Ask: "I need some numbers to work with. At minimum, give me: (1) monthly revenue (even rough), (2) monthly expenses (even rough), and (3) cash in the bank. I'll work with whatever you have."
- **If data is incomplete:** Build the snapshot with what is available. Flag missing sections: "I couldn't calculate [metric] because [data point] was not provided. Share that number and I'll update the snapshot."
- **If the numbers do not make sense (e.g., expenses exceed revenue but user says they are profitable):** Flag the discrepancy respectfully: "The numbers show expenses ($X) exceeding revenue ($Y), which means the business is currently operating at a loss of ($Z/month). If that's not right, double-check the numbers and I'll re-run."
- **If a financial snapshot already exists for this date in Cloud Brain:** Overwrite — the user is likely updating with better data. Note: "Updated your existing financial snapshot."
- **If the user asks for projections far into the future (12+ months):** Provide them but flag: "Projections beyond 3-6 months are increasingly speculative. I've included them, but treat them as directional, not definitive. Refresh monthly for accuracy."
- **If the user asks for tax-specific advice:** Provide general guidance but always add: "For tax-specific decisions (deductions, entity structure, quarterly estimates), consult your CPA. I can help you prepare the data for that conversation."
- **If revenue or expenses vary wildly month to month:** Use a 3-month or 6-month average for projections rather than a single month. Note: "Your income varies significantly, so I'm using a [X]-month average for projections. Individual months may differ."
