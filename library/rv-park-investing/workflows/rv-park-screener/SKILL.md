---
name: rv-park-screener
description: >-
  RV park pre-LOI quick screener — takes a T12, P&L, rent roll, or broker memo and produces a quick NOI estimate, creative finance stack (Gator EMD + PML + seller carry + SBA assumption), true cash flow after all debt service, 2–3 offer scenarios, and a draft LOI. Use when: 'screen this RV park', 'quick look at this T12', 'should I send an LOI', 'what should I offer on this park', 'is this worth pursuing', 'build me an LOI', 'run a quick deal analysis', or any time a broker sends RV park financials and an offer decision is needed fast.
---

# RV Park Quick Screener — Pre-LOI Deal Screener & Offer Tool

## Overview

This skill produces a fast, investor-protective analysis of an RV park deal using whatever financial data is available (T12, P&L, rent roll, broker memo, or even a verbal summary). The output is an LOI-ready report with offer scenarios and a draft Letter of Intent. Speed and protective skepticism are equally important — the goal is to get under contract smart and fast.

**This is Phase 0 of a two-skill system.** Once under contract, the rv-park-underwriter skill runs the comprehensive due diligence.

---

## When This Skill Applies

- Broker sends a T12, P&L, or rent roll and investor needs a quick read
- Investor asks "should I pursue this?" or "what should I offer?"
- Investor needs a draft LOI same day
- Any RV park, campground, or outdoor hospitality property deal analysis
- User says "screen this deal," "quick analysis," "run the numbers," "is this worth it?"

---

## Pre-Flight — Client Investment Preferences

Before running any numbers, load this client's saved RV park investment preferences from Cloud Brain. These replace hardcoded defaults throughout the analysis and make every report specific to their buy box.

### Check for Saved Preferences

Search Cloud Brain using `search_notes` with query: `"rv park preferences"`. If no results, also try `"rv park buy box"`.

**If preferences ARE found:**

Display a confirmation table and ask: *"These are your saved RV park preferences — shall I use these for this analysis, or would you like to update anything before we start?"*

| Preference | Saved Value |
|---|---|
| Target markets | [from memory] |
| Deal size range | [from memory] |
| Min monthly cash flow | [from memory] |
| Target cap rate range | [from memory] |
| Min DSCR | [from memory] |
| Financing approach | [from memory] |
| Risk tolerance | [from memory] |

- **Confirmed** → proceed with saved values applied throughout
- **Update requested** → update those specific fields, re-save with `write_note` to `brain/preferences/rv-park-preferences.md`, then proceed

**If NO preferences are found — run this quick setup (required before analysis):**

*"Before we analyze this deal, I need to capture your investment criteria so every report is calibrated to your buy box. Answer these in one message:"*

1. What markets or states are you targeting for RV parks?
2. What is your deal size range — minimum and maximum purchase price?
3. What is your minimum acceptable monthly cash flow after ALL debt service?
4. What cap rate range do you typically target? (e.g., 9–11%)
5. What is the lowest DSCR you will accept?
6. Do you prefer creative financing first (assumption + seller carry) or conventional?
7. Risk tolerance: conservative / moderate / aggressive?

Save answers immediately before proceeding:
- Tool: `write_note`
- Path: `brain/preferences/rv-park-preferences.md`
- Format: structured markdown with all 7 fields clearly labeled

### Apply Preferences Throughout This Analysis

Replace these defaults with the client's saved values wherever they appear:

| Hardcoded Default | Replace With |
|---|---|
| Cash flow thresholds ($1,500 / $3,000 / $7,500) | Client's stated minimum as the primary benchmark |
| DSCR minimums (1.15x SBA / 1.25x conventional) | Client's stated minimum DSCR |
| Cap rate range for offer scenarios | Weighted toward client's target cap rate range |
| Offer 1 financing structure | Client's preferred financing approach |
| Pursuit decision language | Calibrated to client's risk tolerance |
| Web search market context | Include client's target markets |

Display at the top of every output:
> 📋 **Analysis calibrated to your preferences** — Min cash flow: $[X]/mo | Target cap: [X–X]% | Min DSCR: [X]x | Markets: [X]

---

## Input Confidence Levels

Assess what data is available and set the confidence level at the top of the output:

| Level | Available Data | Confidence |
|-------|---------------|------------|
| HIGH | T12 + rent roll + asking price + existing debt details | LOI-Ready |
| MEDIUM | T12 or P&L + asking price | LOI with contingencies |
| LOW-MEDIUM | Broker memo or summary + asking price | Preliminary offer range |
| LOW | Asking price + site count + location only | Conversation starter only |

For any missing input, flag it clearly and use the conservative default listed in each module. Never stop the analysis because data is missing — flag and continue.

---

## Step 0 — Input Intake

Parse and confirm the following from the user's provided documents or description. For each unknown, note it as [UNVERIFIED] and apply the conservative default.

**Required fields (ask for any that are completely missing):**
- Property address and market description
- Site count — operational (use lower if discrepancy exists)
- Asking price
- Gross annual revenue (from T12, P&L, or rent roll)
- Stated monthly rate per site
- Stated occupancy %
- Existing debt: balance, interest rate, monthly payment, lender name
- Owner-operated or managed? (critical for expense normalization)
- Broker/agent fee (default 4% if unknown)
- Seller motivation (note if unknown — affects LOI strategy)
- Year park was built or opened (for capital reserve calculation)

**Immediately web search the park name + city to pull:**
- Google/Yelp rating and review count
- Any news about the property or market
- Competing parks within 25 miles (note count)
- City/county name for residency ordinance note

---

## Step 1 — Quick NOI Estimate

Perform these 6 normalizations in sequence. Show each adjustment and the dollar amount added or changed. Label all outputs as ESTIMATED until bank statements confirm.

### 1A — Effective Rate Test
Divide gross site revenue by (operational site count × 12 months × stated occupancy %). Compare result to stated monthly rate.
- If calculated effective rate < stated rate by more than 5%: use the calculated effective rate and flag: ⚠️ EFFECTIVE RATE GAP — stated rate may be inflated. Using $[calculated] for this analysis.
- If no site revenue breakdown available: use stated rate × sites × 12 × 70% (conservative default occupancy)

### 1B — Revenue Mix
Separate revenue into streams if data allows:
- Monthly/annual site fees
- Transient (nightly/weekly) income
- Utility reimbursements (flag as unverified pass-through until recapture rate confirmed — apply 80% recapture default)
- Ancillary (store, propane, activities, storage)
- Cabin/glamping revenue (model separately — higher expense ratio ~55–65%)

### 1C — Mandatory Expense Additions (add ALL of these regardless of what P&L shows)

| Expense | Formula | Note |
|---------|---------|------|
| Management Fee | IF owner-operated: 20% of gross revenue | Always normalize — buyer needs this cost |
| Capital Reserves | New build (0–5 yr): $500/site/yr | Mid-age (6–15 yr): $750/site/yr | Older (15+ yr): $1,000/site/yr |
| Insurance | Use actual if provided; else $500/site/yr | Rising market — don't accept low owner figures without verification |
| Repairs & Maintenance | Actual + 20% cushion; minimum $200/site/yr | New build gets lower end |
| Misc/Contingency | 4% of gross revenue | Buffer for unmodeled small expenses |

### 1D — Expense Ratio Check
Calculate: Total Normalized Expenses ÷ Gross Revenue = Expense Ratio
- Below 35%: 🚨 FLAG — impossible for a fully-normalized park. Add a 5% gross revenue contingency line.
- 35–50%: Normal for monthly/annual tenant-heavy park
- 50–70%: Normal for transient-heavy park
- Above 70%: ⚠️ FLAG — investigate operational efficiency

### 1E — Quick NOI Output
```
QUICK NOI FLOOR = Gross Revenue − All Normalized Expenses
```
Show this in a clean table. Label it clearly: **ESTIMATED CONSERVATIVE NOI FLOOR — subject to bank statement verification**

Also calculate an **Upside NOI** (label it separately, NOT used for offer pricing):
- Apply market rate if current rate is below $600/mo (use local comp rate from web search)
- Apply 85% occupancy if current is below that
- Add $30K–$60K transient income if clubhouse/amenities exist or are planned
- Show: "Upside NOI = $[X] — represents stabilized value-add potential. DO NOT use as offer basis."

---

## Step 2 — Valuation Sanity Check

Build a 9-cell value matrix. Highlight where the ask price falls.

```
                    | 7% Cap    | 8% Cap    | 9% Cap    | 10% Cap
--------------------|-----------|-----------|-----------|----------
NOI Floor $[X]      | $[X÷.07]  | $[X÷.08]  | $[X÷.09]  | $[X÷.10]
Upside NOI $[Y]     | $[Y÷.07]  | $[Y÷.08]  | $[Y÷.09]  | $[Y÷.10]
ASK PRICE → $[Z]    | [cap%]    | [cap%]    | [cap%]    | [cap%]
```

State plainly: "At the ask price of $[Z], the implied cap rate on the NOI floor is [X]%. This is [above/below/at] the typical range for this asset class."

If implied cap rate at ask is below 6%: 🚨 **SELLER IS PRICING TO UPSIDE NOI. Offer must be anchored to floor NOI.**

Also calculate Price Per Site: Ask Price ÷ Operational Sites = $[X]/site. Benchmark: rural standard $10,000–$30,000/site. Note if out of range.

Use the client's saved target cap rate range to highlight the column(s) most relevant to their buy box.

---

## Step 3 — Creative Finance Stack Builder

Build the full capital stack for this deal. This is the core of the screener. Model every dollar in and every obligation out.

### The 5-Layer Creative Stack

**LAYER 1 — Gator EMD**
- EMD Amount = 1–2% of purchase price (standard for RV parks)
- Gator Fee = 1–2% of EMD amount (typical) OR flat $500–$1,500
- Example: $2.2M purchase → $22,000–$44,000 EMD → $220–$880 Gator fee
- Note: Gator fee is one-time at contract. EMD returned at close or applied to purchase.
- If no Gator available: buyer brings own EMD. Flag as cash requirement.

**LAYER 2 — Assumed Existing Loan**
- Use actual loan details if provided: balance, rate, monthly P&I
- If not provided: estimate standard SBA 7(a) terms: balance ~$[ask×60%], rate 6–7%, 25-year amortization
- Flag: [UNVERIFIED — must confirm assumability with lender before going hard]
- Monthly debt service = existing P&I payment
- Annual debt service = monthly × 12

**LAYER 3 — Seller Carry Note**
- Amount = Purchase price − Assumed loan balance − Any buyer down payment
- Terms: model at 5–6% interest, interest-only for 3–5 years, then balloon
- Monthly payment = Carry balance × rate ÷ 12 (interest-only)
- Note seller benefit: immediate exit from monthly loan payments, installment sale tax treatment, note income

**LAYER 4 — PML / PMP (Private Money)**
- Use for: closing costs + broker fee + any cash gap
- Closing costs estimate: 1.5% of purchase price
- Broker fee: use actual if known; else 4% of purchase price (buyer side sometimes 0%)
- PML Rate: 10% annualized interest-only (use as default)
- PML Term: 24 months (model as bridge)
- Monthly PML cost = PML balance × 10% ÷ 12
- If using PMP (equity partner): note equity split percentage instead of interest rate

**LAYER 5 — Buyer Cash (Minimize)**
- Any gap not covered by Layers 1–4
- Total Cash to Close = Closing costs + Broker fee + Down payment to seller (if any) + Gator fee − PML proceeds
- Goal: minimize this number. Show it clearly.

### Capital Stack Summary Table
```
CAPITAL STACK — [Property Name] @ $[Purchase Price]

Source              | Amount      | Purpose                    | Cost/Month  | One-Time Cost
--------------------|-------------|----------------------------|-------------|-------------
Gator EMD           | $[amount]   | Earnest money deposit      | $0          | $[fee]
Assumed SBA Loan    | $[balance]  | Primary acquisition debt   | $[P&I/mo]   | —
Seller Carry Note   | $[amount]   | Gap financing              | $[I/O pmt]  | —
PML Bridge          | $[amount]   | Closing costs + fees       | $[int/mo]   | $[points if any]
Buyer Cash          | $[amount]   | Residual gap               | —           | $[amount]
--------------------|-------------|----------------------------|-------------|-------------
TOTAL               | $[price]    |                            | $[total/mo] | $[total OOP]
```

If the client's saved preference is conventional-first, model a conventional structure as Offer 1 and label the creative stack as Offer 2.

---

## Step 4 — True Cash Flow Calculator

This is the number that determines whether to pursue. Show it clearly and prominently.

```
TRUE CASH FLOW CALCULATION — [Property Name]

Gross Effective Revenue (verified/estimated)    $[amount]
− Normalized Operating Expenses                 ($[amount])
= NET OPERATING INCOME (NOI)                    $[amount]
  (This is what the park earns before financing)

− Assumed Loan Annual P&I                       ($[amount])
− Seller Carry Annual Payment (I/O)             ($[amount])
− PML Annual Interest                           ($[amount])
= TRUE ANNUAL CASH FLOW                         $[amount]  ← REAL RETURN
= TRUE MONTHLY CASH FLOW                        $[amount]  ← CASH IN BANK/MO

Cash-on-Cash Return = True Annual Cash Flow ÷ Total Buyer Cash = [X]%
DSCR = NOI ÷ Total Annual Debt Service = [X]x

Client's minimum DSCR: [from preferences]
SBA 7(a) minimum DSCR: 1.15x | Conventional minimum: 1.25x
```

Flag immediately if:
- True Monthly Cash Flow is negative: 🚨 **DEAL DOES NOT CASH FLOW at this price/structure. See offer scenarios for workable prices.**
- DSCR below client's minimum: 🚨 **Below your stated DSCR threshold of [X]x.**
- DSCR below 1.0x: 🚨 **Deal will not qualify for bank financing at this price.**
- True Monthly Cash Flow < client's minimum: ⚠️ **Below your minimum cash flow target of $[X]/mo.**
- True Monthly Cash Flow $1,500–$3,000 (or client's range): ⚠️ **Acceptable but verify assumptions closely.**
- True Monthly Cash Flow exceeds client's minimum: ✅ **Meets your cash flow target.**

---

## Step 5 — Offer Scenario Comparison

Build 3 offer scenarios side by side. Always show all three even if only recommending one.

### Scenario Selection Logic
- **Offer 1 (Recommended):** Anchored to client's preferred financing approach. Price anchored to NOI floor at client's target cap rate range. If client prefers creative: Gator + Assumption + Seller Carry + PML. If client prefers conventional: 25% down, new loan.
- **Offer 2 (Middle Ground):** Price at NOI floor at mid cap rate. Seller carry is primary debt. PML covers down payment gap.
- **Offer 3 (Backup):** The alternative to Offer 1's approach — if Offer 1 is creative, Offer 3 is conventional, and vice versa.

```
OFFER SCENARIO COMPARISON

                        | Offer 1 — [Client Preferred] | Offer 2 — Middle     | Offer 3 — [Alternative]
------------------------|------------------------------|----------------------|----------------------
Purchase Price          | $[X]                         | $[X]                 | $[X]
Cap Rate on NOI Floor   | [X]%                         | [X]%                 | [X]%
EMD (Gator)             | $[X]                         | $[X]                 | $[X]
Gator Fee               | $[X] one-time                | $[X] one-time        | N/A
Assumed Loan            | $[balance] @ [rate]          | N/A — paid off       | N/A — paid off
Seller Carry            | $[X] @ [rate]% I/O           | $[X] @ [rate]% I/O   | None
PML Bridge              | $[X] @ 10%                   | $[X] @ 10%           | Optional
Down Payment (cash)     | $[X]                         | $[X]                 | $[X] (25%)
Total Cash to Close     | $[X]                         | $[X]                 | $[X]
Monthly Debt Service    | $[X]                         | $[X]                 | $[X]
Annual Debt Service     | $[X]                         | $[X]                 | $[X]
True Monthly Cash Flow  | $[X]                         | $[X]                 | $[X]
True Annual Cash Flow   | $[X]                         | $[X]                 | $[X]
Cash-on-Cash Return     | [X]%                         | [X]%                 | [X]%
DSCR                    | [X]x                         | [X]x                 | [X]x
Meets client min CF?    | ✅/❌                         | ✅/❌                | ✅/❌
Meets client min DSCR?  | ✅/❌                         | ✅/❌                | ✅/❌
Seller Net (at close)   | Debt relief + note           | Debt relief + note   | $[X] cash for 1031
Best For                | Client's preferred approach  | Moderate cash needed | Seller wants all cash
```

**Recommended Offer:** State which offer scenario to lead with and why, in 2–3 sentences. Reference how it compares to the client's saved minimum thresholds.

---

## Step 6 — Quick Red Flag Check

Run all 10 checks. Any HIGH flag appears in a 🚨 red box BEFORE the offer recommendation.

| # | Flag | Check | Severity |
|---|------|-------|----------|
| 1 | DSCR below client's minimum | Calculated in Step 4 | 🚨 HIGH |
| 2 | DSCR below 1.0x at ask price | Calculated in Step 4 | 🚨 HIGH |
| 3 | Expense ratio below 35% on seller P&L | Calculated in Step 1D | 🚨 HIGH |
| 4 | Effective rate < stated rate by >10% | Calculated in Step 1A | 🚨 HIGH |
| 5 | No management fee on P&L | Check P&L line items | 🚨 HIGH |
| 6 | SBA assumability not confirmed | Ask broker/seller | ⚠️ MEDIUM-HIGH |
| 7 | Site count discrepancy | Check broker memo vs permit | 🚨 HIGH |
| 8 | Residency ordinance not verified | Web search city + "RV park maximum stay" | ⚠️ MEDIUM-HIGH |
| 9 | Google rating below 3.8 or <20 reviews | Web search in Step 0 | ⚠️ MEDIUM |
| 10 | Park listed >180 days or fell out of contract | Ask broker | ⚠️ MEDIUM |
| 11 | Deal outside client's target market | Check against saved preferences | ⚠️ MEDIUM |
| 12 | Deal outside client's size range | Check against saved preferences | ⚠️ MEDIUM |

For each triggered flag, state:
- What was found
- Why it matters
- What to verify before going hard on contingencies

---

## Step 7 — LOI Draft + Pursuit Decision

### Pursuit Decision Logic

Calibrate thresholds to client's saved risk tolerance:
- **Conservative:** Use client's stated minimums strictly. Any flag = caution.
- **Moderate:** Standard thresholds apply. 1–2 medium flags acceptable.
- **Aggressive:** Allow more creative assumptions. Focus on upside potential.

**GO — SUBMIT LOI:** True cash flow meets or exceeds client's minimum + no unresolved HIGH flags + DSCR meets client's minimum
→ Output full LOI draft

**GO WITH CONDITIONS:** Cash flow positive and near client's minimum + 1–2 medium-high flags present
→ Output LOI draft with contingency language for each flag

**PROCEED WITH CAUTION:** Cash flow below client's minimum OR NOI relies heavily on unverified assumptions
→ Output lower offer price + list what must be verified before LOI

**DO NOT PURSUE YET:** True cash flow negative on ALL scenarios at any reasonable price OR 2+ HIGH flags unresolved
→ State what price/NOI/structure would make this a GO. No LOI output.

---

### LOI Draft Template

When verdict is GO or GO WITH CONDITIONS, generate the following complete LOI:

```
LETTER OF INTENT
To Purchase: [Property Name and Address]

Date: [Today's Date]
From: [Buyer Name / Entity — leave blank for investor to fill]
To: [Seller Name — from intake]

RE: Non-Binding Letter of Intent — [Property Name], [Address]

Dear [Seller Name],

[OPENING — personalize based on seller motivation]
IF 1031 exchange: "We understand you are seeking a transition from your current
debt obligation and a 1031 exchange pathway. We have structured this offer
specifically to address both goals."
IF debt relief: "We recognize the burden of the current loan payments and have
designed this offer to give you immediate relief while maximizing your long-term proceeds."
DEFAULT: "Thank you for the opportunity to consider [Property Name]. After reviewing
the financials, we are excited to present the following offer."

1. PROPERTY: [Full address], consisting of approximately [site count] operational RV sites
   on [acreage] acres, known as [Park Name].

2. PURCHASE PRICE: $[recommended offer price]

3. EARNEST MONEY DEPOSIT: $[EMD amount] to be deposited within [3] business days of
   mutual execution. [IF GATOR: Buyer intends to fund EMD through a short-term capital
   partner; EMD will be in escrow within [5] business days of execution.]

4. FINANCING STRUCTURE:
   [IF ASSUMPTION]: Buyer to assume existing [lender name if known] loan with approximate
   balance of $[amount] at existing terms, subject to lender approval. Seller to carry a
   purchase money note in the amount of $[carry amount] at [rate]% interest-only for
   [term] years, with a balloon payment at maturity.

   [IF SELLER CARRY ONLY]: Seller to carry purchase money note of $[amount] at [rate]%
   interest, [amortizing/interest-only] over [term] years.

   [IF CONVENTIONAL]: Buyer to obtain new conventional financing with 25% down payment.
   Closing contingent on financing approval within [21] days of execution.

5. DUE DILIGENCE PERIOD: [30–45] calendar days from mutual execution. During this period,
   Buyer shall have the right to inspect all physical, financial, legal, and operational
   aspects of the property.

6. DUE DILIGENCE ITEMS: The following items are specifically required during the DD period:
   [AUTO-POPULATE from Step 6 flags — each HIGH/MEDIUM-HIGH flag becomes a named DD item]
   - Verification of permitted site count via approved site plan
   - Confirmation of SBA loan assumability with existing lender [if applicable]
   - Review of 24 months of bank statements confirming reported revenue
   - Verification of municipal residency/maximum stay ordinances [if not confirmed]
   - [Any other flags from Step 6]

7. CLOSING: Approximately [45–90] days from execution of Purchase Agreement, subject to
   financing and due diligence completion.

8. SELLER CARRY TERMS: [rate]% per annum, interest-only, [term] year term, balloon payment
   at maturity. Seller carry note to be secured by a deed of trust on the property in second
   position behind the assumed loan [or in first position if no assumption].

9. BROKER FEES: [Buyer/Seller/Both] to pay broker commissions per separate agreement.

10. EXCLUSIVITY: Seller agrees to remove the property from the market and cease negotiations
    with other parties for [30] calendar days from mutual execution.

11. NON-BINDING: This Letter of Intent is non-binding on both parties, except for the
    Exclusivity (Section 10) and Confidentiality provisions, which are binding upon execution.

12. EXPIRATION: This LOI expires if not executed by both parties within [5] business days.

This offer reflects our genuine interest in creating a transaction that works for both parties.
We are prepared to move quickly and close efficiently.

Respectfully submitted,

_______________________________
[Buyer Name]
[Entity Name]
[Phone]
[Email]
[Date]
```

---

## Step 8 — Phase 2 Handoff

Always end the screener report with this section:

```
PHASE 2 — WHAT TO VERIFY DURING DUE DILIGENCE
(Hand these items to the rv-park-underwriter skill once under contract)

TOP 5 VERIFICATION PRIORITIES FOR THIS DEAL:
1. [Most critical item from Step 6 flags — e.g., "Bank statements confirming $[X] monthly deposits"]
2. [Second priority — e.g., "City Planning confirmation on residency ordinance"]
3. [Third priority — e.g., "SBA lender confirmation of loan assumability and assumption requirements"]
4. [Fourth priority — e.g., "Utility billing records to verify utility recapture rate"]
5. [Fifth priority — e.g., "Permitted site count via approved site plan from county"]

When the LOI is accepted, run: rv-park-underwriter
The screener's NOI estimate and capital stack become the starting point for full verification.
```

---

## Save Screener Analysis to Memory

After delivering the full output, save a deal summary to Cloud Brain so the rv-park-underwriter can reference the screener's findings without starting from scratch.

- Tool: `write_note`
- Path: `brain/deal-analyses/[property-slug]-screen-[YYYY-MM-DD].md`

Include in the saved note:
- Property address, market, and site count
- Asking price and input confidence level
- NOI floor and upside NOI
- Recommended offer price and scenario
- Pursuit decision (GO / GO WITH CONDITIONS / CAUTION / DO NOT PURSUE)
- Top 3 red flags triggered
- Capital stack from recommended offer scenario
- Phase 2 handoff priorities

---

## Output Format

Deliver the complete report inline, structured as follows:

1. **PREFERENCES CONFIRMATION** — Client buy box applied (markets, min CF, target cap, min DSCR)
2. **PROPERTY SNAPSHOT** — Address, sites, ask price, input confidence level, data sources used
3. **QUICK RED FLAGS** (if any HIGH flags — these appear FIRST, before financials)
4. **QUICK NOI ANALYSIS** — Revenue breakdown, expense normalizations, NOI floor and upside
5. **VALUATION SANITY CHECK** — Value matrix, ask price position, price/site
6. **CREATIVE FINANCE STACK** — 5-layer capital stack table
7. **TRUE CASH FLOW** — NOI minus all debt service, monthly and annual, DSCR
8. **OFFER SCENARIOS** — 3-scenario comparison table with recommended offer highlighted
9. **PURSUIT DECISION** — GO / GO WITH CONDITIONS / PROCEED WITH CAUTION / DO NOT PURSUE YET
10. **DRAFT LOI** — Complete, ready-to-send (if GO or GO WITH CONDITIONS)
11. **PHASE 2 HANDOFF** — Top 5 items for the rv-park-underwriter

---

## Key Benchmarks (Always Reference These)

| Metric | Conservative | Standard | Strong |
|--------|-------------|----------|--------|
| Monthly rate/site (rural) | $450–$550 | $550–$700 | $700+ |
| Occupancy (monthly-heavy) | 80% | 88–92% | 95%+ |
| Occupancy (transient-heavy) | 55% | 65–75% | 80%+ |
| Expense ratio (monthly-heavy) | 48–55% | 42–48% | 38–42% |
| Expense ratio (transient-heavy) | 65–70% | 58–65% | 50–58% |
| Cap rate (rural standard park) | 10–12% | 8–10% | 7–8% |
| DSCR (minimum bankable) | 1.15x (SBA) | 1.25x | 1.4x+ |
| True monthly cash flow | $1,500 | $3,000–$5,000 | $7,500+ |
| PML rate (bridge) | 12% | 10% | 8% |
| Seller carry rate | 6–7% | 5–6% | 4–5% |

---

## Error Handling

- **No purchase price provided:** Ask immediately — cannot run any scenario without it.
- **No site count:** Estimate from acreage (standard rural: 8–12 sites/acre); flag heavily.
- **No revenue data at all:** Use RevPAS benchmark ($4,000–$6,000/site/yr for rural park) and label output as BENCHMARK ESTIMATE ONLY.
- **Existing debt unknown:** Model Offer 1 with estimated SBA terms; flag SBA assumability as unconfirmed.
- **Deal produces negative cash flow on all scenarios:** Do not say "pass." Calculate what price, NOI, or terms would make it cash flow at the client's minimum and show that scenario.
- **User provides deal that is clearly overpriced by 40%+:** State the gap plainly, show the math, recommend what price anchors the conversation, and still produce a lowball LOI scenario if the seller motivation warrants.
- **Client preferences not yet set:** Run setup interview before any analysis — do not skip.
