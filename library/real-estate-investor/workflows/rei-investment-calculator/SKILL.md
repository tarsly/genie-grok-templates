---
name: rei-investment-calculator
description: >-
  Deep real estate financial modeling — BRRRR, flip, buy-and-hold projections, seller finance amortization, lease-option analysis, refinance analysis, wrap mortgage, subject-to, amortization tables, equity build-up projections, multi-scenario comparisons, depreciation and cost segregation estimates, balloon payment calculations, or needs an amortization table, equity build-up projection, or multi-scenario comparison for a specific deal.
---

# REI Investment Calculator

> **Disclaimer:** This skill produces financial models for informational and planning purposes only. It is not a substitute for advice from a licensed financial advisor, CPA, or lender. Tax estimates are illustrative only — consult a qualified tax professional before making tax-related decisions. All projections should be independently verified before investing.

## Overview

A comprehensive real estate financial modeling engine. Not just mortgage math — this produces full financial models with amortization tables, equity build-up curves, tax benefit estimates, and multi-year projections. Use this **after** `rei-deal-analyzer` has confirmed a deal is worth pursuing. Reads prior deal analysis from cloud brain when available to avoid re-entering data.

**Analysis types supported:**
1. BRRRR (Buy, Rehab, Rent, Refinance, Repeat)
2. Flip profit/loss
3. Buy-and-hold projections (5/10/20/30 year)
4. Refinance analysis (current vs. new terms)
5. Seller finance amortization
6. Lease-option analysis (buyer and seller perspectives)
7. Side-by-side scenario comparison

## When This Skill Applies

- User asks for BRRRR analysis on a property
- User wants flip profit/loss calculated
- User asks for buy-and-hold projections over multiple years
- User needs a refinance analysis (current vs. new terms)
- User wants an amortization table or schedule
- User asks about seller finance terms and payment breakdowns
- User needs lease-option analysis (from buyer or seller side)
- User wants to compare two or more investment scenarios side by side
- User asks "what are my returns over 10 years?"
- User wants to see equity build-up over time
- User asks about tax benefits, depreciation, or cost segregation
- User needs a balloon payment or payoff calculation

---

## Pre-Flight — Investor Preferences

1. Use `mcp__cloud-brain__search_notes` with query `REI preferences`
2. **If found:** Load preferences. Display ⚙️ banner with investor name and key thresholds.
3. **If not found:** Run preferences setup (same as rei-deal-analyzer). Save to `brain/preferences/rei-preferences.md`.
4. **Check for prior deal analysis:** Use `mcp__cloud-brain__search_notes` with any address/deal slug mentioned. If found, load it and note: "Found saved deal analysis from {date} — pre-loading those inputs."

---

## Core Calculation Engine

**Monthly Payment (fully amortizing)**
```
M = P × [r(1+r)^n] / [(1+r)^n − 1]
P = principal | r = monthly rate (annual/12) | n = total payments (years×12)
```

**Monthly Payment (interest-only)**
```
M = P × r
```

**Remaining Balance at Month X**
```
B(x) = P × [(1+r)^n − (1+r)^x] / [(1+r)^n − 1]
```

**Total Interest Over Life of Loan**
```
Total Interest = (M × n) − P
```

---

## Analysis Type 1: BRRRR

**Phase 1 — Buy & Rehab**
```
Total Acquisition Cost = Purchase Price + Rehab + Closing Costs (buy 2–3%) + Holding Costs
Holding Costs = Monthly carrying cost × rehab timeline (months)
```

**Phase 2 — Rent**
```
Monthly Cash Flow = Rent − Vacancy − Expenses − Debt Service
Stabilized NOI = Annual cash flow before debt service
```

**Phase 3 — Refinance**
```
Refinance Amount = ARV × LTV% (typically 70–80%)
Cash Recouped = Refinance Amount − Original Loan Payoff − Refi Closing Costs
Cash Left in Deal = Total Acquisition Cost − Cash Recouped
```

**Phase 4 — Repeat Assessment**
```
New Monthly Cash Flow = Rent − Vacancy − Expenses − New Payment
Cash-on-Cash (on cash left in deal) = Annual Cash Flow / Cash Left in Deal
Infinite Return = Cash Left in Deal ≤ 0
```

**BRRRR Summary Table:**

| Phase | Amount |
|-------|--------|
| Purchase Price | |
| Rehab Cost | |
| Closing Costs (buy) | |
| Holding Costs | |
| **Total Investment** | |
| ARV | |
| Refinance LTV | |
| Refinance Loan Amount | |
| Payoff Original Loan | |
| Refi Closing Costs | |
| **Cash Back at Refi** | |
| **Cash Left in Deal** | |
| Monthly Rent | |
| Monthly Expenses + New Payment | |
| **Monthly Cash Flow** | |
| **Cash-on-Cash Return** | |
| **Infinite Return?** | Yes / No |

---

## Analysis Type 2: Flip Profit/Loss

```
ACQUISITION COSTS
Purchase Price:
Closing Costs (buy 2–3%):
Financing Points (hard money):
Total Acquisition:

REHAB COSTS
Construction / Labor:
Materials:
Permits:
Contingency (10–15%):
Total Rehab:

HOLDING COSTS (over X months)
Loan Payments:
Insurance / Taxes / Utilities:
Total Holding:

SELLING COSTS
Realtor Commission (5–6%):
Seller Closing Costs (1–2%):
Staging / Marketing:
Total Selling:

PROFIT / LOSS
Sale Price (ARV):
− Total Acquisition:
− Total Rehab:
− Total Holding:
− Total Selling:
═══════════
NET PROFIT:
ROI:
Annualized ROI:
Profit per Month:
```

**70% Rule Check:**
```
Maximum Offer = ARV × 70% − Rehab Cost
```

---

## Analysis Type 3: Buy-and-Hold Projections (5/10/20/30 Year)

Year-by-year table (adjustable assumptions):
- Annual rent growth: 3% (conservative) / 4% (moderate) / 5% (aggressive)
- Annual appreciation: 3% / 4% / 5%
- Annual expense growth: 2%

| Year | Rent | Expenses | NOI | Debt Service | Cash Flow | Property Value | Loan Balance | Equity | Total Return |
|------|------|----------|-----|-------------|-----------|---------------|-------------|--------|-------------|
| 1 | | | | | | | | | |
| 5 | | | | | | | | | |
| 10 | | | | | | | | | |
| 20 | | | | | | | | | |
| 30 | | | | | | | | | |

**Wealth Building Summary:**

| Metric | Year 5 | Year 10 | Year 20 | Year 30 |
|--------|--------|---------|---------|---------|
| Property Value | | | | |
| Equity | | | | |
| Total Cash Flow (cumulative) | | | | |
| Total Depreciation Tax Savings | | | | |
| **Total Wealth Created** | | | | |
| **Annualized Total Return** | | | | |

**Opportunity Cost Comparison (included by default):**
Show how the same initial cash invested would perform in:
- S&P 500 at 10% average annual return
- High-yield savings at 4.5%
- This real estate investment (total return including equity, cash flow, and depreciation savings)

---

## Analysis Type 4: Refinance Analysis

```
CURRENT LOAN
Current Balance:
Interest Rate:
Monthly Payment:
Remaining Term:

PROPOSED REFINANCE
New Loan Amount:
New Interest Rate:
New Term:
Closing Costs:

COMPARISON
Monthly Savings:
Break-Even Point: XX months (closing costs / monthly savings)
Total Interest (current remaining):
Total Interest (new):
Interest Savings Over Life:

CASH-OUT ANALYSIS (if applicable)
Cash Out Amount:
New LTV:
Net Cash After Costs:
```

---

## Analysis Type 5: Seller Finance Amortization

Full amortization schedule:

| Payment # | Date | Payment | Principal | Interest | Balance |
|-----------|------|---------|-----------|----------|---------|
| 1 | | | | | |
| ... | | | | | |
| Balloon | | | | | |

**Summary:**
- Total interest paid (to balloon or full term)
- Seller's total return (down + payments + balloon)
- Buyer's effective cost
- Buyer's cash-on-cash if renting
- Refinance-ability at balloon date

---

## Analysis Type 6: Lease-Option Analysis

**For the Tenant-Buyer:**
```
Option Price:
Option Fee (non-refundable):
Monthly Rent:
Monthly Rent Credit:
Lease Term:
Estimated Value at Exercise:

Total Rent Paid:
Total Rent Credits:
Effective Down Payment at Exercise (option fee + credits):
Remaining Balance to Finance:
Built-in Equity at Exercise:
```

**For the Seller (Investor doing lease-option):**
```
Current Value / Your Basis:
Option Fee Received:
Monthly Spread (rent − your costs):
Total Spread Over Term:

IF tenant-buyer exercises: Gross Profit + Cash Flow + Option Fee = Total Return
IF tenant-buyer does NOT exercise: Option Fee Kept + Cash Flow + Property Retained
```

---

## Analysis Type 7: Side-by-Side Scenario Comparison

| Metric | Scenario A | Scenario B | Scenario C |
|--------|-----------|-----------|-----------|
| Property / Strategy | | | |
| Purchase Price | | | |
| Cash Required | | | |
| Monthly Payment | | | |
| Monthly Cash Flow | | | |
| Cash-on-Cash Return | | | |
| Cap Rate | | | |
| DSCR | | | |
| 5-Year Equity | | | |
| 10-Year Wealth Created | | | |
| Break-Even Occupancy | | | |
| **vs. Investor's Buy Box** | | | |
| **Winner?** | | | |

Include written analysis explaining which scenario wins and why, with consideration for investor's saved risk tolerance, capital availability, and strategy preference.

---

## Tax Benefit Calculations

**Depreciation (Residential)**
```
Depreciable Basis = Purchase Price − Land Value (typically 20%) + Improvements
Annual Depreciation = Depreciable Basis / 27.5 years
Tax Savings = Annual Depreciation × Marginal Tax Rate
```

**Depreciation (Commercial)**
```
Annual Depreciation = Depreciable Basis / 39 years
```

**Cost Segregation Estimate**
```
Personal property (5-year): ~15–25% of building value
Land improvements (15-year): ~5–15% of building value
Year 1 Bonus Depreciation Impact: [estimated additional tax savings vs. straight-line]
```

*Tax estimates are illustrative only. Consult a CPA before making tax-related decisions.*

---

## Save to Cloud Brain

```
Path: brain/investment-calcs/{description-slug}-{YYYY-MM-DD}.md
Tool: mcp__cloud-brain__write_note
```

---

## Error Handling

- **Analysis type ambiguous:** Infer from context ("rehab + refi" = BRRRR; "ARV + sell" = flip; "rent + hold" = buy-and-hold). Ask if still unclear.
- **Missing inputs:** List specifically what's needed for the chosen analysis type.
- **Negative cash flow / returns:** Show what price, rent, or terms would hit the investor's saved targets.
- **Unrealistic inputs:** Flag with note; proceed as-is.
- **30-year amortization table requested:** Show annual summaries (30 rows) by default; offer full 360-row monthly detail.
- **Balloon scenario without exit plan:** Calculate balloon amount and flag — "At the balloon date, balance will be $X. Plan for refinance, payoff, or extension."
- **Comparing scenarios across different markets:** Note that the comparison reflects both deal terms and market differences, not terms alone.
