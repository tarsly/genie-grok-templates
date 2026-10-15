---
name: tax-quarterly-estimate
description: >-
  Calculate quarterly estimated tax payments for federal and state. Works for SMB owners (sole prop, partnership, S-Corp pass-through, C-Corp), real estate investors with rental + RE pro income, and W2 + side income earners. Uses safe-harbor methods (100/110% of prior year) AND annualized income method, returns the lower payment. Tracks all four quarters in Cloud Brain so you never miss a deadline.
---

# Quarterly Tax Estimate

> **NOT TAX ADVICE.** This skill produces estimates only. Confirm all amounts with your CPA before paying.

## Overview

Computes a quarterly estimated tax payment. Supports three calculation methods, returns the lowest legitimate payment, and saves the calc to Cloud Brain so you can compare quarter-to-quarter.

Supported entities: sole prop, single-member LLC (default), multi-member LLC / partnership (Schedule K-1), S-Corp (Schedule K-1 + W2), C-Corp (separate), self-employed + W2 mix.

Supported states: all US states (with state tax rate lookup). Note: TX, FL, WA, NV, SD, AK, WY, TN, NH = no state income tax (TN/NH have / had limited).

## When This Skill Applies

- User asks about quarterly taxes ("what do I owe Q3?", "estimated tax payment")
- User mentions: 1040-ES, quarterly estimates, safe harbor, underpayment penalty
- User just had a big-income event and wants to plan
- Calendar trigger: 1-2 weeks before Apr 15 / Jun 15 / Sep 15 / Jan 15

## Pre-Flight — Preferences

1. Search Cloud Brain: `search_notes` for `"tax preferences"` in `brain/preferences`
2. **If found:** Load, banner
3. **If not found:** Ask in ONE message:
   - Filing status (single / mfj / mfs / hoh)
   - State of residence
   - Entity type (sole prop / SMLLC / MMLLC / S-Corp / C-Corp / W2 + side)
   - Prior year total federal tax (Form 1040 line 24, prior year) — used for safe-harbor
   - Prior year AGI (>$150K MFJ / $75K MFS triggers 110% safe harbor)
   - Spouse income if MFJ (or "include with mine")
   - Withholding already paid YTD (from W2 + 1099 / quarterly already paid)
   - Save to Cloud Brain: `write_note` → title: `tax-preferences`, folder: `brain/preferences`
4. Banner:
   ```
   🎯 Quarterly Estimate | Filing: {status} | State: {state} | Entity: {type}
   ```

## How It Works

### Step 1: Determine the Quarter

If `--quarter` not provided, infer from current date:
- Q1 (Jan 1 — Mar 31) → due Apr 15
- Q2 (Apr 1 — May 31) → due Jun 15
- Q3 (Jun 1 — Aug 31) → due Sep 15
- Q4 (Sep 1 — Dec 31) → due Jan 15 (next year)

### Step 2: Gather YTD Income / Expense Data

User can:
- Provide `--ytd-income` and `--ytd-expenses` directly
- Reference a Cloud Brain QBO snapshot if `qbo-bookkeeper` is installed
- Ask the skill to pull from QBO live (if `mcp__qbo__*` available)

Required:
- YTD gross income (revenue)
- YTD deductible business expenses
- YTD depreciation / amortization
- YTD W2 wages received (if S-Corp owner / employee)
- YTD withholding (federal + state)
- YTD estimates already paid

For RE investors: include rental income, mortgage interest, property tax, depreciation, repairs, management.

### Step 3: Compute Federal Tax Liability (Three Methods)

**Method 1: Annualized Income Installment Method**
- Annualize YTD income: `YTD income × (12 / months_in_quarter_so_far)`
- Annualize YTD expenses
- Apply Schedule C, K-1, or W2 logic
- Apply standard or itemized deduction estimate (use standard if not provided)
- Apply QBI deduction (20% of qualified business income, with phaseouts above $191,950 single / $383,900 MFJ for 2026)
- Compute tax on annualized taxable income using current-year brackets
- Add SE tax (15.3% on first $168,600 of SE income for 2026, then 2.9%)
- Subtract YTD federal withholding + YTD estimates paid
- Quarter share = (full-year tax × quarter_factor) where:
  - Q1: 22.5%
  - Q2: 45%
  - Q3: 67.5%
  - Q4: 90%

**Method 2: Safe Harbor 100% / 110% of Prior Year**
- If prior year AGI ≤ $150K MFJ / $75K MFS: pay 100% of prior year total tax across 4 quarters
- If prior year AGI > $150K MFJ / $75K MFS: pay 110%
- Quarter share = prior_year_tax × multiplier / 4
- Subtract YTD withholding + YTD estimates paid

**Method 3: 90% of Current Year (Estimated)**
- If you can accurately estimate this year's tax, pay 90% of it spread across quarters
- Risk: if you underestimate, you owe underpayment penalty
- Quarter share = (current_year_estimate × 0.9 / 4) - YTD paid

### Step 4: Choose the Lower Payment

Return the lowest of the three methods that satisfies safe-harbor rules. Default recommendation: Method 2 (safe harbor) unless income is materially lower this year, in which case Method 1 may be lower.

### Step 5: Compute State Tax

Lookup state rate / brackets for the user's state. Apply to taxable income. Subtract state withholding + state estimates.

For no-income-tax states: state estimate = $0.

### Step 6: Special Considerations

Surface these if data suggests:
- **Real estate professional status** (>750 hours + 50% of personal services in RE) → losses can offset W2/active income
- **Section 199A QBI deduction** — applies to most pass-through, with phaseouts
- **Section 179 + bonus depreciation** — for asset purchases this year
- **S-Corp reasonable salary** — if S-Corp, confirm salary is reasonable to avoid IRS challenge
- **Solo 401(k) / SEP-IRA contributions** — major deduction, deadline matters
- **HSA contributions** — pre-tax, increases QBI
- **1031 exchange in flight** — affects timing of gain recognition (cross-ref `realtor-toolkit` 1031 tracker)

### Step 7: Save and Render

- **title:** `Quarterly Estimate — {YYYY} — {Q1/Q2/Q3/Q4}`
- **folder:** `brain/tax`
- **tags:** `["quarterly-estimate", "{quarter}", "{year}"]`

Include all methods, recommended payment, due date, EFTPS link, state payment link.

## Data Structure

```markdown
# Quarterly Estimate — {Year} — {Quarter}

> **Due Date:** {YYYY-MM-DD}
> **Generated:** {YYYY-MM-DD}
> **Filing Status:** {status}
> **Entity:** {type}
> **State:** {state}

## Inputs

| Field | Value |
|-------|-------|
| YTD Gross Income | ${X} |
| YTD Deductible Expenses | ${X} |
| YTD Depreciation | ${X} |
| YTD Net Business Income | ${X} |
| YTD W2 wages | ${X} |
| YTD Federal Withholding | ${X} |
| YTD State Withholding | ${X} |
| YTD Estimates Already Paid | Federal ${X} / State ${X} |
| Prior Year Total Federal Tax | ${X} |
| Prior Year AGI | ${X} (safe harbor: {100% / 110%}) |

## Federal Tax Computation

### Method 1 — Annualized Income
- Annualized income: ${X}
- Annualized expenses: ${X}
- Estimated annual tax: ${X}
- Quarter share ({factor}%): ${X}
- Minus YTD paid: ${X}
- **Method 1 payment due:** ${X}

### Method 2 — Safe Harbor ({100% / 110%})
- Annual safe-harbor target: ${X}
- Per-quarter share: ${X}
- Minus YTD paid (this quarter's pro-rata): ${X}
- **Method 2 payment due:** ${X}

### Method 3 — 90% of Current Year Estimate
- Estimated annual tax: ${X}
- 90% of annual: ${X}
- Per-quarter share: ${X}
- Minus YTD paid: ${X}
- **Method 3 payment due:** ${X}

## State Tax Computation

- State rate / bracket: {bracket}
- Estimated state tax: ${X}
- Minus YTD state withholding + estimates: ${X}
- **State payment due:** ${X}

## Recommended Payment

| | Federal | State | Total |
|--|---------|-------|-------|
| **{Recommended method}** | **${X}** | **${X}** | **${X}** |

**Why this method:** {1-sentence rationale}

## Action Items

- [ ] Pay federal: {EFTPS link or 1040-ES voucher} by {date}
- [ ] Pay state: {state portal link} by {date}
- [ ] Log payment in Cloud Brain under `brain/tax/payments/`
- [ ] Set Q+1 reminder for {next quarter due date}

## Tax-Planning Flags

- {Flag — e.g., "You haven't maxed Solo 401(k) yet, $X opportunity"}
- {Flag — e.g., "Section 179 candidates: ${X} of equipment purchases YTD"}
- {Flag — e.g., "RE Pro status looks possible — confirm hours with CPA"}

## Caveats

This estimate is a model. Real taxable income depends on items this skill doesn't see (AMT, NIIT, additional Medicare tax, state-specific add-backs, prior year carryovers, credits). Confirm with CPA before paying.
```

## Output Format (Chat)

```
🎯 QUARTERLY ESTIMATE — {Year} {Quarter}
Due: {date}

RECOMMENDED PAYMENT
Federal: ${X}  (method: {safe harbor / annualized / 90%})
State:   ${X}
TOTAL:   ${X}

ALTERNATIVE METHODS
Annualized: F ${X} / S ${X}
Safe Harbor: F ${X} / S ${X}
90% Current: F ${X} / S ${X}

PLANNING FLAGS
• {flag}
• {flag}

ACTIONS
1. Pay federal: eftps.gov by {date}
2. Pay state: {state portal} by {date}
3. Log payment

Full calc: brain/tax/Quarterly Estimate — {year} — {quarter}
```

## Example Usage

**User:** "What's my Q3 estimated tax payment?"

**AI:** Loads tax-preferences. Pulls YTD data from QBO if `qbo-bookkeeper` is installed (detected via `mcp__qbo__*` tools); otherwise asks user for YTD totals inline. Runs three methods. Returns recommended payment + flags.

**User:** "/tax-quarterly-estimate --quarter Q4 --ytd-income 285000 --ytd-expenses 120000 --filing-status mfj --state UT --prior-year-tax 32000"

**AI:** Skips preferences ask. Computes all three methods. Recommends method, shows the breakdown.

**User:** "I just sold a rental for a $200K gain. How does this change my Q3?"

**AI:** Pulls Q3 data. Adds capital gain (long-term if held >1 year, short-term if <1). Recomputes. Flags NIIT (3.8%) on gain if MAGI thresholds met. Suggests 1031 if user is still within 45-day identification window.

## Error Handling

- **If user has no prior-year tax data:** Method 2 (safe harbor) is unavailable. Use Method 1 + Method 3. Note: "First year of estimates — no safe-harbor protection. Pay 90%+ of estimated tax to avoid penalty."
- **If user is W2-only with no side income:** This skill doesn't apply. Withholding handles it. Redirect: "W2 withholding usually covers your liability. Use `/tax-deduction-finder` if you have a side hustle."
- **If user provides only quarter YTD income (not start-of-year YTD):** Annualization math will be wrong. Ask for start-of-year YTD totals.
- **If estimated payment is < $1,000:** No penalty for underpayment. Note: "Liability is small — penalty risk is low. Pay $X but skipping is allowable."
- **If user is RE professional with major paper losses:** Walk through the test (750 hours, 50% material participation). Note that losses against W2 income require both.
- **If user has S-Corp with no W2 (no reasonable salary):** Strongly flag — IRS will reclassify distributions to wages if no W2.
- **If state-specific add-backs or credits would change the answer materially:** Note: "State tax estimate is a simplification. Your state has {add-back X} that this skill doesn't model. Confirm with CPA."
- **If user wants AMT calculation:** Note: "AMT post-TCJA rarely triggers for most filers. If your income > $1M or you have major ISO exercise, talk to CPA — this skill doesn't model AMT."
- **If user crosses an entity-structure threshold:** Append to the output: `See also: /tax-entity-structure — at this income level, an S-Corp election might save ~${X}/year.`

## See Also

- `/tax-deduction-finder` — find missed deductions before computing the quarter (same plugin)
- `/tax-entity-structure` — evaluate S-Corp / C-Corp election (same plugin)
- `/qbo-bookkeeper` — YTD income data source (from `qbo-bookkeeper`)
