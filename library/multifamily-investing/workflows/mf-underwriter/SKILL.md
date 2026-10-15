---
name: mf-underwriter
description: >-
  Comprehensive multifamily underwriting — post-contract deep due diligence analysis. Verifies all financials against T12, bank statements, and Schedule E actuals. Runs 3-scenario NOI (Floor/Base/Upside), full valuation matrix, DSCR at every offer price, post-sale tax reassessment, formal Risk Matrix across 4 categories, 30-item due diligence checklist, 25+ hard seller questions, and a gated Go/No-Go verdict. Use when: underwrite this apartment deal, run full multifamily due diligence, verify this deal, go deep on this property, we are under contract analyze everything, run the comprehensive underwriting, multifamily DD, deep dive this apartment deal, or any time an investor is under contract or approaching LOI acceptance on a multifamily property and needs full verification before going hard on contingencies. Also use when someone uploads a T12, Schedule E, rent roll, or bank statements and asks you to verify the numbers.
---

# Multifamily Comprehensive Underwriter — Post-Contract Due Diligence

## Overview

This skill runs after an LOI is accepted and the investor is under contract. Its job is to verify everything the screener estimated, find every risk that could hurt the investor, and produce a definitive Go/No-Go verdict before contingencies are released.

**Design philosophy:** Skeptical first, actionable second. This skill does NOT exist to validate a decision already made. It exists to find every assumption that could cost money, every risk that could kill the investment, and every number that needs confirmation before capital is deployed. If the deal is good, the numbers will prove it. If they don't hold up, the investor walks away before losing real money.

**The skill cannot output GO until the Risk Matrix is complete.** Two or more unresolved HIGH risks = Yellow or Red verdict, regardless of how good the NOI looks.

---

## When This Skill Applies

- Investor is under contract on a multifamily property
- Investor needs to verify financials before releasing due diligence contingencies
- Someone says "underwrite this apartment deal," "run full multifamily DD," "deep dive this property"
- Used as Phase 1 after the mf-screener has secured the contract
- Someone uploads a T12, Schedule E, rent roll, or bank statements and asks to verify the numbers

---

## Pre-Flight — Client Investment Preferences

Before running any analysis, load this client's saved multifamily investment preferences from Cloud Brain. These calibrate all thresholds and verdicts to their specific buy box.

### Check for Saved Preferences

Search Cloud Brain: `search_notes` with query `"multifamily preferences"`. Also search for screener analysis: `search_notes` with `"[property name or address] mf screen"` to pull Phase 0 screener output if available.

**If preferences ARE found:**

Display a quick confirmation and proceed:
> 📋 **Underwriting calibrated to your preferences** — Min cash flow: $[X]/mo | Target cap: [X–X]% | Min DSCR: [X]x | Class: [X] | Markets: [X]

**If screener analysis IS found in memory:**
> 📎 **Screener found:** Conservative NOI: $[X] | Viability score: [X]/10 | Recommended offer: $[X] | Pursuit decision: [X] | Top flags: [list]
> Using screener estimates as starting baseline — all figures will be verified against bank statements and Schedule E.

**If NO preferences found — run setup first:**

*"Before we begin underwriting, I need your investment criteria so every threshold and verdict reflects your actual buy box:"*

1. What markets or states are you targeting for multifamily?
2. What asset class do you prefer: Class A / B / C / any?
3. What is your unit count range — minimum and maximum?
4. What is your deal size range — minimum and maximum purchase price?
5. What is your minimum acceptable monthly cash flow after ALL debt service?
6. What cap rate range do you typically target?
7. What is the lowest DSCR you will accept?
8. Do you prefer creative financing first or conventional?
9. Risk tolerance: conservative / moderate / aggressive?

Save to memory: `write_note` → `brain/preferences/multifamily-preferences.md`

### Apply Preferences Throughout This Analysis

| Hardcoded Default | Replace With |
|---|---|
| DSCR thresholds (1.20x / 1.25x) | Client's stated minimum DSCR |
| Cash flow flags ($500/month) | Client's stated monthly minimum |
| CapEx reserve by class | Weight toward client's preferred asset class |
| Cap rate range for valuation matrix | Weight toward client's target cap rate range |
| GO/NO-GO verdict thresholds | Calibrated to client's risk tolerance |
| Offer price recommendations | Anchored to client's target cap rate |

---

## Step 0 — Input Intake & Document Triage

Parse all available documents. For any document not yet provided, add it to the Due Diligence Checklist with HIGH priority.

**Tier 1 — Critical (analysis cannot be completed without these):**
- T-12 P&L (trailing 12 months)
- 24 months of bank statements (revenue and expense verification)
- Current rent roll: all units, tenant names, lease start/end, current rent, security deposit held, any concessions
- Seller's Schedule E (last 2 years of tax returns)
- Existing debt: full loan statement with balance, rate, term, monthly payment, lender name, loan type

**Tier 2 — Required for Full Analysis:**
- Property tax bills for all parcels (verify against the seller's stated expense)
- Insurance policy declaration page (ACV vs. RCV; confirm premium is the actual bound cost)
- Utility bills — any landlord-paid utilities for 12 months
- Maintenance and repair logs (24 months)
- Capital improvement history (5 years)

**Tier 3 — Due Diligence Period Items:**
- Phase I Environmental Site Assessment (or waiver justification)
- Title report (liens, judgments, easements, deed restrictions)
- All permits, certificates of occupancy, and zoning compliance
- Inspection reports: roof, HVAC, plumbing, electrical
- Vacancy history (3 years)

Build a structured property profile from all available inputs. Note confidence level for each data point: Verified / Estimated / Unknown.

---

## Step 1 — Revenue Verification (Challenge Everything)

Revenue is verified in 6 steps. Every step is mandatory.

### 1A — Bank Statement Cross-Check
Compare 24 months of bank deposits to T-12 revenue line by line.
- Calculate: average monthly deposits (12-month trailing)
- Compare to: T-12 gross revenue for the same period
- If deposits < T-12 revenue by more than 5%: **MATERIAL REVENUE DISCREPANCY — flag amount and percentage.**
- State: "Verified Revenue = $[bank deposits]. T-12 Revenue = $[T-12]. Gap = $[difference]."
- Use the LOWER of the two figures for all downstream calculations.

### 1B — Schedule E Cross-Check
Compare bank statement revenue to Schedule E Line 3 (Rents Received).
- A large gap between what the bank shows and what was reported to the IRS is a serious flag — either income is being underreported (tax fraud risk transferred to buyer) or the bank deposits include non-rental income.
- State findings plainly. Do not soften this.

### 1C — Physical Vacancy Verification
From the rent roll:
- Count every vacant unit
- Physical vacancy % = vacant units / total units
- Compare to OM stated vacancy
- If physical > stated: "Actual physical vacancy is [X]%. OM stated [Y]%. This overstates income by $[Z]/year."
- Verify move-in dates — a unit rented 3 weeks ago does not represent stabilized occupancy

### 1D — Rent Roll Audit
For every unit on the rent roll, verify:
- Lease expiration dates (a wall of month-to-month leases is a risk — note count)
- Any units with concessions, free rent, or side agreements that reduce effective rent
- Any units renting above market (flag — likely not renewable at same rate)
- Any units renting significantly below market (flag — value-add opportunity, but also asks why)
- Security deposits held: verify against state legal requirements

### 1E — Other Income Verification
For every non-rent income line:
- Laundry, parking, storage, late fees, application fees, etc.
- Verify against bank statements — is the money actually showing up?
- Flag any Other Income that appears owner-managed (e.g., income a new operator may not replicate)

### 1F — Year-Over-Year Revenue Trend
Plot revenue by year (use 3 years if available):
- Declining trend (2+ years): flag as structural demand or management problem
- Volatile (>15% swings): flag as seasonal or instability risk; stress-test at the low year
- Stable/growing: note positively
- Use 3-year average if current year appears to be an outlier.

**VERIFIED GROSS REVENUE = $[final verified amount]**
List every assumption and every unverified figure with its dollar impact.

---

## Step 2 — Full Expense Normalization

Apply ALL expense lines. For every line not on the T-12, add it and explain why. An expense ratio below 35% (tenant-pays-utilities) is impossible for a fully normalized property.

### Mandatory Normalization Lines

| Expense Line | Calculation Method | Default If Unknown | Flag If |
|-------------|-------------------|--------------------|---------|
| Property Management | 9% of Verified EGI (if self-managed) | 9% of EGI | Missing from T-12 |
| Property Taxes | Verify against tax bills; calculate post-sale reassessment | Pull from county assessor | Using seller's pre-sale bill |
| Insurance | Actual bound policy declaration page | $1,000–$1,500/unit/year | Soft quote / ACV policy |
| Maintenance & Repairs | Actual + 15% first-year cushion | $700/unit/year minimum | Below $500/unit |
| CapEx Reserve | Client's preferred class: A=$300 · B=$400 · C=$500/unit/yr | Client's class default | Missing entirely |
| Landlord Utilities | Actual bills if landlord-pays | Estimate from utility comps | Excluded when landlord pays |
| Landscaping | Actual invoices | $200–$400/unit/year | Suspiciously low |
| Admin / Accounting / Legal | Actual | $300–$500/unit/year | Missing |
| Pest Control | Actual | $75–$150/unit/year | Missing |
| Misc / Contingency | 3% of verified gross revenue | 3% always | Never remove |

### Post-Sale Tax Reassessment (Always Run This)
1. Pull the county assessor's current assessed value and tax bill
2. Pull the millage rate from the county website
3. Calculate: New Assessed Value × Millage Rate = Post-Sale Annual Taxes
4. Show: "Seller's current taxes: $[X]. Post-sale taxes at purchase price of $[Y]: $[Z]. Annual delta: $[difference]."

### Expense Ratio Check After Normalization

| Ratio | Scenario | Verdict |
|-------|----------|---------|
| Below 30% | Tenant-pays-utilities | Impossible — add missing lines |
| 30–40% | Tenant-pays-utilities, Class A | Possible if truly new construction |
| 40–50% | Tenant-pays-utilities, Class B/C | Normal |
| 50–65% | Landlord-pays-utilities | Normal |
| Above 65% | Any | Flag — operational inefficiency or severe deferred maintenance |

**TOTAL NORMALIZED EXPENSES = $[amount] ([X]% expense ratio)**

---

## Step 3 — Three-Scenario NOI

Always produce three NOI figures. Never blend scenarios. Never use Upside NOI as the offer basis.

```
THREE-SCENARIO NOI — [Property Name]

SCENARIO              | FLOOR (Conservative)  | BASE (Stabilized)     | UPSIDE (Value-Add)
----------------------|----------------------|----------------------|----------------------
Revenue Basis         | 3-yr avg OR bank     | Verified current     | All units at market
                      | statements (lower)   | run-rate actuals     | rent, stabilized
Vacancy Used          | [actual low yr] %    | [current physical] % | 5% stabilized
Gross Revenue         | $[amount]            | $[amount]            | $[amount]
Total Normalized Exp  | ($[amount])          | ($[amount])          | ($[amount])
NET OPERATING INCOME  | $[amount]            | $[amount]            | $[amount]
Expense Ratio         | [X]%                 | [X]%                 | [X]%

USE CASE:
- Floor: Stress testing, financing worst-case, walk-away trigger
- Base: Recommended offer price, primary underwriting, DSCR calculations
- Upside: Strategic planning only — never for offer pricing or financing
```

Identify the top 3 drivers of the gap between Floor and Upside NOI.

---

## Step 4 — Valuation Matrix

Build the full matrix using all three NOI scenarios. Assign cap rate range BEFORE showing numbers.

### Cap Rate Selection Rationale

| Factor | Cap Rate Adjustment |
|--------|-------------------|
| Class A, major MSA, strong demand | Start at 4.5–6.0% |
| Class B, suburban / secondary market | Start at 6.5–8.0% |
| Class C, tertiary / value-add | Start at 8.0–10.0% |
| High-crime submarket | Add 1.0–2.0% |
| FEMA flood zone | Add 0.5–1.5% |
| Deferred maintenance evident | Add 0.5–1.0% |
| Rent control jurisdiction | Add 0.5–1.5% |
| No CapEx done in 10+ years | Add 1.0% |

State the selected cap rate range and justify in 2–3 sentences. Note where client's target cap rate range falls.

```
VALUATION MATRIX — [Property Name]
Cap Rate Range: [X]% – [X]% | Client's Target: [X]% – [X]% | Rationale: [justification]

                      | [low cap]%   | [mid cap]%   | [high cap]%
----------------------|--------------|--------------|-------------
FLOOR NOI ($[X])      | $[X÷low]     | $[X÷mid]     | $[X÷high]
BASE NOI ($[X])       | $[X÷low]     | $[X÷mid]     | $[X÷high]
UPSIDE NOI ($[X])     | $[X÷low]     | $[X÷mid]     | $[X÷high]
CONTRACT PRICE ($[X]) | [cap%]       | [cap%]       | [cap%]

Price Per Unit @ Contract: $[contract ÷ units]
Price Per SF @ Contract: $[contract ÷ GBA]
Revenue Multiple @ Contract: $[contract ÷ verified revenue]x
```

State the recommended offer range based on Base NOI and the client's target cap rate range. Flag if the contract price falls outside client's target range.

---

## Step 5 — DSCR & Financing Analysis

Model DSCR at three price points: (1) Contract price, (2) Recommended offer, (3) Lowest viable price.

**Scenario A — Assumed Existing Loan + Seller Carry**
- Assumed loan: $[balance] at [rate]% | Monthly P&I: $[payment]
- Seller carry: $[purchase − assumed balance] at [rate]% I/O | Monthly: $[payment]
- Annual debt service: $[total]
- DSCR = Base NOI ÷ Annual Debt Service = [X]x

**Scenario B — New Conventional (25% Down)**
- Purchase price: $[price] | Down: $[25%] | Loan: $[75%] at [current rate], 30yr amort
- Monthly P&I: $[payment] | Annual: $[× 12]
- DSCR = Base NOI ÷ Annual Debt Service = [X]x

**Scenario C — Agency (Fannie/Freddie, if eligible)**
- LTV: 80% | Rate: current agency rate | 30yr amortization
- Requires: stabilized occupancy (90%+ for 90 days), DSCR ≥ 1.25x, property condition
- DSCR = Base NOI ÷ Annual Debt Service = [X]x

```
DSCR ANALYSIS

Price Point     | Contract ($[X]) | Recommended ($[X]) | Floor ($[X])
----------------|-----------------|--------------------|--------------
Scenario A DSCR | [X]x            | [X]x               | [X]x
Scenario B DSCR | [X]x            | [X]x               | [X]x
Scenario C DSCR | [X]x            | [X]x               | [X]x
Client Min DSCR | [X]x ✅/❌       | [X]x ✅/❌          | [X]x ✅/❌
```

Flag any DSCR below client's minimum OR below 1.0x.
Flag DSCR 1.0–1.20x: "Below conventional minimum. Requires seller carry, larger down payment, or price reduction."

Also model True Cash Flow after all debt service and compare to client's minimum monthly cash flow target.

---

## Step 6 — Risk Matrix

Every identified risk is entered here. The overall deal rating is calculated from this matrix.

**FINANCIAL RISKS**

| Risk | Severity | Finding | Mitigation |
|------|----------|---------|------------|
| Revenue actuals < T-12 stated | HIGH/MED/LOW | [Finding] | [Action] |
| Physical vacancy > stated vacancy | HIGH/MED/LOW | [Finding] | [Action] |
| No management fee on T-12 | HIGH/MED/LOW | [Finding] | [Action] |
| CapEx reserve missing | HIGH/MED/LOW | [Finding] | [Action] |
| Insurance soft-quoted / ACV only | HIGH/MED/LOW | [Finding] | [Action] |
| Post-sale tax reassessment impact | HIGH/MED/LOW | [Finding] | [Action] |
| Maintenance below $500/unit/year | HIGH/MED/LOW | [Finding] | [Action] |
| Only 1 year of financials available | HIGH/MED/LOW | [Finding] | [Action] |
| "Other Income" unverified / owner-dependent | HIGH/MED/LOW | [Finding] | [Action] |
| Schedule E income < bank deposits | HIGH/MED/LOW | [Finding] | [Action] |
| NOI below level needed to meet client's DSCR minimum | HIGH/MED/LOW | [Finding] | [Action] |

**LEGAL & REGULATORY RISKS**

| Risk | Severity | Finding | Mitigation |
|------|----------|---------|------------|
| Rent control / stabilization ordinance | HIGH/MED/LOW | [Finding] | Research city ordinance |
| Zoning non-conforming / grandfathered | HIGH/MED/LOW | [Finding] | Verify rebuild rights |
| Code violations or open permits | HIGH/MED/LOW | [Finding] | Pull permit history |
| Lease expirations clustered near close | HIGH/MED/LOW | [Finding] | Review all lease dates |
| Month-to-month tenants (>30% of units) | HIGH/MED/LOW | [Finding] | Review state tenant laws |
| Undisclosed easements or deed restrictions | HIGH/MED/LOW | [Finding] | Title review |
| Eviction history / problem tenants | HIGH/MED/LOW | [Finding] | Request 3-year history |

**PHYSICAL & INFRASTRUCTURE RISKS**

| Risk | Severity | Finding | Mitigation |
|------|----------|---------|------------|
| Roof age unknown / near end of life | HIGH/MED/LOW | [Finding] | Roof inspection |
| Aluminum wiring (pre-1973 construction) | HIGH/MED/LOW | [Finding] | Licensed electrical inspection |
| Galvanized plumbing (pre-1970 construction) | HIGH/MED/LOW | [Finding] | Plumbing inspection |
| HVAC systems aged / deferred | HIGH/MED/LOW | [Finding] | HVAC inspection |
| FEMA flood zone (anything other than X) | HIGH/MED/LOW | [Finding] | Flood insurance cost |
| Phase I Environmental not completed | HIGH/MED/LOW | [Finding] | Order immediately |
| ADA non-compliance | HIGH/MED/LOW | [Finding] | ADA audit + cost estimate |
| Parking inadequate for unit count | HIGH/MED/LOW | [Finding] | Local code check |
| Deferred exterior / structural maintenance | HIGH/MED/LOW | [Finding] | Licensed inspector |

**MARKET & DEMAND RISKS**

| Risk | Severity | Finding | Mitigation |
|------|----------|---------|------------|
| Outside client's target markets | HIGH/MED/LOW | [Finding] | Confirm client buy box applies |
| Crime score above 60 (scale of 100) | HIGH/MED/LOW | [Finding] | NeighborhoodScout research |
| Population declining in submarket | HIGH/MED/LOW | [Finding] | Census trend analysis |
| Single employer driving local demand | HIGH/MED/LOW | [Finding] | Employer diversification research |
| New supply (units under construction nearby) | HIGH/MED/LOW | [Finding] | Local permit research |
| Rent control legislation pending | HIGH/MED/LOW | [Finding] | City council agenda research |
| Property on market >180 days / prior contract failure | HIGH/MED/LOW | [Finding] | Ask broker for history |

### Overall Deal Risk Rating

| Rating | Criteria | Recommendation |
|--------|----------|----------------|
| 🟢 GREEN | 0 HIGH risks; financials verified; DSCR meets client's minimum | GO HARD — release contingencies |
| 🟡 YELLOW | 1–2 HIGH risks, resolvable; DSCR near client's minimum; some unverified | GO WITH CONDITIONS — specify exactly what must clear |
| 🔴 RED | 3+ HIGH risks unresolved; DSCR below client's minimum; material misrepresentation | DO NOT GO HARD — specify what moves this to Yellow |

**OVERALL DEAL RATING: [GREEN / YELLOW / RED]**

---

## Step 7 — Due Diligence Checklist

Generate a deal-specific, prioritized DD checklist. Every HIGH risk from the Risk Matrix becomes a DD item.

### Pre-Contingency Release (Complete Before Going Hard)

| # | Item | Request From | Status | Priority |
|---|------|-------------|--------|----------|
| 1 | 24 months bank statements — verify revenue | Seller | ☐ | CRITICAL |
| 2 | Schedule E (last 2 years) — cross-check income | Seller / CPA | ☐ | CRITICAL |
| 3 | Current rent roll — all tenants, rates, lease dates, deposits | Seller | ☐ | CRITICAL |
| 4 | Executed leases for all occupied units | Seller | ☐ | CRITICAL |
| 5 | Post-sale property tax assessment — county assessor | County | ☐ | CRITICAL |
| 6 | Hard-bound insurance quote (RCV, not ACV) | Insurance broker | ☐ | CRITICAL |
| 7 | Title report — liens, judgments, easements | Title company | ☐ | CRITICAL |
| 8 | Existing loan statement — balance, rate, term, assumability | Lender | ☐ | CRITICAL |
| 9 | Full building inspection — roof, HVAC, plumbing, electrical | Licensed inspector | ☐ | CRITICAL |
| 10 | Phase I Environmental Site Assessment | Environmental firm | ☐ | CRITICAL |
| 11 | 3-year capital improvement history | Seller | ☐ | HIGH |
| 12 | Maintenance and repair logs (24 months) | Seller | ☐ | HIGH |
| 13 | Utility bills — landlord-paid items only (12 months) | Seller | ☐ | HIGH |
| 14 | Rent payment history — delinquency rate | Seller / PM | ☐ | HIGH |
| 15 | Crime score and trend research — submarket | NeighborhoodScout | ☐ | HIGH |
| 16 | Rent control / tenant protection ordinance verification | City/County | ☐ | HIGH |
| 17 | Zoning compliance certificate | City/County | ☐ | HIGH |
| 18 | Certificate of Occupancy for all buildings | City/County | ☐ | HIGH |
| 19 | Open permits or code violations check | City/County | ☐ | HIGH |
| 20 | Roof inspection and age verification | Roofing contractor | ☐ | HIGH |
| 21 | Electrical inspection (aluminum wiring risk if pre-1973) | Licensed electrician | ☐ | HIGH |
| 22 | Plumbing inspection (galvanized risk if pre-1970) | Licensed plumber | ☐ | HIGH |
| 23 | Insurance claims history (5 years) | Seller / insurance broker | ☐ | MEDIUM |
| 24 | FEMA flood map confirmation | FEMA / surveyor | ☐ | MEDIUM |
| 25 | Vacancy history (3 years) | Seller / PM | ☐ | MEDIUM |
| 26 | Security deposits held — full schedule | Seller | ☐ | MEDIUM |
| 27 | Vendor contracts and any assignable agreements | Seller | ☐ | MEDIUM |
| 28 | Any pending litigation — seller or property | Seller / title search | ☐ | MEDIUM |
| 29 | ADA compliance status | ADA consultant | ☐ | LOW-MEDIUM |
| 30 | Market rent comps — current (3rd party) | Appraisal or PM survey | ☐ | MEDIUM |

Add any deal-specific items from the Risk Matrix.

---

## Step 8 — Seller Question Bank

Generate 25 hard, specific questions tailored to this deal.

**Financial Questions (always required):**
1. Can you provide 24 months of bank statements showing deposits that match your T-12 revenue?
2. What is the effective rent per unit — what does each tenant actually pay after any concessions, move-in deals, delinquencies, or long-term discounts?
3. Your T-12 shows no property management fee. Have you calculated what a buyer would pay a third-party manager?
4. What capital improvements have you made in the last 5 years, and what major replacements do you expect will be needed in the next 5?
5. Are all revenue streams on this T-12 expected to continue post-sale, or is any income tied to your personal relationships or on-site presence?
6. How does this year's revenue compare to the prior 3 years? Is this year representative, above average, or below average?
7. Are there any deferred rent payments, unpaid balances, or tenants in any repayment arrangement?
8. Have you ever had a revenue audit, and would you provide your Schedule E for cross-referencing?
9. Are any of the T-12 expense payments made to related parties, family members, or entities you control?
10. What is your current rent delinquency rate — how many tenants are behind on rent right now?

**Legal & Regulatory Questions (always required):**
11. Is this property in a jurisdiction with any form of rent control, rent stabilization, or just-cause eviction protection?
12. Are all current operating permits current and have you confirmed they are transferable to a new owner?
13. Are any tenants classified as legal tenants with formal eviction protections under state law?
14. Is the property's current use specifically permitted by right, or does it operate under a grandfathered status or conditional use permit?
15. Have you ever received a notice of violation, code enforcement action, or government inquiry related to this property?
16. Are there any pending or threatened legal actions involving the property or any tenant?

**Physical & Infrastructure Questions (tailor to building age):**
17. What is the age and condition of each roof? Have any been replaced, and do you have documentation?
18. (If pre-1973): Has the electrical been inspected for aluminum wiring?
19. (If pre-1970): What type of plumbing is in place — copper, galvanized, PVC? Has it been inspected or replaced?
20. When was the HVAC last serviced or replaced in each unit? Do you have service records?
21. Has a Phase I Environmental Assessment ever been conducted? Are you aware of any underground storage tanks, contamination, or prior industrial use?

**Operational Questions:**
22. Why are you selling now? What would you do differently if you were keeping this property for another 5 years?
23. Is the property currently self-managed or third-party managed? What does your management routine look like?
24. Which, if any, tenants would you describe as problematic, and what is the eviction history for the past 3 years?
25. What do you know about the vacant units — how long have they been vacant, what is the cause, and what does it cost to make them rent-ready?

---

## Step 9 — Go / No-Go Summary

**This section is always the last thing generated.** Never produce the verdict before the Risk Matrix is complete.

```
GO / NO-GO VERDICT — [Property Name]

OVERALL RISK RATING: [GREEN / YELLOW / RED]

VERIFIED NOI: $[Base NOI] (Floor: $[X] | Upside: $[X])
RECOMMENDED OFFER RANGE: $[X] – $[X]
CONTRACT PRICE: $[X] — [above/below/within] recommended range
DSCR AT RECOMMENDED PRICE: [X]x ([Scenario A/B/C]) vs. client minimum of [X]x ✅/❌
TRUE MONTHLY CASH FLOW: $[X]/month vs. client minimum of $[X]/month ✅/❌

THE 3 BIGGEST RISKS:
1. [Most critical — specific, quantified where possible]
2. [Second risk]
3. [Third risk]

THE 3 BIGGEST OPPORTUNITIES:
1. [Most compelling upside — specific, quantified]
2. [Second opportunity]
3. [Third opportunity]

RECOMMENDED NEXT STEPS:
1. [Immediate action]
2. [Second action]
3. [Third action]
Timeline: [X days to complete verification before DD deadline]

VERDICT:
[GO HARD — Release contingencies. All key risks resolved. Numbers verified. Meets client's DSCR and cash flow minimums.]
[GO WITH CONDITIONS — Release contingencies on [specific items]. Hold on [specific items] until [action].]
[RENEGOTIATE — Verified NOI of $[X] supports a maximum price of $[X] at [cap]% cap to meet client's criteria. Current contract price of $[X] requires a reduction of $[amount].]
[WALK — [Specific reason]. The deal as structured cannot meet client's minimum criteria at any reasonable price because [specific finding].]
```

---

## Output Format

Deliver the complete report in this sequence:

1. **PREFERENCES CONFIRMATION** — Client buy box applied, screener findings referenced if available
2. **PROPERTY PROFILE** — Address, unit count, contract price, input confidence level, data sources
3. **RISK ALERT BOX** — Any HIGH or CRITICAL risks appear prominently before the financials
4. **REVENUE VERIFICATION** — All 6 steps, discrepancies flagged, verified revenue stated
5. **EXPENSE NORMALIZATION** — Full normalized schedule with every addition explained
6. **THREE-SCENARIO NOI** — Floor / Base / Upside comparison table
7. **VALUATION MATRIX** — Cap rate table with contract price position, client target highlighted
8. **DSCR & FINANCING** — All scenarios at contract, recommended, and floor prices vs. client minimums
9. **RISK MATRIX** — All 4 categories, rated, with mitigations
10. **DUE DILIGENCE CHECKLIST** — Prioritized, deal-specific, 25–30 items with status boxes
11. **SELLER QUESTION BANK** — 25 hard questions, deal-specific
12. **GO / NO-GO SUMMARY** — Verdict, NOI, offer range, top risks/opportunities, next steps vs. client criteria

**Always save the analysis to:** `brain/deal-analyses/[property-slug]-mf-underwrite-[YYYY-MM-DD].md`

---

## Key Benchmarks

| Metric | Conservative | Standard | Strong |
|--------|-------------|----------|--------|
| Cap rate (Class A major MSA) | 6.0%+ | 4.5–6.0% | Below 4.5% |
| Cap rate (Class B suburban) | 8.0%+ | 6.5–8.0% | Below 6.5% |
| Cap rate (Class C tertiary) | 10.0%+ | 8.0–10.0% | Below 8.0% |
| Expense ratio (tenant pays utilities) | 48%+ | 38–48% | Below 38% |
| Expense ratio (landlord pays utilities) | 60%+ | 50–60% | Below 50% |
| CapEx reserve (Class A) | $300/unit/yr | $350/unit/yr | — |
| CapEx reserve (Class B) | $400/unit/yr | $450/unit/yr | — |
| CapEx reserve (Class C) | $500/unit/yr | $600/unit/yr | — |
| Maintenance minimum | $500/unit/yr | $700/unit/yr | $900/unit/yr |
| Management fee (3rd party) | 10% EGI | 8–9% EGI | 6–8% EGI |
| Physical vacancy (stabilized) | 8%+ | 5–8% | Below 5% |
| DSCR (conventional minimum) | 1.20x | 1.25x | 1.40x+ |
| DSCR (agency Fannie/Freddie) | 1.25x | 1.30x | 1.40x+ |
| True monthly CF/unit | $50 | $150–$250 | $400+ |

---

## Error Handling

- **Bank statements not provided:** Flag prominently. Proceed with T-12 figures but label all NOI as UNVERIFIED ESTIMATES. Add bank statements as #1 DD checklist item.
- **Only 1 year of financials:** Use it but flag. Apply 10% revenue haircut to conservative scenario.
- **Phase I not available:** Add to DD as CRITICAL. Environmental contamination can prevent any financing and create unlimited liability.
- **Rent roll not provided:** Cannot complete a meaningful analysis. Request immediately. State plainly: "Without a current rent roll, income is entirely unverified. Do not release contingencies."
- **Deal looks excellent across all metrics:** Do not lower the guard. State positive findings, then still run every risk check.
- **Seller refuses to provide Schedule E:** Flag as HIGH risk. A seller who won't provide tax returns on a commercial deal is a serious warning sign. Note in Risk Matrix.
- **Client preferences not set:** Run setup interview before any analysis.
- **DSCR meets bank minimums but not client's minimum:** Flag specifically — the deal may be financeable but does not meet client's stated criteria.
