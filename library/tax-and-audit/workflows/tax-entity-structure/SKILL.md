---
name: tax-entity-structure
description: >-
  Entity structure recommendation — sole prop vs LLC vs S-Corp election vs C-Corp. Compares federal tax, self-employment tax, payroll cost, administrative burden, and liability protection across each option for your specific income level and state. Includes RE-specific holdco/operating LLC patterns and S-Corp reasonable-salary modeling.
---

# Entity Structure Recommendation

> **NOT TAX ADVICE.** This is a comparison model. Entity choice has long-term consequences. Confirm with CPA + business attorney before electing or restructuring.

## Overview

For a given net business income and state, the skill models federal income tax + SE tax + payroll tax + state tax + administrative cost across:

1. Sole Proprietorship / Single-Member LLC (disregarded)
2. LLC taxed as partnership (multi-member)
3. LLC with S-Corp election
4. C-Corporation
5. (Special) RE-specific Holdco + Operating LLC structure

Returns the option with the lowest total annual cost, and shows the others for comparison.

## When This Skill Applies

- User is starting a business and asks what entity to form
- User's income has grown and they ask whether to elect S-Corp
- User has multiple properties and asks about LLC structure
- User mentions: S-Corp election, LLC vs S-Corp, C-Corp, entity structure, holdco, series LLC
- User got CPA advice and wants a second opinion (this skill is the second opinion — still confirm)

## Pre-Flight — Preferences

Load `tax preferences`. Asks one additional question on first run:
- Annual administrative tolerance — how much extra paperwork is worth saving $1K? (low / medium / high)
- Liability concern level (low / medium / high — affects whether to recommend LLC even when tax-neutral)
- Future fundraising plans (none / SAFE notes / VC) — affects C-Corp recommendation

Banner:
```
🎯 Entity Structure | State: {state} | Net income: ${X} | Goal: minimize total cost
```

## How It Works

### Step 1: Gather Inputs

Required:
- Net business income (revenue − deductible expenses, before owner compensation)
- State of operation
- Industry (some industries have entity restrictions — e.g., personal service C-Corps face higher tax)
- Number of owners (1 / 2-N)
- Number of employees (you / spouse / others)
- W2 income from outside this business (affects total tax bracket)

For RE: include rental portfolio size, separate vs. commingled, and whether income is RE Pro qualified.

### Step 2: Model Each Entity

For each option, compute:

**Sole Prop / SMLLC (disregarded):**
- Federal income tax on full net income (after QBI deduction)
- SE tax: 15.3% on first $168,600 SE income (2026), 2.9% above
- State tax
- Admin cost: ~$200/year (registered agent if LLC)

**LLC (multi-member, partnership):**
- Same as sole prop but allocated per partner
- K-1 to each partner
- Each partner pays SE tax on their share if active
- Admin cost: ~$500-$1,500/year (partnership return + K-1s)

**LLC + S-Corp Election (Form 2553):**
- Owner draws W2 "reasonable salary" — model at IRS-defensible level for the industry/role (use BLS data lookup)
- Payroll tax (FICA 7.65% employer + 7.65% employee on salary, with Medicare add-back above $200K single / $250K MFJ)
- Distributions above salary: ordinary income, NOT subject to SE tax
- QBI deduction may apply
- Admin cost: $1,500-$4,000/year (payroll service + 1120-S return + reasonable salary documentation)
- Threshold: S-Corp typically saves money once net income > ~$50K-$80K (state-dependent)

**C-Corporation:**
- 21% flat federal corporate tax on profits
- Double-tax: dividends taxed again at qualified dividend rate (0/15/20%)
- Owner takes W2 salary (no double-tax on salary)
- Best for: high retained-earnings businesses, fundraising plans, certain deductions (e.g., medical reimbursement plans)
- Admin cost: $3,000-$10,000/year (1120 + payroll + board minutes + state)

**RE-Specific Holdco + Operating LLC:**
- Holdco (parent) owns operating LLCs (one per property or per market)
- Holdco is the partner / sole member of operating LLCs
- Provides liability separation between properties
- Tax-neutral compared to flat sole prop / SMLLC — same flow-through
- Admin cost: scales linearly with property count ($300-$600 per LLC/year)

### Step 3: Compute Total Annual Cost Per Option

For each option:
- Federal tax
- + SE tax / payroll tax
- + State tax (corporate or pass-through)
- + Admin cost (registered agent + tax prep + payroll)
- = Total annual cost
- − Net business income retained by owner

Rank by retained owner cash.

### Step 4: Apply Soft Factors

After the math, adjust recommendation for:
- Liability exposure (high → favor LLC even when tax-neutral)
- Admin tolerance (low → favor sole prop / SMLLC when savings < $3-5K)
- Fundraising goals (VC plans → favor C-Corp despite tax cost)
- State-specific quirks (e.g., CA $800 minimum LLC franchise tax + 1.5% S-Corp tax; TX franchise tax; NY publication requirement)
- Reasonable-salary risk (S-Corp with no real W2 salary is an IRS audit magnet)

### Step 5: RE-Specific Considerations

If RE portfolio:
- Single-property SMLLC (disregarded) → simplest, OK for 1-2 properties
- Holdco + per-property SMLLCs → standard pattern at 3-10 properties
- Series LLC (in supporting states: DE, NV, TX, IL, UT, OK, AL, KS, MO, TN, IN, IA, ND, VA) → consolidate filings under a parent series; legal protection still developing in some states
- Wyoming/Delaware holdco for anonymity + charging-order protection (popular but not always necessary)
- S-Corp for rental real estate → almost never. Rental income isn't SE income, so S-Corp savings don't apply. Holding RE in S-Corp also creates basis problems and limits 1031.

If RE Pro: structure choice doesn't change RE Pro status — that's owner-level. But entity affects how losses flow.

### Step 6: Save and Render

- **title:** `Entity Recommendation — {YYYY-MM-DD}`
- **folder:** `brain/entities`
- **tags:** `["entity-structure", "{state}", "{industry}"]`

## Data Structure

```markdown
# Entity Structure — {YYYY-MM-DD}

> **State:** {state}
> **Industry:** {industry}
> **Owners:** {N}
> **Net Business Income:** ${X}
> **Outside W2 Income:** ${X}
> **Liability Concern:** {low/med/high}
> **Admin Tolerance:** {low/med/high}

## Comparison Table

| Option | Fed Tax | SE/FICA | State | Admin | Total Cost | Retained | Recommendation |
|--------|---------|---------|-------|-------|------------|----------|----------------|
| Sole Prop / SMLLC | ${X} | ${X} | ${X} | ${X} | ${X} | ${X} | |
| MMLLC (partnership) | ${X} | ${X} | ${X} | ${X} | ${X} | ${X} | |
| LLC + S-Corp election | ${X} | ${X} | ${X} | ${X} | ${X} | ${X} | ⭐ |
| C-Corp | ${X} | ${X} | ${X} | ${X} | ${X} | ${X} | |
| (RE) Holdco + Op LLCs | ${X} | ${X} | ${X} | ${X} | ${X} | ${X} | |

## Recommended: {Option}

**Why:** {2-3 sentence rationale}

### S-Corp Election Specifics (if recommended)

- **Reasonable salary target:** ${X} (based on {BLS data / industry benchmark})
- **Distribution target:** ${X} (net income − salary)
- **FICA on salary:** ${X}
- **SE tax savings vs. sole prop:** ${X}
- **Net savings (after admin cost):** ${X}
- **Break-even threshold:** Net income > ${X} for S-Corp to net out

### Admin Setup Required

- [ ] Form LLC in {state} ({fee})
- [ ] EIN application (free, online IRS)
- [ ] Operating agreement (template — recommend attorney review)
- [ ] Open business bank account
- [ ] Set up payroll service ({Gusto / OnPay / Justworks}) if S-Corp
- [ ] File Form 2553 if S-Corp election (within 2 months 15 days of LLC formation or by Mar 15 of effective year)
- [ ] Quarterly payroll filings if S-Corp
- [ ] Annual 1120-S + K-1 if S-Corp

## Soft Factors Considered

- {Factor — how it influenced the recommendation}
- {Factor}

## Risks to Confirm with CPA

- {Risk specific to recommendation — e.g., S-Corp reasonable salary defense}
- {Risk — e.g., state-specific franchise tax surprise}

## When to Re-Evaluate

- If net income changes by >25%
- If you add a partner or sell ownership
- If you start hiring W2 employees
- Annually before year-end (look-back vs. look-forward)
```

## Output Format (Chat)

```
🎯 ENTITY RECOMMENDATION

NET INCOME: ${X} • STATE: {state}

RANKING (by retained income after taxes + admin)
1. {Option} — retained ${X} ⭐
2. {Option} — retained ${X}
3. {Option} — retained ${X}

RECOMMENDED: {Option}
Savings vs. status quo: ${X}/year

S-Corp ELECTION (if applicable)
Reasonable salary: ${X}
Distributions: ${X}
Net savings after admin: ${X}

SETUP STEPS
1. Form {entity} in {state}
2. EIN
3. Payroll setup (if S-Corp)
4. File 2553 (if S-Corp election)

Full analysis: brain/entities/Entity Recommendation — {date}
```

## Example Usage

**User:** "Should I elect S-Corp? Net income this year will be ~$150K, I'm in Utah."

**AI:** Models sole prop, S-Corp, C-Corp. S-Corp wins at $150K — saves ~$8-10K in SE tax vs. sole prop, net of payroll and admin. Recommends S-Corp election. Provides setup steps.

**User:** "/tax-entity-structure --net-business-income 60000 --state CA --industry consulting"

**AI:** S-Corp savings get eaten by CA's $800 minimum + 1.5% S-Corp tax. Recommends LLC (no S-Corp election) at this income. Notes the threshold for re-evaluation.

**User:** "I have 6 rentals in Indiana, all under my personal name. What's the right structure?"

**AI:** RE pattern. Recommends Wyoming or Indiana Holdco LLC + per-property SMLLCs. Notes Indiana has Series LLC option (cheaper) as alternative. Explains liability separation. Provides ballpark setup costs.

**User:** "I'm raising a seed round in 6 months. Currently sole prop."

**AI:** Recommends Delaware C-Corp. Models tax cost vs. flow-through. Notes VC-required structure, employee stock options, and Section 1202 QSBS implications (5-year hold for up to $10M tax-free gain).

## Error Handling

- **If net income is < $30K:** Recommend sole prop / SMLLC. S-Corp admin overhead eats the savings. Note: "Below ~$40K-$60K (state-dependent), S-Corp usually loses money."
- **If user is in a service business (consulting, law, medicine):** Note QBI limitations above income thresholds and PSC C-Corp restrictions.
- **If user has W2 income from outside business:** SE tax / payroll math changes — outside W2 wages already hit the Social Security cap potentially, reducing S-Corp savings. Model accordingly.
- **If user has international tax exposure (non-US owners, foreign sales):** Recommend specialist. Don't model — too many edge cases.
- **If state has unusual entity rules (CA franchise tax, NY publication, TX margin tax):** Surface these prominently. They can flip the recommendation.
- **If user already has an entity and is considering restructure:** Model the conversion cost (e.g., gain recognition on transfer, election timing windows).
- **If user mentions Series LLC:** Confirm state allows it. Note legal-protection status is still being tested in some courts.
- **If RE Pro status is in play:** Note that entity choice doesn't change RE Pro status. Owner-level test. Affects loss flow-through though.
- **If user is in a community property state and married:** Note that disregarded SMLLC + community property has special rules.

## See Also

- `/tax-quarterly-estimate` — model the tax impact of the recommended structure (same plugin)
- `/tax-deduction-finder` — find S-Corp specific opportunities (same plugin)
- `/bizops-financial-snapshot` — read prior P&L to inform the decision (from `business-operations`)
