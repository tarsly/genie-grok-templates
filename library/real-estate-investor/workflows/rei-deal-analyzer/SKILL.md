---
name: rei-deal-analyzer
description: >-
  Analyze real estate deals — cash flow, cap rate, cash-on-cash return, DSCR, creative finance, subject-to, seller finance, BRRRR, multi-family underwriting, financing comparisons, go/no-go recommendation, or needs a cash flow, cap rate, DSCR, or creative-finance analysis before deciding on a deal.
---

# REI Deal Analyzer

> **Disclaimer:** This skill produces financial analysis for informational and planning purposes only. It is not a substitute for advice from a licensed financial advisor, real estate professional, CPA, or lender. All figures should be independently verified by qualified professionals before making investment decisions.

## Overview

Analyzes any real estate deal and delivers a comprehensive financial breakdown with a clear go/no-go recommendation — **scored against your personal buy box criteria, not just generic benchmarks.** Covers cash-on-cash return, cap rate, DSCR, monthly cash flow, GRM, break-even occupancy, 1% rule, and ROI. Supports multiple financing scenarios side by side (conventional, FHA, hard money, creative finance, subject-to, seller finance). Reads saved property research from cloud brain when available.

## When This Skill Applies

- User provides a property address and asks "is this a good deal?"
- User gives purchase price, rents, and financing terms and wants analysis
- User wants to compare conventional vs. creative financing on a deal
- User asks for cash flow, cap rate, or cash-on-cash return
- User says "run the numbers on this"
- User asks "should I buy this property?"
- User wants to underwrite a multi-family or commercial property
- User provides a deal and wants a go/no-go recommendation
- User mentions subject-to, seller finance, or wrap mortgage analysis
- User asks about DSCR, break-even, or debt coverage on a deal

---

## Pre-Flight — Investor Preferences

1. Use `mcp__cloud-brain__search_notes` with query `REI preferences`
2. **If found:** Load saved preferences. Display banner with investor name, target markets, strategy focus, and **personal thresholds** (min CoC, min cash flow/door, target cap rate, min DSCR). Confirm or offer to update. All scoring in this analysis will use these thresholds — not generic defaults.
3. **If not found:** Ask in ONE message:
   > "Before I run this analysis, let me save your investor profile so every deal is scored against your personal targets. Tell me:
   > - Your name and contact info
   > - Target markets (city/state)
   > - Investment strategy focus (LTR, STR, BRRRR, flip, creative finance, mixed)
   > - Property types (SFR, multifamily, commercial)
   > - Target price range
   > - **Min monthly cash flow per door** (e.g., $200)
   > - **Target cash-on-cash return** (e.g., 10%)
   > - **Target cap rate** (e.g., 7%)
   > - **Min DSCR** (e.g., 1.25)
   > - Financing preference (conventional, creative, cash)"

   Save to `brain/preferences/rei-preferences.md` via `mcp__cloud-brain__write_note`. Proceed.

4. **Check for prior property research:** Use `mcp__cloud-brain__search_notes` with the property address slug. If a research dossier exists, load it and note: "Found saved property research from {date} — using that data."

5. Show ⚙️ **Preferences Active** banner at top of output.

---

## How It Works

### Step 0: Gather Deal Inputs

Parse what the user provided. Required inputs — ask for anything missing:

| Input | Required | Example |
|-------|----------|---------|
| Property address or description | Yes | "123 Main St, Dallas TX" |
| Purchase price | Yes | $150,000 |
| Estimated monthly rent (gross) | Yes | $1,800/mo |
| Financing type | Yes | Conventional, FHA, hard money, seller finance, subject-to, cash |
| Down payment or cash to close | Yes (unless cash) | 20% or $30,000 |
| Interest rate | Yes (unless cash) | 7.5% |
| Loan term | Yes (unless cash) | 30 years |
| Estimated rehab cost | If applicable | $25,000 |
| ARV | If applicable | $200,000 |

If the user provides an address but not comps/rents, use WebSearch to find estimated values.

### Step 1: Research (if address provided and no brain data)

Use WebSearch to pull:
- Recent comparable sales (within 0.5 mi, last 6 months)
- Current rental listings for similar properties
- Property tax estimates
- Insurance cost estimates

### Step 2: Set Expense Assumptions

Use researched data or conservative defaults (note which are assumed):

| Expense | Default | Notes |
|---------|---------|-------|
| Property taxes | 1.2% of purchase price/yr | Adjust from research |
| Insurance | 0.5% of purchase price/yr | Adjust for flood zone, state |
| Vacancy | 8% of gross rent | 5% hot markets, 10%+ soft |
| Maintenance / repairs | 10% of gross rent | Higher for older properties |
| CapEx reserves | 5% of gross rent | Roof, HVAC, water heater |
| Property management | 10% of gross rent | Include even if self-managing |
| HOA | $0 unless specified | Ask if condo/townhouse |

### Step 3: Calculate Core Metrics

**Gross Rent Multiplier (GRM)**
```
GRM = Purchase Price / (Monthly Rent × 12)
Below 8 = Strong | 8–12 = Market dependent | Above 12 = Weak
```

**1% Rule Check**
```
Monthly Rent / Purchase Price ≥ 1%
```

**Net Operating Income (NOI)**
```
NOI = Gross Annual Rent − Vacancy − Operating Expenses (no debt service)
```

**Cap Rate**
```
Cap Rate = NOI / Purchase Price × 100
```

**Debt Service (Monthly P&I)**
```
M = P × [r(1+r)^n] / [(1+r)^n − 1]
```

**Monthly Cash Flow**
```
Cash Flow = Monthly Rent − Vacancy − Monthly Expenses − Monthly Debt Service
```

**Cash-on-Cash Return (CoC)**
```
CoC = Annual Cash Flow / Total Cash Invested × 100
Total Cash Invested = Down Payment + Closing Costs + Rehab (if any)
```

**DSCR**
```
DSCR = NOI / Annual Debt Service
Below 1.0 = Negative | 1.0–1.2 = Tight | 1.2–1.5 = Healthy | 1.5+ = Strong
```

**Break-Even Occupancy**
```
Break-Even = (Operating Expenses + Debt Service) / Gross Potential Rent × 100
Below 75% = Very safe | 75–85% = Acceptable | Above 85% = Tight
```

**Total ROI (Year 1)**
```
ROI = (Cash Flow + Principal Paydown + Appreciation) / Total Cash Invested
```

### Step 4: Run Multiple Financing Scenarios (if requested)

| Metric | Conventional 20% | FHA 3.5% | Seller Finance | Subject-To | Cash |
|--------|-----------------|-----------|----------------|------------|------|
| Cash to close | | | | | |
| Monthly payment | | | | | |
| Monthly cash flow | | | | | |
| Cash-on-cash | | | | | |
| DSCR | | | | | |

**Creative finance notes:**
- **Subject-To:** Use existing mortgage terms. Cash to close = purchase price minus existing loan balance.
- **Seller Finance:** User-defined terms. Calculate both P&I and interest-only cash flows.
- **Hard Money:** Short-term bridge (6–18 months), higher rates (10–14%). Include points as upfront cost.
- **Wrap Mortgage:** Underlying loan + seller spread. Show all-in effective rate.

### Step 5: Score Against Investor's Personal Buy Box

Use the thresholds loaded from preferences (not generic defaults). Score each metric:

| Metric | Your Target | This Deal | Signal |
|--------|------------|-----------|--------|
| Cash-on-Cash | [saved pref] | X% | 🟢 / 🟡 / 🔴 |
| Monthly Cash Flow/door | [saved pref] | $X | 🟢 / 🟡 / 🔴 |
| Cap Rate | [saved pref] | X% | 🟢 / 🟡 / 🔴 |
| DSCR | [saved pref] | X.X | 🟢 / 🟡 / 🔴 |
| 1% Rule | | Meets/Misses | 🟢 / 🔴 |
| Break-Even Occupancy | | X% | 🟢 / 🟡 / 🔴 |

**Verdict:**
- 5–6 🟢 = **STRONG BUY**
- 3–4 🟢, rest 🟡 = **GOOD DEAL** (note the risks)
- Mixed 🟢/🟡/🔴 = **PROCEED WITH CAUTION** (specify what needs to change)
- 2+ 🔴 = **PASS** (explain why; show what price/terms would make it work)

Always include: "What would make this deal work" — the price, rent, or terms needed to hit your target returns.

### Step 6: Save Analysis to Cloud Brain

```
Path: brain/deal-analyses/{address-slug}-{YYYY-MM-DD}.md
Tool: mcp__cloud-brain__write_note
```

Tell the user: "Analysis saved. Run `rei-investment-calculator` for deep financial modeling, `rei-rental-analysis` for a rental strategy comparison, or `rei-offer-generator` to draft your LOI."

---

## Output Format

```markdown
⚙️ Preferences Active | Investor: [Name] | Strategy: [Focus] | Targets: CoC [X%] | CF $[X]/door | Cap [X%] | DSCR [X.X]

---

# Deal Analysis: {Address}
**Date:** {date}

---

## Property Overview
[Property details table]

## Financing Terms
[Financing table]

## Income & Expenses (Monthly)
| Item | Amount |
|------|--------|
| Gross Rent | |
| − Vacancy (X%) | |
| − Property Tax | |
| − Insurance | |
| − Maintenance | |
| − CapEx Reserve | |
| − Property Management | |
| − HOA | |
| **= Net Operating Income** | |
| − Debt Service | |
| **= Monthly Cash Flow** | |

## Key Metrics vs. Your Buy Box
[Scoring table with personal targets]

## Recommendation
**Verdict: [STRONG BUY / GOOD DEAL / PROCEED WITH CAUTION / PASS]**
[Explanation]

### What Would Make This Deal Work
[If not STRONG BUY — the price, rent, or terms to hit targets]

## Comparable Data
[Comps from research or web search]

---
*REInvestor Toolbox — Deal Analyzer | Saved to brain/deal-analyses/{slug}*
```

---

## Error Handling

- **No purchase price:** Ask specifically before proceeding.
- **No estimated rents:** Search via WebSearch; if unavailable, ask.
- **No financing terms:** Ask: financing type, down payment, interest rate, term.
- **Negative cash flow:** Don't just say PASS. Show the price, rent, and rate that would break even and hit targets.
- **Unrealistic inputs (e.g., 1.5% conventional rate):** Flag and proceed as-is with a note.
- **Subject-to but no existing loan terms:** Flag — need existing balance, rate, payment, remaining term.
- **Unusual calculation results:** Double-check inputs and flag to user before displaying.
