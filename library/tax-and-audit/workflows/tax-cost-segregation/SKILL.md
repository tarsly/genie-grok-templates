---
name: tax-cost-segregation
description: >-
  Cost segregation analysis for real estate. Estimates the tax savings from reclassifying portions of a property's purchase price from 27.5- or 39-year depreciation into 5-, 7-, and 15-year buckets that qualify for bonus depreciation. Produces a model estimate, identifies whether a formal engineered study is worth commissioning, and tracks the analysis in Cloud Brain across properties.
---

# Cost Segregation Analysis

> **NOT TAX ADVICE.** This skill produces an estimate. Formal cost segregation studies must be performed by a qualified engineer/CPA to be IRS-defensible.

## Overview

Cost segregation accelerates depreciation by separating a property's purchase price into components that depreciate over 5, 7, 15, or 27.5/39 years. Components in the shorter buckets qualify for bonus depreciation in the placed-in-service year (60% in 2026 under the phasedown schedule, scheduled to drop to 40% in 2027 and 20% in 2028 absent legislation).

This skill estimates the savings BEFORE you commission a formal study. Studies typically cost $4,000-$15,000 and only make sense if the estimated tax savings exceed the cost by a healthy margin.

## When This Skill Applies

- User just acquired a rental property and wants to estimate cost seg savings
- User mentions: cost segregation, cost seg, accelerated depreciation, bonus depreciation, Section 168
- User has multiple properties and wants to know which are best candidates for formal studies
- User is RE professional and wants to maximize current-year losses

## Pre-Flight — Preferences

1. Search Cloud Brain: `search_notes` for `"tax preferences"` in `brain/preferences`
2. Search Cloud Brain: `search_notes` for `"investor-profile buy box"` in `brain/preferences`
3. **If found:** Load relevant fields (filing status, marginal tax rate estimate, RE pro status, state)
4. **If not found:** Ask in ONE message:
   - Marginal federal tax rate this year (10/12/22/24/32/35/37%)
   - State tax rate
   - Are you a Real Estate Professional under IRC §469(c)(7)? (yes / no / unsure)
   - Will losses offset passive RE income only, or also W2/active? (depends on RE Pro)
   - Save to Cloud Brain.
5. Banner:
   ```
   🎯 Cost Seg | Marginal: {fed}% + {state}% | RE Pro: {yes/no}
   ```

## How It Works

### Step 1: Gather Property Inputs

Required:
- Purchase price (total)
- Land value (cannot be depreciated — exclude)
- Improvements value = purchase - land
- Asset class
- Placed-in-service date

Optional but useful:
- Square footage
- Year built
- Renovations done since purchase + cost
- Major systems replaced (HVAC, roof, plumbing, electrical)

If user doesn't know land value: assume county assessor's land:improvement ratio applies to purchase price (e.g., assessor says land 20% of total → use 20% × purchase price).

### Step 2: Estimate Component Breakdown

Use rule-of-thumb ratios for the asset class. These are estimates — a real study will identify more components.

**Residential rental (long-term):**
- 5-year (personal property: appliances, carpet, blinds, certain interior trim): 5-8% of improvements
- 7-year (rare for residential rental): 0-2%
- 15-year (land improvements: landscaping, fencing, paving, exterior lighting): 8-12%
- 27.5-year (building structure): 78-87%

**Short-term rental (Airbnb):**
- Same as residential but TREATED differently under tax code if avg stay < 7 days
- 5-year: 6-10% (furniture, decor items added for STR)
- 15-year: 8-12%
- 27.5-year: 78-86%
- BONUS: STR avg stay < 7 days = not passive activity, losses can offset active income

**Commercial:**
- 5-year: 8-15% (specialty fixtures, equipment, decorative)
- 7-year: 0-3% (specific machinery)
- 15-year: 10-18% (parking lots, landscaping, signs, fencing)
- 39-year: 64-82% (building structure)

**Self-storage:**
- 5-year: 5-10%
- 15-year: 15-25% (heavy land improvements — paved drives, lighting, fencing)
- 39-year: 65-80%

**Industrial:**
- 5-year: 10-20% (process equipment, specialty wiring)
- 7-year: 2-5%
- 15-year: 8-15%
- 39-year: 60-80%

Show the estimated component ranges to the user — they can override if they know specifics.

### Step 3: Compute Depreciation Acceleration

Without cost seg:
- Year 1 depreciation = improvements / 27.5 (or 39) × partial-year factor

With cost seg:
- Year 1 depreciation =
  - 5-year × {bonus%} + 5-year × (1 - bonus%) / 5
  - 7-year × {bonus%} + 7-year × (1 - bonus%) / 7
  - 15-year × {bonus%} + 15-year × (1 - bonus%) / 15
  - 27.5/39-year × partial-year factor (no bonus on real property)

Bonus depreciation schedule (current law):
- 2026: 60%
- 2027: 40% (scheduled)
- 2028: 20% (scheduled)
- 2029+: 0% absent legislation

### Step 4: Compute Tax Savings

- Acceleration = year-1 depreciation (with cost seg) − year-1 depreciation (without)
- Federal savings = acceleration × marginal federal rate
- State savings = acceleration × marginal state rate
- Total Year-1 savings = federal + state
- Lifetime "depreciation savings" is approximately neutral — cost seg moves savings forward in time. The benefit is **time value of money** (TVM) on the acceleration.

### Step 5: Decide Whether to Commission a Study

Heuristic:
- If estimated Year-1 savings > $25,000 → formal study is almost always worth it
- If $10K-$25K → maybe — depends on study cost and TVM assumptions
- If <$10K → DIY using IRS Form 3115 + record-keeping, or skip altogether
- Always: a formal engineered study finds MORE 5/7/15-year items than rule-of-thumb. Real-world studies often find 25-35% in shorter buckets vs. the 13-22% rule-of-thumb suggests.

### Step 6: Passive Activity Considerations

- If user is NOT RE Pro: losses are passive. They can ONLY offset passive income. Carry forward unused.
- If user IS RE Pro: losses can offset W2/active income. Major leverage.
- If property is STR with avg stay <7 days: losses can offset active income REGARDLESS of RE Pro status. Huge benefit. Flag this clearly.

### Step 7: Save

- **title:** `Cost Seg — {address or property nickname} — {YYYY-MM-DD}`
- **folder:** `brain/tax`
- **tags:** `["cost-seg", "{asset-class}", "{property-id}"]`

## Data Structure

```markdown
# Cost Segregation Analysis — {Property} — {YYYY-MM-DD}

> **Generated:** {YYYY-MM-DD}
> **Asset Class:** {class}
> **Placed in Service:** {YYYY-MM-DD}

## Property

| Field | Value |
|-------|-------|
| Address | {address} |
| Purchase Price | ${X} |
| Land Value | ${X} |
| Improvements Value | ${X} |
| Square Footage | {N} |
| Year Built | {year} |
| Asset Class | {class} |

## Owner Profile

| Field | Value |
|-------|-------|
| Marginal Federal Rate | {N}% |
| Marginal State Rate | {N}% |
| RE Professional | {yes/no} |
| STR Avg Stay <7 Days | {yes/no/n/a} |
| Loss Treatment | {passive / active / STR active} |

## Component Estimate (Rule-of-Thumb)

| Bucket | Life (yrs) | Est. % | Est. Value | Bonus-Eligible (2026: 60%) |
|--------|-----------|--------|------------|--------------------------|
| Personal Property | 5 | {N}% | ${X} | Yes — ${X × 0.60} |
| Specialty | 7 | {N}% | ${X} | Yes |
| Land Improvements | 15 | {N}% | ${X} | Yes |
| Building Structure | {27.5 / 39} | {N}% | ${X} | No |
| **Total Improvements** | | **100%** | **${X}** | |

## Depreciation Comparison (Year 1)

| | Without Cost Seg | With Cost Seg | Acceleration |
|--|------------------|---------------|--------------|
| 5-year (60% bonus + MACRS) | $0 | ${X} | +${X} |
| 7-year | $0 | ${X} | +${X} |
| 15-year (60% bonus + MACRS) | $0 | ${X} | +${X} |
| 27.5/39-year (straight-line) | ${X} | ${X} | $0 |
| **Year 1 Total** | **${X}** | **${X}** | **+${X}** |

## Tax Savings (Year 1)

| | Federal | State | Total |
|--|---------|-------|-------|
| Without Cost Seg | ${X} | ${X} | ${X} |
| With Cost Seg | ${X} | ${X} | ${X} |
| **Savings** | **${X}** | **${X}** | **${X}** |

## Loss Utilization

- Passive losses available: ${X}
- Active offset (if RE Pro / STR / has passive income): ${X}
- Carryforward (if losses exceed offset capacity): ${X}

## Formal Study Decision

- Estimated Year-1 savings: ${X}
- Typical study cost: $4,000-$15,000
- **Recommendation:** {commission study / DIY with Form 3115 / skip}
- **Why:** {1-2 sentence rationale}

## TVM Note

The lifetime depreciation is approximately equal with or without cost seg — the benefit is **moving deductions forward in time**. At a 7% discount rate, ${X} accelerated to Year 1 vs. spread over 27.5 years has a present value of approximately ${X} above the slow-depreciation case.

## Action Items

- [ ] If commissioning study: get 2-3 quotes from cost seg firms
- [ ] If DIYing: prepare Form 3115 if property already placed in service (change of accounting method)
- [ ] Confirm RE Pro / STR status with CPA
- [ ] Verify land:improvements split with county assessor or appraisal
- [ ] Save this analysis in property file

## Caveats

- Rule-of-thumb component % varies widely by property. Real studies find more in shorter buckets.
- Bonus depreciation schedule may change with new legislation — re-check before relying.
- Recapture: cost seg components recapture at ordinary rates (up to 25%) on sale — model the full hold period when comparing.
- §1031 exchange + cost seg interact in complex ways (boot, depreciation carryover) — get CPA confirmation.
```

## Output Format (Chat)

```
🎯 COST SEG ESTIMATE — {Property}

YEAR 1 ACCELERATION
Without cost seg:  ${X}
With cost seg:     ${X}
Acceleration:      +${X}

TAX SAVINGS (Year 1)
Federal: ${X}
State:   ${X}
TOTAL:   ${X}

DECISION
{Commission study / DIY / Skip}
Why: {1-line}

LOSS UTILIZATION
{Passive only / Active offset available / STR active}

NEXT
1. {action}
2. {action}

Full analysis: brain/tax/Cost Seg — {property} — {date}
```

## Example Usage

**User:** "Cost seg estimate on the Indiana SFR — bought for $185K, land per assessor is $22K, placed in service June 2026, I'm not RE Pro"

**AI:** Improvements = $163K. Residential rental. Component estimate: 5-year ~6%, 15-year ~10%, 27.5-year ~84%. With 60% bonus + MACRS, Year 1 depreciation ~$26K vs. ~$3.5K without. At user's marginal rate, savings ~$6K Year 1. Recommendation: DIY (savings below study threshold). Note passive — losses carry forward.

**User:** "/tax-cost-segregation --address '123 Lakeshore' --purchase-price 850000 --asset-class commercial --placed-in-service 2026-03-15"

**AI:** Commercial property. Higher component % in 5/15 buckets. Year 1 acceleration could exceed $80K. Strongly recommend formal study.

**User:** "I have 4 STRs averaging 4-night stays. Run cost seg on each and tell me the total Year-1 acceleration."

**AI:** STR <7 days → active-offset eligible. Loops through 4 properties (asks for purchase price, land value per property). Aggregates total acceleration. Notes total Year-1 active-offset losses.

**User:** "Was my 2025 cost seg study worth it? Re-run the analysis with last year's numbers."

**AI:** Pulls the prior analysis from `brain/tax/`. Re-runs with actual study results vs. rule-of-thumb estimate. Reports actual savings realized vs. estimated.

## Error Handling

- **If land value isn't known:** Suggest county assessor split. Note the assumption clearly in the analysis.
- **If property is owner-occupied (primary residence):** No cost seg — residence depreciation isn't allowed. Redirect to RE Pro / rental conversion strategies.
- **If property was placed in service in a prior year:** Cost seg is still possible via Form 3115 (Change of Accounting Method). Estimate the catch-up depreciation. Note the complexity — formal study + CPA strongly advised.
- **If bonus depreciation is phasing down rapidly:** Show what the savings would be at 60% (2026), 40% (2027), 20% (2028). Encourages timing decisions.
- **If user has multiple properties:** Suggest the batch approach — run individually, then aggregate. Useful for RE Pro status decisions.
- **If property is held by a partnership (MMLLC) or S-Corp:** Note that the loss flows through K-1 — partner's individual situation determines how it's used. Same analysis applies but K-1 timing matters.
- **If user is asking about cost seg on a property they're selling:** Push back — recapture eats much of the benefit. Suggest 1031 exchange instead, or accept that cost seg is best deployed at acquisition with multi-year hold.
- **If TCJA bonus depreciation rules might change:** Note the legislative risk — current 60/40/20 phasedown could be reinstated to 100% by Congress. Don't model speculation.

## See Also

- `/tax-quarterly-estimate` — apply the accelerated depreciation to current-year quarterly tax (same plugin)
- `/realtor-1031-exchange-tracker` — 1031 in flight changes cost seg math (from `realtor-toolkit`)
- `/realestate-buy-box` — RE Pro and entity context (from `investor-profile`)
