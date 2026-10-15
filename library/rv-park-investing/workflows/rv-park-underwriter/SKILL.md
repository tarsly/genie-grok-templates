---
name: rv-park-underwriter
description: >-
  Comprehensive RV park underwriting — post-contract deep due diligence analysis. Verifies all financials against bank statements and actuals, runs 3-scenario NOI (floor/base/upside), valuation matrix, DSCR at every offer price, formal Risk Matrix across 4 categories, 28-item due diligence checklist, 25+ hard seller questions, and a gated Go/No-Go verdict. Use when: 'underwrite this RV park', 'run full RV park due diligence', 'verify this deal', 'go deep on this park', 'we're under contract — analyze everything', 'run the comprehensive underwriting', 'RV park DD', 'deep dive this RV deal', or any time an investor is under contract or approaching LOI acceptance on an RV park and needs full verification before going hard on contingencies.
---

# RV Park Comprehensive Underwriter — Post-Contract Due Diligence

## Overview

This skill is Phase 1 of the two-skill RV Park analysis system. It runs after an LOI is accepted and the investor is under contract. Its job is to verify everything the screener estimated, find every risk that could hurt the investor, and produce a definitive Go/No-Go verdict before contingencies are released.

**Design philosophy:** Skeptical first, actionable second. This skill does NOT exist to validate a decision already made. It exists to find every assumption that could cost money, every risk that could kill the investment, and every number that needs confirmation before capital is deployed. If the deal is good, the numbers will prove it. If the numbers don't hold up, the investor walks away before losing real money.

**The skill cannot output GO until the Risk Matrix is complete.** Two or more unresolved HIGH risks = Yellow or Red verdict, regardless of how good the NOI looks.

---

## When This Skill Applies

- Investor is under contract on an RV park, campground, or outdoor hospitality property
- Investor needs to verify financials before releasing due diligence contingencies
- Someone says "underwrite this RV park," "run full DD," "deep dive this deal"
- Used as Phase 1 after the rv-park-screener has secured the contract
- Any comprehensive RV park acquisition analysis requiring verified NOI and full risk assessment

---

## Pre-Flight — Client Investment Preferences

Before running any analysis, load this client's saved RV park investment preferences from Cloud Brain. These calibrate all thresholds and verdicts to their specific buy box.

### Check for Saved Preferences

Search Cloud Brain using `search_notes` with query: `"rv park preferences"`. Also check for screener analysis: `search_notes` with `"[property name or slug] screen"` to pull the Phase 0 screener output if available.

**If preferences ARE found:**

Display a quick confirmation and proceed:
> 📋 **Underwriting calibrated to your preferences** — Min cash flow: $[X]/mo | Target cap: [X–X]% | Min DSCR: [X]x | Markets: [X]

**If screener analysis IS found in memory:**
> 📎 **Screener found:** NOI floor: $[X] | Recommended offer: $[X] | Pursuit decision: [X] | Top flags: [list]
> Using screener estimates as starting baseline — all figures will be verified against bank statements.

**If NO preferences found — run setup first:**

*"Before we begin underwriting, I need your investment criteria so every threshold and verdict reflects your actual buy box:"*

1. What markets or states are you targeting for RV parks?
2. What is your deal size range — minimum and maximum purchase price?
3. What is your minimum acceptable monthly cash flow after ALL debt service?
4. What cap rate range do you typically target?
5. What is the lowest DSCR you will accept?
6. Do you prefer creative financing first or conventional?
7. Risk tolerance: conservative / moderate / aggressive?

Save to memory: `write_note` → `brain/preferences/rv-park-preferences.md`

### Apply Preferences Throughout This Analysis

| Hardcoded Default | Replace With |
|---|---|
| DSCR thresholds (1.15x / 1.25x) | Client's stated minimum DSCR |
| Cash flow flags ($1,500 / $3,000) | Client's stated minimum as primary benchmark |
| Cap rate range for valuation matrix | Weight toward client's target cap rate range |
| GO/NO-GO verdict thresholds | Calibrated to client's risk tolerance |
| Offer price recommendations | Anchored to client's target cap rate |

---

## Step 0 — Input Intake & Document Request

Parse all available documents. For any document not yet provided, add it to the Due Diligence Checklist with HIGH priority and note that the analysis is proceeding with unverified assumptions.

**Tier 1 — Critical Documents (analysis cannot be completed without these):**
- 2–3 years of complete P&L statements
- 24 months of bank statements (revenue verification)
- Current rent roll with tenant names, sites, rates, move-in dates
- Existing debt: full loan statement with balance, rate, term, monthly payment, lender name

**Tier 2 — Required for Full Analysis:**
- Property tax bills (county verification)
- Utility bills — water/sewer/trash, electric (common areas)
- Insurance policy declaration page
- Business license, health dept permit, and CUP (if applicable)
- Site plan (permitted site count and layout)
- 3-year occupancy records

**Tier 3 — Due Diligence Period Documents:**
- Phase I Environmental Site Assessment
- Well/water test results (if not on municipal)
- Septic records (if not on city sewer)
- Title report
- All permits and licenses
- Reservation system export (booking history)
- Staffing list with roles and compensation

Build a structured property profile from all available inputs. Note confidence level for each data point (Verified / Estimated / Unknown).

---

## Step 1 — Revenue Verification (Challenge Everything)

Revenue is verified in 7 steps. Each step is mandatory. Do not accept any revenue figure as final until all 7 steps are complete.

### 1A — Bank Statement Revenue Cross-Check
Compare 24 months of bank deposits to P&L revenue line by line.
- Calculate: Average monthly deposits (12-month trailing)
- Compare to: P&L gross revenue for same period
- If deposits < P&L revenue by >5%: 🚨 **MATERIAL REVENUE DISCREPANCY. Flag amount and %.**
- State: "Verified Revenue = $[bank deposits]. P&L Revenue = $[P&L]. Gap = $[difference]."
- Use the LOWER of the two figures for all downstream calculations.

### 1B — Effective Rate Test
Divide verified monthly site deposit revenue by (operational sites × stated occupancy %).
- If result < stated monthly rate: **Effective Rate = $[calculated]. Stated Rate = $[owner stated]. Gap = $[difference]/site/month.**
- Annualized impact of rate gap: (Stated − Effective) × sites × 12 × occupancy = $[amount] revenue inflation
- Use effective rate only for all NOI calculations.

### 1C — RevPAS Calculation
Annual verified revenue ÷ total operational sites = RevPAS
- Benchmark: KOA franchisees ~$7,000/site/yr | Sun Communities ~$10,000/site/yr | Rural standard ~$3,000–$5,000/site/yr
- Flag if RevPAS is more than 30% above benchmark: may indicate one-time revenue or occupancy inflation.

### 1D — Revenue Mix Analysis
Identify and separate all revenue streams:

| Stream | Annual Amount | % of Total | Risk Rating |
|--------|-------------|------------|-------------|
| Monthly site fees | $[X] | [X]% | LOW (stable) |
| Transient (nightly/weekly) | $[X] | [X]% | MEDIUM-HIGH (seasonal) |
| Utility reimbursements | $[X] | [X]% | Flag recapture rate |
| Cabin/glamping | $[X] | [X]% | MEDIUM (higher opex) |
| Ancillary (store/propane/activities) | $[X] | [X]% | MEDIUM (owner-dependent risk) |
| Storage | $[X] | [X]% | LOW (passive) |

Flag if >80% from any single stream (concentration risk).
Flag if ancillary revenue appears owner-dependent (won't survive sale).

### 1E — Utility Recapture Verification
If electricity is billed back to tenants via sub-metering:
- Request: utility billing records for 12 months — billed to tenants vs. collected
- Calculate actual recapture rate: Collections ÷ Total Billed = [X]%
- If recapture rate < 95%: the shortfall is an expense, not a pass-through.
- Net utility cost = Total billed × (1 − recapture rate) = $[X]/year → add to expense schedule
- Default if records unavailable: apply 80% recapture; flag as unverified.

### 1F — Transient Income Verification
Compare stated transient income to reservation system export or booking records.
- If transient income is owner-stated without booking records: flag as unverified; use 50% of stated amount as conservative figure.
- Check: Is transient income deliberately suppressed (e.g., no online booking pending clubhouse)? If yes, model current actual and note upside potential separately.

### 1G — Year-Over-Year Revenue Trend
Plot 3-year revenue by category:
- Declining trend (2+ years): 🚨 Flag — structural demand problem or management issue
- Volatile (>15% swings): ⚠️ Flag — seasonal instability; stress-test at low year
- Stable/growing: ✅ Note positively
- Use 3-year average if current year appears to be an outlier (high or low).

**VERIFIED GROSS REVENUE = $[final verified amount]**
List every assumption made and every unverified figure with its impact.

---

## Step 2 — Full Expense Normalization

Apply ALL expense lines. For every line not on the P&L, add it and explain why. An expense ratio below 35% is impossible for a fully-normalized operating park — add missing lines until the ratio is defensible.

### Mandatory Normalization Lines

| Expense Line | Calculation | Verification Method | Default If Unknown |
|-------------|-------------|--------------------|--------------------|
| **Property Taxes** | Verify against county assessor records | Pull county tax records via web search | Owner-stated with county URL citation |
| **Insurance** | Actual policy declaration page | Request policy; $500/site/yr if unavailable | $500/site/yr (rising market) |
| **Water/Sewer/Trash** | 12-month utility bill average | Request bills | $800–$1,200/month for 50-site park |
| **Management Fee** | IF owner-operated: 20% of verified gross revenue | — | ALWAYS add — 20% is industry midpoint |
| **Payroll/Labor** | Cross-check W-2s/1099s against bank outflows | Request payroll records | Include work camper market-value labor |
| **Repairs & Maintenance** | Actual + 20% first-year buyer cushion | Review maintenance logs | Minimum $200/site/yr (new build) to $500/site/yr |
| **Capital Reserves** | New build (0–5 yr): $500/site/yr | Mid-age (6–15 yr): $750/site/yr | Older (15+ yr): $1,000–$1,500/site/yr |
| **Utilities — Common Areas** | Actual from bills | Electric, internet, propane if applicable | Estimate $300–$600/month |
| **Net Utility Cost (recapture shortfall)** | From Step 1E | Verified from billing records | Apply 20% shortfall default if unverified |
| **Marketing / Reservation Software** | Actual + planned increase | Review current spend | $6,000–$15,000/yr |
| **Admin / Accounting / Legal** | Actual | Review bank disbursements | $7,000–$12,000/yr |
| **Landscaping** | Actual | Review vendor invoices | $4,000–$8,000/yr for 40–80 site park |
| **Misc/Contingency** | 4% of verified gross revenue | — | Always include — do not remove |

### Expense Ratio Benchmarks
After normalization, calculate: Total Expenses ÷ Verified Gross Revenue = Expense Ratio

| Expense Ratio | Park Type | Verdict |
|--------------|-----------|---------|
| Below 35% | Any | 🚨 IMPOSSIBLE — add missing expense lines |
| 35–48% | Monthly/annual-heavy | ✅ Normal |
| 48–65% | Transient-heavy or mixed | ✅ Normal |
| 65–72% | Transient-heavy with high labor | ⚠️ Acceptable — monitor for efficiency |
| Above 72% | Any | 🚨 Operational problems — investigate |

**TOTAL NORMALIZED EXPENSES = $[amount] ([X]% expense ratio)**
List every addition, the reason it was added, and the dollar amount.

---

## Step 3 — Three-Scenario NOI

Always produce three NOI figures. Label them clearly. Never blend scenarios.

```
THREE-SCENARIO NOI — [Property Name]

SCENARIO                | FLOOR (Conservative)  | BASE (Stabilized)     | UPSIDE (Value-Add)
------------------------|----------------------|----------------------|----------------------
Revenue Basis           | 3-yr avg OR bank     | Verified current     | Post-improvement
                        | statements (lower)   | run-rate actuals     | market rate + amenities
Occupancy Used          | [actual low yr] %    | [current verified] % | 88–92% stabilized
Monthly Rate Used       | $[effective rate]    | $[effective rate]    | $[market rate]
Gross Revenue           | $[amount]            | $[amount]            | $[amount]
Total Normalized Expenses| ($[amount])         | ($[amount])          | ($[amount+])
NET OPERATING INCOME    | $[amount]            | $[amount]            | $[amount]
Expense Ratio           | [X]%                 | [X]%                 | [X]%

USE CASE:
- Floor: Financing applications, stress test, minimum offer price
- Base: Primary underwriting, recommended offer price, DSCR calculations
- Upside: Strategic planning ONLY — never for offer pricing or financing
```

State clearly: "All offer prices and financing analysis use the BASE NOI. The FLOOR NOI is used for stress testing. The UPSIDE NOI is shown for informational purposes only — it is not a justified offer basis."

Identify and document the top 3 drivers of the gap between FLOOR and UPSIDE NOI.

---

## Step 4 — Valuation Matrix

Build the full valuation matrix using all three NOI scenarios.

### Cap Rate Selection Rationale
Assign the appropriate cap rate range BEFORE showing the numbers, based on:

| Factor | Cap Rate Adjustment |
|--------|-------------------|
| Class A resort / destination / major MSA (< 150 mi) | Start at 6–7% |
| Standard park / solid market / good occupancy | Start at 8–9% |
| Rural / seasonal / single-attraction / older infra | Start at 10–12% |
| New construction (< 5 yr) | Subtract 0.5–1.0% |
| Missing amenities (unbuilt clubhouse, no 50A) | Add 0.5–1.0% |
| Below 3.8 Google rating | Add 1.0% |
| FEMA flood zone | Add 1.0–2.0% |

State the selected cap rate range and justify it in 2–3 sentences. Note where client's target cap rate range falls relative to the selected range.

```
VALUATION MATRIX — [Property Name]
Selected Cap Rate Range: [X]% – [X]% | Client's Target: [X]% – [X]% | Rationale: [brief justification]

                     | [low cap]%  | [mid cap]%  | [high cap]%
---------------------|-------------|-------------|-------------
FLOOR NOI ($[X])     | $[X÷low]    | $[X÷mid]    | $[X÷high]
BASE NOI ($[X])      | $[X÷low]    | $[X÷mid]    | $[X÷high]
UPSIDE NOI ($[X])    | $[X÷low]    | $[X÷mid]    | $[X÷high]
CONTRACT PRICE ($[X])| [cap%]      | [cap%]      | [cap%]

Price Per Site @ Contract: $[contract ÷ sites] | Industry range: $10,000–$30,000/site
Revenue Multiple @ Contract: $[contract ÷ verified revenue]x | Industry range: 2.0–4.0x
```

State the recommended offer range based on Base NOI and client's target cap rate range.

---

## Step 5 — DSCR & Financing Analysis

Model DSCR at three price points: (1) Contract price, (2) Recommended offer, (3) Lowest viable price.

**Scenario A — SBA 7(a) Assumption + Seller Carry**
- Assumed loan balance: $[balance] at [rate]% | Monthly P&I: $[payment]
- Seller carry: $[purchase − assumed balance] at [rate]% I/O | Monthly: $[payment]
- Annual debt service: $[total]
- DSCR = Base NOI ÷ Annual Debt Service = [X]x
- Client's minimum DSCR: [X]x | SBA 7(a) minimum: 1.15x | Conventional minimum: 1.25x

**Scenario B — New Conventional (25% Down)**
- Purchase price: $[price] | Down: $[25%] | Loan: $[75%] at [7–8%], 25 yr
- Monthly P&I: $[payment] | Annual: $[payment × 12]
- DSCR = Base NOI ÷ Annual Debt Service = [X]x

**Scenario C — USDA B&I (if rural eligible)**
- 80% LTV | Down: 20% | Rate: current USDA rate | 25–30 yr amortization
- DSCR = Base NOI ÷ Annual Debt Service = [X]x

```
DSCR ANALYSIS

Price Point    | Contract ($[X]) | Recommended ($[X]) | Floor ($[X])
---------------|-----------------|--------------------|--------------
Scenario A DSCR| [X]x            | [X]x               | [X]x
Scenario B DSCR| [X]x            | [X]x               | [X]x
Scenario C DSCR| [X]x            | [X]x               | [X]x
Client Min DSCR| [X]x ✅/❌       | [X]x ✅/❌          | [X]x ✅/❌
```

🚨 Flag any DSCR below client's minimum OR below 1.0x.
⚠️ Flag DSCR between 1.0–1.15x.
✅ DSCR at or above client's minimum: "Meets your stated DSCR requirement."

Also model True Cash Flow (NOI minus ALL debt service) and compare to client's minimum monthly cash flow target.

---

## Step 6 — Risk Matrix

Every identified risk is entered here. The overall deal rating is CALCULATED from this matrix — not from gut feel.

### Risk Categories & Items

**FINANCIAL RISKS**

| Risk | Severity | Finding | Mitigation |
|------|----------|---------|------------|
| Revenue actuals < P&L stated | [HIGH/MED/LOW] | [Finding from Step 1A] | [Action] |
| Effective rate < stated rate | [HIGH/MED/LOW] | [Finding from Step 1B] | [Action] |
| Expense ratio below 35% before normalization | [HIGH/MED/LOW] | [Finding from Step 2] | [Action] |
| Management fee missing from P&L | [HIGH/MED/LOW] | [Finding] | [Action] |
| Capital reserves missing | [HIGH/MED/LOW] | [Finding] | [Action] |
| Only 1 year of financials available | [HIGH/MED/LOW] | [Finding] | [Action] |
| Revenue appears owner-dependent | [HIGH/MED/LOW] | [Finding] | [Action] |
| Related-party expenses on P&L | [HIGH/MED/LOW] | [Finding] | [Action] |
| Deferred bookings/deposits as liability | [HIGH/MED/LOW] | [Finding] | [Action] |
| NOI below level needed to meet client's DSCR minimum | [HIGH/MED/LOW] | [Finding] | [Action] |

**LEGAL & REGULATORY RISKS**

| Risk | Severity | Finding | Mitigation |
|------|----------|---------|------------|
| Residency / maximum stay ordinance | [HIGH/MED/LOW] | [Finding] | Call city planning pre-LOI |
| Commercial zoning without RV park CUP | [HIGH/MED/LOW] | [Finding] | Verify permitted use |
| Legal non-conforming (grandfathered) status | [HIGH/MED/LOW] | [Finding] | Verify rebuild rights |
| Permits not current or non-transferable | [HIGH/MED/LOW] | [Finding] | Request copies + confirmation |
| Monthly tenants with legal tenant protections | [HIGH/MED/LOW] | [Finding] | Review state tenant law |
| Rent increase restrictions by state law | [HIGH/MED/LOW] | [Finding] | Research state statutes |
| Work camper employees — employment law risk | [HIGH/MED/LOW] | [Finding] | Legal review |
| Undisclosed easements or access issues | [HIGH/MED/LOW] | [Finding] | Title review |

**PHYSICAL & INFRASTRUCTURE RISKS**

| Risk | Severity | Finding | Mitigation |
|------|----------|---------|------------|
| Private well — not on municipal water | [HIGH/MED/LOW] | [Finding] | Water quality + capacity test |
| Private septic / package plant | [HIGH/MED/LOW] | [Finding] | County records + capacity |
| Electrical pedestals 30-amp only | [HIGH/MED/LOW] | [Finding] | Estimate upgrade cost |
| FEMA flood zone location | [HIGH/MED/LOW] | [Finding] | Survey + flood insurance cost |
| Phase I Environmental not completed | [HIGH/MED/LOW] | [Finding] | Order immediately |
| Wetlands on or adjacent to property | [HIGH/MED/LOW] | [Finding] | Army Corps review |
| Roads in poor condition | [HIGH/MED/LOW] | [Finding] | Civil engineer inspection |
| Unbuilt clubhouse or promised amenities | [HIGH/MED/LOW] | [Finding] | Contractor bid required |
| ADA non-compliance | [HIGH/MED/LOW] | [Finding] | ADA audit + cost estimate |
| WiFi infrastructure absent | [HIGH/MED/LOW] | [Finding] | IT assessment + budget |

**MARKET & DEMAND RISKS**

| Risk | Severity | Finding | Mitigation |
|------|----------|---------|------------|
| Outside client's target markets | [HIGH/MED/LOW] | [Finding] | Confirm client buy box applies |
| Single-attraction market | [HIGH/MED/LOW] | [Finding] | Research backup demand drivers |
| > 150 miles from major MSA | [HIGH/MED/LOW] | [Finding] | Drive-time demand analysis |
| 3+ direct competitors within 25 miles | [HIGH/MED/LOW] | [Finding] | Competitive analysis |
| Google rating below 3.8 / <20 reviews | [HIGH/MED/LOW] | [Finding] | Full review analysis |
| Park listed >180 days / prior contract failure | [HIGH/MED/LOW] | [Finding] | Investigate prior DD findings |
| Occupancy declining 2+ years | [HIGH/MED/LOW] | [Finding] | Root cause analysis |

### Overall Deal Risk Rating

Count HIGH risks:

| Rating | Criteria | Recommendation |
|--------|----------|----------------|
| 🟢 GREEN | 0 HIGH risks; all financials verified; DSCR meets client's minimum | GO HARD — release contingencies |
| 🟡 YELLOW | 1–2 HIGH risks, resolvable; DSCR near client's minimum; some unverified | GO WITH CONDITIONS — specify exactly what must clear |
| 🔴 RED | 3+ HIGH risks unresolved; DSCR below client's minimum at recommended price; material misrepresentation suspected | DO NOT GO HARD — specify what would move to Yellow |

**OVERALL DEAL RATING: [GREEN / YELLOW / RED]**
State the 3 biggest risks and 3 biggest opportunities in plain language.

---

## Step 7 — Due Diligence Checklist

Generate a deal-specific, prioritized DD checklist. Every item from the Risk Matrix becomes a DD item. Add all standard items below.

### Pre-Contingency Release (Must Complete Before Going Hard)

| # | Item | Request From | Status | Priority |
|---|------|-------------|--------|----------|
| 1 | 24 months of bank statements — verify revenue | Seller | ☐ | CRITICAL |
| 2 | Residency ordinance verification — call city/county planning | Municipality | ☐ | CRITICAL |
| 3 | Permitted site count — obtain approved site plan | City/County Planning | ☐ | CRITICAL |
| 4 | SBA loan assumability — contact lender directly | Existing Lender | ☐ | CRITICAL |
| 5 | Utility recapture rate — billing records | Seller / Utility | ☐ | CRITICAL |
| 6 | Google and Yelp full review analysis | Public / Web | ☐ | HIGH |
| 7 | 3 years of P&L and tax returns | Seller / CPA | ☐ | CRITICAL |
| 8 | Rent roll — all tenants, sites, rates, move-in dates | Seller | ☐ | CRITICAL |
| 9 | Phase I Environmental Site Assessment | Environmental Firm | ☐ | CRITICAL |
| 10 | Water/well test — potability, capacity, county records | Licensed Inspector | ☐ | HIGH (if private well) |
| 11 | Septic design approval and pump records | County Health Dept | ☐ | HIGH (if private septic) |
| 12 | Electrical inspection — pedestal amperage, panel condition | Licensed Electrician | ☐ | HIGH |
| 13 | Title report — liens, judgments, easements, CC&Rs | Title Company | ☐ | CRITICAL |
| 14 | All current permits + transferability confirmation | Seller | ☐ | CRITICAL |
| 15 | Zoning compliance certificate + CUP if applicable | City/County | ☐ | CRITICAL |
| 16 | Clubhouse/construction cost estimate — independent bid | General Contractor | ☐ | HIGH (if unbuilt) |
| 17 | Adjacent parcel pricing (if expansion planned) | Owner / Realtor | ☐ | MEDIUM |
| 18 | Road and infrastructure — civil engineer inspection | Civil Engineer | ☐ | HIGH |
| 19 | FEMA flood zone determination | Surveyor / FEMA maps | ☐ | CRITICAL |
| 20 | Reservation system — data and system transferability | Software Vendor | ☐ | HIGH |
| 21 | Staffing — who stays, at what terms | Seller / Employees | ☐ | HIGH |
| 22 | Insurance claims history — 5 years | Insurance Broker | ☐ | MEDIUM |
| 23 | Capital improvement history — last 5 years | Seller | ☐ | HIGH |
| 24 | Deferred bookings and deposits — full liability schedule | Seller | ☐ | HIGH |
| 25 | Propane/LP gas system inspection (if present) | Licensed Inspector | ☐ | MEDIUM |
| 26 | ADA compliance audit | ADA Consultant | ☐ | MEDIUM |
| 27 | WiFi/internet infrastructure assessment | IT Consultant | ☐ | LOW-MEDIUM |
| 28 | Competing park analysis — rates, occupancy, amenities within 25 miles | Web Research | ☐ | HIGH |

Add any deal-specific items from the Risk Matrix flags above.

---

## Step 8 — Seller Question Bank

Generate 25 specific, hard questions tailored to this deal. These are NOT friendly questions. They are designed to surface information the seller may not volunteer.

Always include these core questions — add deal-specific ones from the Risk Matrix:

**Financial Questions (always required):**
1. Can you provide 24 months of bank statements showing deposits matching your P&L revenue?
2. What is the actual effective monthly rate per site — what do tenants actually pay after any long-term discounts, move-in deals, or delinquencies?
3. Your P&L shows no management fee. Have you calculated what a buyer would need to pay for management?
4. What capital improvements have you made in the last 5 years, and what major repairs do you anticipate in the next 5?
5. What is the exact utility recapture rate — what percentage of tenant-billed electricity do you actually collect?
6. Are all revenue streams on this P&L expected to continue post-sale, or is any revenue tied to your personal relationships or presence?
7. How does this year's revenue compare to the prior 3 years? Is this year representative, above average, or below average?
8. Are there any deferred bookings, pre-paid reservations, or deposits that would transfer as liabilities at close?
9. Have you ever had a revenue audit, and would you provide your most recent tax returns for cross-referencing?
10. Are any of the P&L expenses paid to family members, related businesses, or entities you control?

**Legal & Regulatory Questions (always required):**
11. Has the city or county ever communicated any maximum stay or residency ordinance restrictions? Has any tenant ever been told they must leave after a maximum period?
12. Are all current operating permits current and have you confirmed in writing with each issuing authority that they are transferable?
13. Are any of your monthly tenants classified as legal tenants with formal eviction protections under state law?
14. Is the property's current use specifically permitted by right, or is it operating under a grandfathered status or conditional use permit?
15. Have you ever received any notice of violation, code enforcement action, or government inquiry related to this property?

**Infrastructure Questions (add based on property):**
16. (If private well) What is the most recent water quality test result, what is the well's tested GPM capacity, and what is the county maintenance history?
17. (If private septic) What is the design capacity, when was it last pumped, and does it have county approval for the current site count?
18. (If clubhouse unbuilt) Have permits been pulled? What is your contractor's bid? Why hasn't it been built yet?
19. (If expansion planned) Have you had conversations with the adjacent parcel owner?
20. Has the property ever flooded, and does the current mortgage require flood insurance?
21. Has a Phase I Environmental been conducted? Are you aware of any underground storage tanks or contamination?

**Operational Questions:**
22. Which employees or contractors are currently working here, and which would you expect to stay after a sale?
23. Is your reservation system transferable, and does the guest database and booking history transfer?
24. Are any of your current tenants behind on rent? What is your current delinquency rate?
25. Why are you selling now rather than waiting for the park to be fully stabilized?

---

## Step 9 — Go / No-Go Summary

**This section is always the last thing generated.** Never generate the verdict before the Risk Matrix is complete.

```
GO / NO-GO VERDICT — [Property Name]

OVERALL RISK RATING: [GREEN / YELLOW / RED]

VERIFIED NOI: $[base NOI] (Floor: $[X] | Upside: $[X])
RECOMMENDED OFFER RANGE: $[X] – $[X]
CONTRACT PRICE: $[X] — [above/below/within] recommended range
DSCR AT RECOMMENDED PRICE: [X]x ([Scenario A/B/C]) vs. client minimum of [X]x ✅/❌
TRUE MONTHLY CASH FLOW: $[X]/month vs. client minimum of $[X]/month ✅/❌

THE 3 BIGGEST RISKS:
1. [Most critical risk — specific, not generic]
2. [Second risk]
3. [Third risk]

THE 3 BIGGEST OPPORTUNITIES:
1. [Most compelling upside — specific, quantified where possible]
2. [Second opportunity]
3. [Third opportunity]

RECOMMENDED NEXT STEPS:
1. [Immediate action]
2. [Second action]
3. [Third action]
Timeline: [X days to complete all verification before DD deadline]

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
2. **PROPERTY PROFILE** — Address, sites, market, contract price, input confidence, data sources
3. **RISK ALERT BOX** — Any HIGH or CRITICAL risks appear prominently before the numbers
4. **REVENUE VERIFICATION** — All 7 steps with findings, discrepancies flagged, verified revenue stated
5. **EXPENSE NORMALIZATION** — Full normalized expense schedule with all additions explained
6. **THREE-SCENARIO NOI** — Floor / Base / Upside in comparison table
7. **VALUATION MATRIX** — 9-cell cap rate × NOI table with contract price position, client target highlighted
8. **DSCR & FINANCING** — All scenarios at contract, recommended, and floor prices vs. client minimums
9. **RISK MATRIX** — All 4 categories, rated, with mitigations
10. **DUE DILIGENCE CHECKLIST** — Prioritized, deal-specific, 20–28 items with status boxes
11. **SELLER QUESTION BANK** — 25 hard questions, deal-specific
12. **GO / NO-GO SUMMARY** — Verdict, NOI, offer range, top risks/opportunities, next steps vs. client criteria

**Always save the analysis to:** `brain/deal-analyses/[property-slug]-underwrite-[YYYY-MM-DD].md`

---

## Industry Benchmarks — Always Reference

| Metric | Conservative | Standard | Strong |
|--------|-------------|----------|--------|
| Cap rate (rural standard) | 10–12% | 8–10% | 6–8% |
| Cap rate (resort/destination) | 8–9% | 6–8% | 5–7% |
| Expense ratio (monthly-heavy) | 48–55% | 42–48% | 38–42% |
| Expense ratio (transient-heavy) | 65–70% | 55–65% | 48–55% |
| EBITDA margin | 13–17% | 18–24% | 25%+ |
| DSCR (minimum bankable) | 1.15x (SBA 7a) | 1.25x | 1.40x+ |
| Management fee (3rd party co.) | 25–30% | 18–25% | 15–18% |
| Management fee (in-house hire) | $50,000/yr | $42,000/yr | $38,000/yr |
| Capital reserves (new build) | $500/site/yr | $600/site/yr | — |
| Capital reserves (mid-age) | $750/site/yr | $850/site/yr | — |
| Capital reserves (older) | $1,000–$1,500/site/yr | — | — |
| RevPAS (KOA franchise) | — | ~$7,000/site/yr | — |
| RevPAS (resort/Sun style) | — | ~$10,000/site/yr | — |
| RevPAS (rural standard) | $2,500/site/yr | $3,500–$5,000/site/yr | $6,000+ |
| Price per site (rural) | $10,000 | $15,000–$25,000 | $30,000+ |
| Revenue multiple | 2.0x | 2.5–3.5x | 4.0x+ |

---

## Error Handling

- **Bank statements not provided:** Flag prominently. Proceed with P&L figures but label all NOI as UNVERIFIED ESTIMATES. Add bank statements as #1 DD checklist item.
- **Only 1 year of financials:** Use it but flag. Add 3-year history request to seller questions. Apply 10% revenue haircut to conservative scenario.
- **Phase I not available:** Add to DD as CRITICAL. Note: environmental contamination can prevent financing and must be completed before contingencies are released.
- **Site count disputed:** Use the lower number for all calculations without exception.
- **Residency ordinance unknown:** Do not assume it's fine. Add web search and city planning call to immediate action list. Flag in risk matrix as HIGH until confirmed.
- **Deal looks excellent across all metrics:** Do not lower the guard. State the positive findings, then still run every risk check.
- **Client preferences not set:** Run setup interview before any analysis.
- **DSCR meets bank minimums but not client's minimum:** Flag specifically as not meeting client's stated criteria even if technically financeable.
