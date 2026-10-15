---
name: cma-builder
description: >-
  Build a complete Comparative Market Analysis to determine home value and pricing strategy — the number one most time-consuming task for real estate agents. From subject property intake through comp selection, adjustment analysis, value conclusion, and a full seller presentation package including comp grid, pricing strategy, and objection handlers for the Zillow conversation. Use whenever a seller asks what their home is worth, you need to recommend a list price, or you need to build a pricing presentation. Triggers on: CMA, comparative market analysis, what should I list for, what is this house worth, pricing analysis, market analysis, comps, comp analysis, what price should I recommend, seller presentation price, listing price, home valuation.
---

# CMA Builder: Real Estate Valuation & Pricing Strategy

> **Disclaimer:** A Comparative Market Analysis (CMA) is a professional pricing tool for real estate agents — it is not a licensed appraisal. CMA values are estimates based on available comparable sales data and agent judgment. They do not constitute a formal property valuation under USPAP or any appraisal standard, and should not be represented as such. Always confirm comparable data against your local MLS and advise clients that final value will be determined by a licensed appraiser if required by their lender. This skill is for agent use — not for direct distribution as an appraisal or formal valuation document.

You're building a **Comparative Market Analysis (CMA)** — the most powerful tool for setting seller expectations, winning listings, and maximizing sale prices.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], Market: [area]. Default adjustments loaded. Proceed or update?"

**If not found:** Run setup interview:

> Let me save your details so every CMA is pre-loaded with your info.
> 1. Your full name?
> 2. Brokerage name?
> 3. Primary market / city?
> 4. Typical comp window in your market (days)? (Common: 90 days)
> 5. Default condition adjustment ranges (or use standard: needs work→avg +3–5%, avg→updated +5–8%, updated→like new +3–5%)?
> 6. Any local market factors to note (hot market, slow inventory, appreciation trend)?

Save to memory:
```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
```

Show preferences banner on every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | Market: [area]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## PHASE 1: SUBJECT PROPERTY INTAKE

Gather complete details. The more accurate the subject profile, the better the comps and adjustments.

**Property Basics:** Address, property type, year built, lot size
**Living Space:** Beds, baths (full + half), above-grade sq ft, below-grade sq ft
**Garage & Parking:** Type (attached/detached/none), car capacity
**Basement:** Finished / partially finished / unfinished / none
**Recent Updates:** Kitchen, bathrooms, roof, HVAC, windows, flooring, exterior (with approximate year)
**Overall Condition:** Needs work / Average / Updated / Like new — any deferred maintenance?
**HOA:** Yes/no + monthly fee
**Special Features:** Pool, view, waterfront, cul-de-sac, patio/deck, bonus room, other

---

## PHASE 2: COMPARABLE SELECTION GUIDANCE

Walk agent through selection criteria before collecting comp data.

**Time Frame:** Ideal = last 90 days (use preferences default). Acceptable = 91–180 days. Flag if older.
**Location:** Tier 1 = same neighborhood. Tier 2 = within 0.5 miles. Tier 3 = same zip/adjacent.
**Size Match:** Within 15–20% of subject sq footage.
**Style & Type:** Match property type as closely as possible.

---

## PHASE 3: COMP INPUT COLLECTION

For each comparable (up to 6): address, sale price, close date, beds/baths, sq footage, DOM, and key differences from subject (condition, garage, basement, updates, special features).

---

## PHASE 4: ADJUSTMENT ANALYSIS

**Price Per Sq Ft Baseline:** Sale Price ÷ Square Footage for each comp.

**Condition Adjustments** (use preferences defaults or standard ranges):
- Needs Work → Average: +3–5%
- Average → Updated: +5–8%
- Updated → Like New: +3–5%

**Feature Adjustments:**
- Pool (in-ground): +2–4%
- Finished Basement (per 100 sq ft): +1–2%
- Garage stall: +3–5% per stall
- View upgrade: +5–15%
- Updated Kitchen: +3–5%
- Updated Bathrooms: +1–3% per bath
- Recent Roof/HVAC: +1–2%
- Cul-de-Sac/Corner Lot: +1–3%

**Time Adjustment** (comps 60+ days old):
- Appreciating market: +0.5–1.5% per month
- Declining market: −0.5–1.5% per month

**Location Adjustment:** Same neighborhood = no adjustment. Adjacent = minimal. Different area = explain and apply +/−2–5%.

---

## PHASE 5: ADJUSTED VALUE CALCULATION

```
Adjusted Sale Price = Sale Price × (1 + Total Adjustments %)
```

Calculate for all comps.

---

## PHASE 6: VALUE CONCLUSION & PRICING RECOMMENDATION

- **Low End:** Average of 2–3 most conservative adjusted values
- **Target:** Average/median of all adjusted comps
- **High End:** Average of 2–3 most aggressive adjusted values

**Recommended Price Range: $[LOW] – $[HIGH] (Target: $[TARGET])**

---

## PHASE 7: PRICING STRATEGY

Present three strategies with pros/cons:

**Strategy 1 — Aggressive (List High):** At or above HIGH end. Best for sellers with time to test the market. Risk: higher DOM, fewer buyers.

**Strategy 2 — Neutral (List at Target):** At the TARGET. Market-supported. Most defensible in negotiations. Recommended default.

**Strategy 3 — Competitive (List to Sell):** At or below LOW end. Best for motivated/time-sensitive sellers. Creates urgency, potential multiple offers. Leaves money on the table.

---

## PHASE 8: CMA PRESENTATION PACKAGE

Generate four deliverables:

### 1. Executive Summary (1 page)
Subject property profile, market summary (comp count, avg DOM, avg $/sq ft, market trend), pricing recommendation, and strategy rationale. All branded with agent name and brokerage from preferences.

### 2. Comparable Grid (Table)
Subject vs. all comps side-by-side: address, sale price, close date, beds, baths, sq ft, DOM, condition, adjustments, adjusted value.

### 3. Pricing Strategy Summary
Three strategies in a table with list price, rationale, expected timeline, and best-fit seller profile.

### 4. Seller Talking Points (Objection Handling)

**"Zillow says it's worth $[more]."**
Zillow uses public data and algorithms; our CMA uses actual recent sales adjusted for your home's specific features. These are the same comps a buyer's appraiser will use — this is the real market value.

**"Can we list higher and negotiate down?"**
Homes priced above market sit longer, attract fewer serious buyers, and risk low appraisals that kill deals. Starting at market value — and letting strong buyer interest push us up — is how we maximize your return.

**"The house down the street sold for more."**
Let's look at that sale. [Review comp differences: condition, size, features, timing.] Markets have variance; our CMA uses multiple data points for reliability.

**"I want to price high to maximize profit."**
Higher listing prices maximize days on market, not profit. Best strategy: price at market value, generate multiple offers, negotiate strongest terms.

---

Save CMA output to memory when requested:
```
mcp__cloud-brain__write_note:
  path: brain/deal-analyses/[address-slug]-cma-[YYYY-MM-DD].md
```

## See Also

- `/listing-launch-checklist` — launch the listing this CMA priced (same plugin)
- `/listing-presentation-builder` — present the CMA to the seller (same plugin)
- `/mkt-programmatic-seo` — generate "homes for sale in {city}" location pages at scale using CMA data (from `marketing-engineering`)
- `/mkt-landing-page` — listing landing page powered by this pricing (from `marketing-engineering`)
