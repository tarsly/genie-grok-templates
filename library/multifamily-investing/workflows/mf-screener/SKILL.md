---
name: mf-screener
description: >-
  Multifamily pre-LOI quick screener — takes a broker OM, T12, rent roll, or any combination of deal documents and produces a rapid acquisition analysis through the eyes of a skeptical lead analyst. Outputs a rent roll reality check, NOI estimate, valuation matrix, creative finance stack (assumption + seller carry + MLO + PML gap), true cash flow after all debt service, 3 offer scenarios, a viability score (1-10), and a draft LOI. Use when: screen this apartment deal, analyze this multifamily OM, what should I offer on this complex, is this deal worth pursuing, quick look at this rent roll, run the numbers on this apartment building, should I send an LOI on this property, analyze this multifamily, or any time a broker sends apartment or multifamily financials and a fast offer decision is needed. Also use when the user drops any multifamily OM, T12, rent roll, or broker package into the conversation without a specific instruction — they almost certainly want a screener.
---

# Multifamily Quick Screener — Pre-LOI Deal Analysis

## Overview


This skill produces a fast, investor-protective analysis of a multifamily deal using whatever financial data is available — OM, T12, rent roll, broker memo, or even a verbal summary. The output is an LOI-ready report with a viability score, red flag audit, creative finance stack, offer scenarios, and draft LOI. Speed and protective skepticism are equally important. Every Pro Forma is treated as fiction until Actuals prove otherwise.

**This is Phase 0 of a two-skill system.** Once under contract, the mf-underwriter skill runs full post-contract due diligence.

---

## The Three Lenses (Apply to Every Deal)

Before running any numbers, frame the analysis through these three perspectives:

**Lens A — The Truth (Actuals):** What does the T-12 or rent roll actually show? Flag any income the seller is counting that tenants aren't paying today. Flag any expense the seller is hiding or underestimating.

**Lens B — The Hope (Value-Add):** What's the gap between current rents and market? What CAPEX is required to get there? Never treat Pro Forma NOI as the offer basis — treat it as the upside roadmap only.

**Lens C — The Exit (Risk):** What kills this deal? Find the building-specific red flags, submarket risks, and structural deal killers before capital is committed.

---

## Pre-Flight — Client Investment Preferences

Before running any numbers, load this client's saved multifamily investment preferences from Cloud Brain. These replace hardcoded defaults throughout the analysis and make every report specific to their buy box.

### Check for Saved Preferences

Search Cloud Brain using `search_notes` with query: `"multifamily preferences"`. If no results, also try `"multifamily buy box"` or `"apartment preferences"`.

**If preferences ARE found:**

Display a confirmation table and ask: *"These are your saved multifamily preferences — shall I use these for this analysis, or would you like to update anything before we start?"*

| Preference | Saved Value |
|---|---|
| Target markets | [from memory] |
| Asset class (A/B/C) | [from memory] |
| Unit count range | [from memory] |
| Deal size range | [from memory] |
| Min monthly cash flow | [from memory] |
| Target cap rate range | [from memory] |
| Min DSCR | [from memory] |
| Financing approach | [from memory] |
| Risk tolerance | [from memory] |

- **Confirmed** → proceed with saved values applied throughout
- **Update requested** → update those specific fields, re-save with `write_note` to `brain/preferences/multifamily-preferences.md`, then proceed

**If NO preferences are found — run this quick setup (required before analysis):**

*"Before we analyze this deal, I need to capture your investment criteria so every report is calibrated to your buy box. Answer these in one message:"*

1. What markets or states are you targeting for multifamily?
2. What asset class do you prefer: Class A / B / C / any?
3. What is your unit count range — minimum and maximum?
4. What is your deal size range — minimum and maximum purchase price?
5. What is your minimum acceptable monthly cash flow after ALL debt service?
6. What cap rate range do you typically target? (e.g., 7–9% for Class B)
7. What is the lowest DSCR you will accept?
8. Do you prefer creative financing first (assumption + seller carry) or conventional?
9. Risk tolerance: conservative / moderate / aggressive?

Save answers immediately before proceeding:
- Tool: `write_note`
- Path: `brain/preferences/multifamily-preferences.md`
- Format: structured markdown with all 9 fields clearly labeled

### Apply Preferences Throughout This Analysis

Replace these defaults with the client's saved values wherever they appear:

| Hardcoded Default | Replace With |
|---|---|
| Cash flow flag ($500/month) | Client's stated monthly minimum |
| DSCR minimums (1.20x / 1.25x) | Client's stated minimum DSCR |
| Cap rate range for offer scenarios | Weighted toward client's target range |
| Asset class benchmarks | Use client's preferred class as primary |
| Offer 1 financing structure | Client's preferred financing approach |
| Pursuit decision language | Calibrated to client's risk tolerance |

Display at the top of every output:
> 📋 **Analysis calibrated to your preferences** — Min cash flow: $[X]/mo | Target cap: [X–X]% | Min DSCR: [X]x | Class: [X] | Markets: [X]

---

## Input Confidence Level

Assess what data is available and state confidence at the top of every output:

| Level | Available Data | Confidence |
|-------|---------------|------------|
| HIGH | T12 + rent roll + asking price + existing debt details | LOI-Ready |
| MEDIUM | OM financials + asking price | LOI with contingencies |
| LOW-MEDIUM | Broker summary or verbal + asking price | Preliminary offer range |
| LOW | Asking price + unit count + location only | Conversation starter only |

Never stop the analysis because data is missing — flag it clearly and apply the conservative default.

---

## Step 0 — Input Intake

Parse and confirm the following. Mark anything unknown as [UNVERIFIED] and apply the conservative default listed.

- Property address, city, submarket
- Unit count (total and operational — flag if discrepancy)
- Unit mix (bed/bath configuration and square footage per type)
- Asking price
- Current gross rents (from rent roll if available)
- Physical vacancy (actual empty units / total units)
- Stated vacancy rate from broker (compare to physical)
- Utilities: who pays? (Tenant-paid vs. landlord-paid — critical for expense modeling)
- Existing debt: balance, rate, monthly payment, lender, loan type (conventional, agency, SBA)
- Owner-managed or third-party PM? (critical for expense normalization)
- Flood zone designation
- Building age / year built
- Seller motivation (flag if unknown — affects structure)
- Broker fee (default 3% if unknown)

Note if deal falls within client's saved preferences for unit count, deal size, asset class, and target markets. Flag any mismatch.

**Web search the property address to pull:**
- Any news, permit history, code violations, or litigation
- Neighborhood crime score (NeighborhoodScout, SpotCrime, or similar)
- Competing rentals nearby and market rent comps
- Any local rent control or tenant protection legislation

---

## Step 1 — Rent Roll Reality Check

This is the single most important step. The broker's stated income is almost never the real income.

### 1A — Physical Vacancy vs. Stated Vacancy
- Count actual vacant units from the rent roll
- Physical vacancy % = vacant units / total units
- Compare to broker's stated vacancy rate
- If physical vacancy > stated vacancy by more than 1 unit: flag it explicitly with the dollar impact. Show: "Broker states [X]% vacancy. Actual physical vacancy is [Y]%. This inflates stated income by $[Z]/year."

### 1B — Current Rents vs. Market Rents
- Identify any units renting significantly below market (flag units more than 10% below market)
- Calculate the rent delta: (market rent − current rent) × 12 × number of below-market units = annual upside
- Label this clearly as Value-Add Potential — NOT a basis for the offer price
- Note any units with suspiciously high rents (above market) — verify lease terms and whether tenant is actually paying

### 1C — Effective Gross Income Reality Check
True current EGI = sum of actual current rents for occupied units only
- Show this clearly: "True current EGI based on occupied units: $[X]/year"
- Compare to broker's stated Potential Gross Rent
- Calculate the gap and flag it

### 1D — Other Income Audit
If the OM includes "Other Income" (laundry, parking, fees, etc.):
- Flag any Other Income above $50/unit/year as unverified until T12 confirms it
- Do not include Other Income in the conservative NOI calculation unless it is documented

---

## Step 2 — Expense Normalization

Apply ALL mandatory expense lines regardless of what the broker's OM shows. An expense line missing from an OM is not a savings — it's a hidden cost.

### Mandatory Expense Additions

| Expense Line | Conservative Default | Flag If |
|-------------|---------------------|---------|
| Property Management | 9% of EGI (if self-managed or no PM listed) | Missing from OM entirely |
| CapEx Reserve | Class A: $300/unit/yr · Class B: $400/unit/yr · Class C: $500/unit/yr | Below $300/unit or missing |
| Maintenance & Repairs | $700/unit/year minimum | Below $500/unit (flag as deferred maintenance risk) |
| Insurance | Use actual if provided; flag "soft quote" vs. bound policy | ACV policy in high-risk market |
| Property Taxes | Use actual; calculate post-sale reassessment (see 2A) | Seller using pre-sale tax bill |
| Vacancy | 5% minimum for stabilized; 8% for value-add | Below 3% — almost always manipulated |

Use client's preferred asset class defaults for CapEx reserve if saved.

### 2A — Post-Sale Tax Reassessment
Never use the seller's current tax bill as the operating expense. Calculate post-sale taxes:
- Look up the current millage rate for the jurisdiction
- Apply millage rate to the new assessed value (typically the purchase price in reassessment states)
- Show: "Seller's current taxes: $[X]. Estimated post-sale taxes at $[purchase price]: $[Y]. Delta: $[difference]/year."

### 2B — Expense Ratio Check
After normalization: Total Expenses / EGI = Expense Ratio
- Below 30%: impossible for a normalized property — add missing lines
- 30–40%: only possible if tenants pay ALL utilities and property is Class A, newer construction
- 40–50%: normal for tenant-pays-utilities Class B/C
- 50–65%: normal for landlord-pays-utilities
- Above 65%: flag — operational problems or severe deferred maintenance

---

## Step 3 — Conservative NOI

```
CONSERVATIVE NOI CALCULATION — [Property Name]

True Current EGI (occupied units only)       $[amount]
Less Vacancy Allowance (5% min)             ($[amount])
Effective Gross Income                       $[amount]

Less Normalized Operating Expenses:
  Property Management (9%)                  ($[amount])
  Property Taxes (post-reassessment)        ($[amount])
  Insurance (hard-bound)                    ($[amount])
  Maintenance & Repairs                     ($[amount])
  CapEx Reserve                             ($[amount])
  [Other actual expense lines]              ($[amount])
Total Expenses                              ($[amount])
Expense Ratio                               [X]%

CONSERVATIVE NOI                            $[amount]
Label: ESTIMATED — subject to T12 and bank statement verification
```

Also calculate an **Upside NOI** (label separately — never use as offer basis):
- All units at market rent
- 5% vacancy
- Same normalized expenses
- Show: "Upside NOI = $[X] — represents stabilized value-add potential. DO NOT use as offer basis."

---

## Step 4 — Valuation Sanity Check

Build a 9-cell cap rate matrix. Highlight where the asking price falls and where the client's target cap rate falls.

### Cap Rate Selection

Select the appropriate range based on the asset and client's preferred class:

| Asset Profile | Cap Rate Range |
|--------------|---------------|
| Class A, major MSA, new construction | 4.5–6.0% |
| Class B, suburban/secondary market, stabilized | 6.0–8.0% |
| Class C, tertiary market, value-add | 8.0–10.0% |
| High-crime submarket, deferred maintenance | Add 1.0–2.0% |
| Flood zone or elevated insurance risk | Add 0.5–1.5% |
| No CapEx done in 10+ years | Add 1.0% |

```
VALUATION MATRIX — [Property Name]
Selected Cap Rate Range: [X]% – [X]% | Client's Target: [X]% – [X]% | Rationale: [2 sentences]

                        | [low]%      | [mid]%      | [high]%
------------------------|-------------|-------------|----------
Conservative NOI ($[X]) | $[X÷low]    | $[X÷mid]    | $[X÷high]
Upside NOI ($[X])       | $[X÷low]    | $[X÷mid]    | $[X÷high]
ASK PRICE ($[X])        | [cap%]      | [cap%]      | [cap%]

Price Per Unit @ Ask: $[ask ÷ units]
Price Per SF @ Ask: $[ask ÷ GBA]
```

State plainly: "At the asking price of $[Z], the implied cap rate on our conservative NOI is [X]%. This is [above/below/at] the appropriate range for this asset class and your target of [X]%."

---

## Step 5 — Creative Finance Stack

For every deal, evaluate all four structures. Build the capital stack that puts the least of our money in while protecting the investor.

If client's preference is conventional-first, model conventional as Structure A and label creative as the alternative.

### Structure A — Assumable Debt
- Is there an existing Fannie/Freddie, SBA, or CMBS loan? What is the rate? What is the remaining term?
- Calculate the Gap: Purchase price − loan balance = gap to fill
- Flag: [UNVERIFIED — must confirm assumability with lender before going hard]
- If rate is below current market: quantify the interest rate savings annually

### Structure B — Seller Carry
Why would this seller carry? Identify their motivation:
- Long hold = low basis = large capital gains hit on sale → installment sale benefit
- Estate/probate situation → steady income preferred
- Distressed/tired landlord → wants out, not max cash

Propose seller carry terms:
- Rate: 5.0–6.0% (below conventional market)
- Structure: interest-only for 3–5 years, then balloon or refinance
- Position: first or second behind assumed debt
- Benefit to seller: installment sale tax treatment, guaranteed income, no management headache

### Structure C — Master Lease Option (MLO)
Evaluate for deals with: high vacancy, below-market rents, or clear mismanagement
- Propose: Control the asset via MLO, fill vacancies and bring rents to market, then exercise purchase at locked price
- MLO monthly payment to seller: slightly below their current net (motivates them to hand over control)
- Option price: locked at today's value — capture the upside after stabilization
- Best for: deals where the Pro Forma cap rate is compelling but current operations don't support the ask

### Structure D — PML / PMP Gap Financing
- Identify the cash gap: down payment + closing costs + any seller carry gap
- Model PML at 10% annualized, interest-only, 24-month bridge
- PMP (equity partner): note equity split instead of interest rate if preferred
- Goal: cover the entire down payment with PML/PMP so the investor uses $0 of their own capital

```
CAPITAL STACK — [Property Name] @ $[Purchase Price]

Source              | Amount      | Purpose                      | Cost/Month  | One-Time Cost
--------------------|-------------|------------------------------|-------------|-------------
Assumed Loan        | $[balance]  | Primary acquisition debt     | $[P&I/mo]   | —
Seller Carry Note   | $[amount]   | Gap between loan and price   | $[I/O pmt]  | —
PML Bridge          | $[amount]   | Down payment / closing costs | $[int/mo]   | $[points]
Buyer Cash          | $[amount]   | Residual gap (minimize this) | —           | $[amount]
--------------------|-------------|------------------------------|-------------|-------------
TOTAL               | $[price]    |                              | $[total/mo] | $[OOP total]
```

---

## Step 6 — True Cash Flow

```
TRUE CASH FLOW — [Property Name]

Conservative NOI                            $[amount]
Less Assumed Loan Annual P&I               ($[amount])
Less Seller Carry Annual Payment (I/O)     ($[amount])
Less PML Annual Interest                   ($[amount])
TRUE ANNUAL CASH FLOW                       $[amount]
TRUE MONTHLY CASH FLOW                      $[amount]

Cash-on-Cash Return = True Annual CF ÷ Total Buyer Cash = [X]%
DSCR = Conservative NOI ÷ Total Annual Debt Service = [X]x

Client's minimum DSCR: [from preferences]
Agency (Fannie/Freddie) minimum DSCR: 1.25x | Conventional minimum DSCR: 1.20x
```

Flag immediately if:
- True Monthly Cash Flow is negative: 🚨 "DEAL DOES NOT CASH FLOW at this price/structure."
- DSCR below client's minimum: 🚨 "Below your stated DSCR threshold of [X]x."
- DSCR below 1.20x: "Deal will not qualify for conventional financing at this price."
- True Monthly CF below client's minimum: ⚠️ "Below your minimum cash flow target of $[X]/month."

---

## Step 7 — Offer Scenarios

Three scenarios side by side. Always show all three.

Lead with client's preferred financing approach as Offer 1:

- **Offer 1 ([Client Preferred]):** Price anchored to conservative NOI at high end of client's target cap rate range. Client's preferred financing structure.
- **Offer 2 (Middle Ground):** Price at mid cap rate. Seller carry primary. PML covers down payment.
- **Offer 3 ([Alternative]):** The opposite financing approach from Offer 1.

```
OFFER SCENARIO COMPARISON

                        | Offer 1 — [Client Preferred] | Offer 2 — Middle     | Offer 3 — [Alternative]
------------------------|------------------------------|----------------------|------------------------
Purchase Price          | $[X]                         | $[X]                 | $[X]
Cap Rate on Cons. NOI   | [X]%                         | [X]%                 | [X]%
Assumed Loan            | $[balance] @ [rate]          | N/A — paid off       | N/A — paid off
Seller Carry            | $[X] @ [rate]% I/O           | $[X] @ [rate]% I/O   | None
PML Bridge              | $[X] @ 10%                   | $[X] @ 10%           | Optional
Down Payment (cash)     | $[X]                         | $[X]                 | $[X] (25%)
Total Cash to Close     | $[X]                         | $[X]                 | $[X]
Monthly Debt Service    | $[X]                         | $[X]                 | $[X]
True Monthly Cash Flow  | $[X]                         | $[X]                 | $[X]
Cash-on-Cash Return     | [X]%                         | [X]%                 | [X]%
DSCR                    | [X]x                         | [X]x                 | [X]x
Meets client min CF?    | ✅/❌                         | ✅/❌                | ✅/❌
Meets client min DSCR?  | ✅/❌                         | ✅/❌                | ✅/❌
Seller Net at Close     | Debt relief + note           | Debt relief + note   | $[X] cash for 1031
Best For                | Client's preferred approach  | Moderate cash needed | Seller wants all cash
```

**Recommended Offer:** State which scenario to lead with and why, referencing how it compares to client's minimum thresholds.

---

## Step 8 — Viability Score and Red Flag Audit

### Viability Score (1–10)

| Score | Meaning |
|-------|---------|
| 1–4 | Pass immediately — too much risk, no creative potential |
| 5–7 | Interesting — needs deep due diligence before LOI |
| 8–10 | High priority — move fast |

Assign the score and write a 2-sentence summary of why this deal works or fails relative to the client's buy box.

### Red Flag Audit (minimum 3 flags)

For each flag:
- State what was found
- Why it matters to the investment thesis
- What needs to be verified before going hard

Always check for these specific deal killers:
- Vacancy rate manipulation (stated vs. physical)
- Missing CapEx reserve
- Insurance: ACV vs. RCV, "soft quote" vs. bound policy
- Post-sale tax reassessment impact
- No property management fee visible (self-managed — add real cost)
- Building age and likely deferred maintenance (roofs, plumbing, HVAC, electrical)
- Submarket risks: crime, population trend, rent control legislation, employer concentration
- Maintenance expense below $500/unit/year — flag as deferred maintenance masking
- "Other Income" above $50/unit/year — flag as unverified until T12 confirms
- Seller motivation unknown — always a risk
- Deal outside client's stated target markets, unit count, or size range

---

## Step 9 — Pursuit Decision + Draft LOI

### Decision Logic

Calibrate to client's saved risk tolerance:
- **Conservative:** Use client's stated minimums strictly. Any high flag = caution.
- **Moderate:** Standard thresholds apply. 1–2 medium flags acceptable.
- **Aggressive:** Allow more creative assumptions. Focus on upside potential.

**GO — SUBMIT LOI:** Conservative NOI positive, DSCR ≥ client's minimum, no unresolved HIGH flags, deal within client's buy box.

**GO WITH CONDITIONS:** Cash flow positive and near client's minimum, 1–2 medium flags present, specific contingency language needed.

**PROCEED WITH CAUTION:** Cash flow below client's minimum OR NOI relies on unverified assumptions. State what must be confirmed before LOI.

**DO NOT PURSUE YET:** Cash flow negative on all scenarios, or 2+ HIGH flags unresolved, or deal falls outside client's buy box criteria. State what price or structure would make this a GO.

---

### LOI Draft Template (output when verdict is GO or GO WITH CONDITIONS)

```
LETTER OF INTENT
To Purchase: [Property Name and Address]

Date: [Today's Date]
From: [Buyer Name / Entity]
To: [Seller Name]

RE: Non-Binding Letter of Intent — [Property Name], [Address]

Dear [Seller Name],

[OPENING — personalize to seller motivation]
IF capital gains concern: "We understand you've held this asset for a significant period and are mindful of the tax implications of a conventional sale. We've structured this offer specifically to address that."
IF tired landlord: "We recognize the demands of managing a portfolio like this and have designed an exit that gives you a clean handoff without sacrificing your long-term proceeds."
DEFAULT: "Thank you for the opportunity to consider [Property Name]. After reviewing the financials, we are pleased to present the following offer."

1. PROPERTY: [Full address], consisting of [unit count] residential units totaling approximately [GBA] square feet, known as [Property Name].

2. PURCHASE PRICE: $[recommended offer price]

3. EARNEST MONEY DEPOSIT: $[EMD amount] to be deposited within [3] business days of mutual execution.

4. FINANCING STRUCTURE:
   [IF ASSUMPTION]: Buyer to assume existing [lender name] loan with approximate balance of $[amount] at existing terms, subject to lender approval. Seller to carry a purchase money note in the amount of $[carry amount] at [rate]% interest-only for [term] years, with a balloon payment at maturity.
   [IF SELLER CARRY ONLY]: Seller to carry purchase money note of $[amount] at [rate]% interest-only for [term] years.
   [IF CONVENTIONAL]: Buyer to obtain new conventional financing with [25]% down. Closing contingent on financing approval within [21] days of execution.
   [IF MLO]: Parties to execute a Master Lease Option Agreement granting Buyer operational control of the property for [24] months at $[monthly] per month, with the exclusive option to purchase at $[price] exercisable at any time during the option period.

5. DUE DILIGENCE PERIOD: [30–45] calendar days from mutual execution. Buyer shall have the right to inspect all physical, financial, legal, and operational aspects of the property.

6. KEY DUE DILIGENCE ITEMS:
   - Review of T-12 P&L and corresponding bank statements
   - Verification of all current leases and tenant payment history
   - Physical inspection of all units and building systems
   - Hard-bound insurance quote (replacement cost value)
   - Confirmation of post-sale property tax assessment
   [Add any specific flags from Step 8 Red Flag Audit]

7. CLOSING: Approximately [45–60] days from execution of Purchase Agreement, subject to financing and due diligence completion.

8. BROKER FEES: [Buyer/Seller/Both] to pay broker commissions per separate agreement.

9. EXCLUSIVITY: Seller agrees to remove the property from the market and cease negotiations with other parties for [21] calendar days from mutual execution.

10. NON-BINDING: This Letter of Intent is non-binding on both parties except for Sections 9 and 11 (Confidentiality), which are binding upon execution.

11. CONFIDENTIALITY: Both parties agree to keep the terms of this LOI confidential.

12. EXPIRATION: This LOI expires if not executed by both parties within [5] business days.

Respectfully submitted,

_______________________________
[Buyer Name]
[Entity Name]
[Phone / Email]
[Date]
```

---

## Step 10 — Document Request & Phase 2 Handoff

Always end with:

```
DOCUMENTS NEEDED BEFORE GOING HARD ON CONTINGENCIES
(Priority order — top items are most critical)

1. T-12 P&L and corresponding bank statements (verify income is real)
2. Current executed leases for all occupied units
3. Hard-bound insurance quote — replacement cost value, not ACV
4. Property tax bills for all parcels + county millage rate
5. Seller's Schedule E (last 2 years tax returns)
6. Maintenance and repair logs (last 24 months)
7. [Any deal-specific items from Step 8 Red Flag Audit]

When the LOI is accepted, run: mf-underwriter
The screener's conservative NOI and capital stack become the starting point for full verification.
```

---

## Save Screener Analysis to Memory

After delivering the full output, save a deal summary to Cloud Brain so the mf-underwriter can reference the screener's findings without starting from scratch.

- Tool: `write_note`
- Path: `brain/deal-analyses/[property-slug]-mf-screen-[YYYY-MM-DD].md`

Include in the saved note:
- Property address, market, and unit count
- Asking price and input confidence level
- Conservative NOI and upside NOI
- Viability score (1–10)
- Recommended offer price and scenario
- Pursuit decision (GO / GO WITH CONDITIONS / CAUTION / DO NOT PURSUE)
- Top 3 red flags triggered
- Capital stack from recommended offer scenario
- Key documents still needed

---

## Output Format

Deliver the complete report in this sequence:

1. **PREFERENCES CONFIRMATION** — Client buy box applied (markets, class, unit range, min CF, target cap, min DSCR)
2. **PROPERTY SNAPSHOT** — Address, unit count, ask price, input confidence level, buy box match check
3. **QUICK RED FLAGS** — Any HIGH flags appear here FIRST before the financials
4. **RENT ROLL REALITY CHECK** — Vacancy, current rents, market delta, EGI truth
5. **CONSERVATIVE NOI** — Expense normalizations, NOI floor and upside
6. **VALUATION MATRIX** — Cap rate table with ask price position, client target highlighted
7. **CREATIVE FINANCE STACK** — Capital stack table
8. **TRUE CASH FLOW** — NOI minus all debt service, monthly/annual, DSCR vs. client minimums
9. **OFFER SCENARIOS** — 3-scenario comparison table
10. **VIABILITY SCORE + RED FLAG AUDIT** — Score, 2-sentence summary, flag detail
11. **PURSUIT DECISION** — GO / GO WITH CONDITIONS / CAUTION / DO NOT PURSUE
12. **DRAFT LOI** — Complete, ready to send (if GO or GO WITH CONDITIONS)
13. **DOCUMENT REQUEST + PHASE 2 HANDOFF** — What we still need

---

## Key Benchmarks

| Metric | Conservative | Standard | Strong |
|--------|-------------|----------|--------|
| Cap rate (Class A, major MSA) | 6.0%+ | 4.5–6.0% | Below 4.5% |
| Cap rate (Class B, suburban) | 8.0%+ | 6.5–8.0% | Below 6.5% |
| Cap rate (Class C, tertiary) | 10.0%+ | 8.0–10.0% | Below 8.0% |
| Expense ratio (tenant pays utilities) | 48%+ | 38–48% | Below 38% |
| Expense ratio (landlord pays utilities) | 60%+ | 50–60% | Below 50% |
| CapEx reserve (Class A) | $300/unit/yr | $350/unit/yr | — |
| CapEx reserve (Class B) | $400/unit/yr | $450/unit/yr | — |
| CapEx reserve (Class C) | $500/unit/yr | $600/unit/yr | — |
| Maintenance (minimum) | $500/unit/yr | $700/unit/yr | $900/unit/yr |
| Management fee | 10% EGI | 8–10% EGI | 6–8% EGI |
| Physical vacancy (stabilized) | 8%+ | 5–8% | Below 5% |
| DSCR (conventional minimum) | 1.20x | 1.25x | 1.40x+ |
| True monthly CF per unit | $50 | $150–$250 | $400+ |

---

## Error Handling

- **No purchase price provided:** Ask immediately — cannot model any scenario without it.
- **No rent roll:** Use stated occupancy and average rent; flag heavily as UNVERIFIED.
- **No T12:** Use broker OM figures but label all NOI as ESTIMATED and apply 10% revenue haircut.
- **Deal produces negative CF on all scenarios:** Do not say "pass." Calculate what price, NOI, or carry terms would make it work at the client's minimum positive CF and show that scenario.
- **Clearly overpriced OM (40%+ gap to value):** State the gap plainly, show the math, and still produce a negotiating LOI if seller motivation warrants it.
- **Deal outside client's buy box:** Flag clearly at the top. Still run the analysis, but note which criteria are not met and whether there's a case to make an exception.
- **Client preferences not yet set:** Run setup interview before any analysis — do not skip.
