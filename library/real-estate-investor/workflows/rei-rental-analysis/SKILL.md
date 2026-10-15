---
name: rei-rental-analysis
description: >-
  Rental market analysis — rent comps, Airbnb vs long-term rental, STR and midterm analysis, Section 8 rents, fair market rent, vacancy rates, rental strategy comparison, rental income estimates, 5-year income projections by strategy, or wants to compare STR vs long-term rental income, check Section 8 rents, or project 5-year rental returns.
---

# REI Rental Analysis

> **Disclaimer:** This skill produces rental market analysis for informational purposes only. Rental income projections are estimates and are not guaranteed. Market conditions vary. Consult local property managers and real estate professionals to verify data before making investment decisions. Always confirm STR regulations with local authorities before pursuing a short-term rental strategy.

## Overview

Performs a deep rental market analysis for any property or area. Compares five rental strategies head-to-head — long-term, Section 8, midterm (furnished), short-term (Airbnb/VRBO), and co-living — then recommends the optimal strategy based on the property type, location, and **the investor's saved preferences.** Adds a 5-year income projection for the top two strategies so the compounding difference is visible. Reads prior property research from cloud brain when available.

## When This Skill Applies

- User asks "what can I rent this property for?"
- User wants to compare Airbnb vs. long-term rental income
- User asks about rental rates in a specific city, zip, or neighborhood
- User wants Section 8 Fair Market Rent (FMR) for an area
- User asks about vacancy rates in a market
- User says "rental analysis" or "rent comps"
- User is deciding between rental strategies for a property
- User wants to know if a property meets the 1% rule based on actual rents
- User asks about midterm rental or travel nurse housing potential
- User wants co-living or rent-by-the-room analysis

---

## Pre-Flight — Investor Preferences

1. Use `mcp__cloud-brain__search_notes` with query `REI preferences`
2. **If found:** Load preferences. Note any saved strategy preference (e.g., "you prefer LTR — I'll highlight that in the recommendation, but I'll still show all five strategies for comparison"). Display ⚙️ banner.
3. **If not found:** Run preferences setup. Save to `brain/preferences/rei-preferences.md`.
4. **Check for prior property research:** Use `mcp__cloud-brain__search_notes` with the address slug. If found, pre-load property details (beds/baths, sqft, year built, purchase price) to avoid re-entry.

---

## How It Works

### Step 0: Parse User Input

| Input | Required | Example |
|-------|----------|---------|
| Property address OR city/zip | Yes | "4521 Elm St, Dallas TX" or "Dallas TX 75216" |
| Property type | Helpful | SFR, duplex, triplex, quad, condo |
| Bedrooms / bathrooms | Helpful | 3 bed / 2 bath |
| Purchase price | Optional | $165,000 |
| Current condition | Optional | Turnkey, needs rehab, furnished |
| Strategy bias | Optional | "I'm thinking Airbnb" |

### Step 1: Research Long-Term Rental Market

**Search queries:**
- `"{city} {state} {beds} bedroom house for rent {year}"`
- `"{zip code} average rent {year}"`
- `"Zillow rent estimate {address}"`
- `"Rentometer {city} {state} {beds} bedroom"`

**Capture:** Average rent, range (low/median/high), vacancy rate, rent trend, rent-to-price ratio.

**Rent comp table:**

| Comp | Address/Area | Beds/Ba | Rent | Condition | Distance |
|------|-------------|---------|------|-----------|----------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

### Step 2: Research Section 8 / Fair Market Rent

**Search:** `"HUD fair market rent {county} {state} {year}"`, `"FMR {zip code} {year}"`

| Bedrooms | FMR | Payment Standard (100%) | Subject Property |
|----------|-----|------------------------|-----------------|
| 1 BR | | | |
| 2 BR | | | |
| 3 BR | | | |
| 4 BR | | | |

Also capture: waitlist status (open/closed), voucher utilization rate.

### Step 3: Research Short-Term Rental (STR) Potential

**Search:** `"Airbnb average daily rate {city} {state} {year}"`, `"short term rental regulations {city} {state}"`, `"AirDNA {city} {state} market data"`

**Capture:**
- Average daily rate (ADR) for matching property type
- Average occupancy rate
- Estimated monthly revenue (ADR × occupancy × 30)
- STR legal status (permit required? banned? restricted zones?)
- Seasonality patterns
- Platform fees (Airbnb ~3%, VRBO ~3–5%)

**STR Legality Check:**
- Is STR legal in this jurisdiction?
- Permit / license required? Cost?
- Minimum stay requirements?
- HOA or zoning restrictions?
- Occupancy / hotel tax implications?

### Step 4: Research Midterm Rental Potential

**Search:** `"furnished rental {city} {state} monthly rate"`, `"travel nurse housing {city} {state}"`, `"Furnished Finder {city} {state}"`

**Demand drivers to check near property:**
- Hospitals (travel nurses — 13-week contracts)
- Military bases (PCS / TDY)
- Universities (visiting professors, grad students)
- Corporate offices (relocations, project assignments)
- Construction projects (traveling workers)
- Insurance claims (displaced families)

**Capture:** Monthly furnished rate, typical lease length (1–6 months), furniture cost estimate ($3,000–$8,000), premium over unfurnished LTR.

### Step 5: Analyze Co-Living / Rent-by-the-Room

**Search:** `"room for rent {city} {state} {zip code}"`, `"co-living {city} {state}"`

**Calculate:**
- Rentable bedrooms × per-room rate (typically 60–80% of a 1BR apartment)
- Total gross rent if all rooms rented
- Additional costs: shared utilities, higher turnover, more management intensity

### Step 6: Build Strategy Comparison

| Metric | Long-Term | Section 8 | Midterm | STR (Airbnb) | Co-Living |
|--------|-----------|-----------|---------|--------------|-----------|
| Monthly Gross Revenue | | | | | |
| Vacancy Rate | | | | | |
| Operating Expenses | | | | | |
| **Net Monthly Income** | | | | | |
| Annual Net Income | | | | | |
| Startup Cost | | | | | |
| Management Intensity | Low | Low | Medium | High | High |
| Regulatory Risk | Low | Low | Low | Medium–High | Low |
| Scalability | High | High | Medium | Low | Medium |

**Expense adjustments by strategy:**
- **Long-term:** Baseline. Lowest management intensity.
- **Section 8:** Add annual inspection prep. Rent guaranteed by HUD.
- **Midterm:** Add furniture, utilities ($150–300/mo), higher cleaning. 30–75% rent premium.
- **STR:** Add furniture, utilities, turnover cleaning ($75–150/stay), supplies ($50–100/mo), platform fees, higher insurance, occupancy taxes.
- **Co-Living:** Add shared utilities, higher maintenance and turnover per room.

### Step 7: Weighted Scoring & Recommendation

Score each strategy 1–5 across dimensions weighted to investor's profile:

| Dimension | Weight | LTR | S8 | MTR | STR | Co-Living |
|-----------|--------|-----|----|-----|-----|-----------|
| Net Income | 30% | | | | | |
| Management Ease | 20% | | | | | |
| Reliability | 20% | | | | | |
| Startup Cost | 15% | | | | | |
| Regulatory Safety | 15% | | | | | |
| **Weighted Score** | 100% | | | | | |

**If investor has a saved strategy preference:** Note it explicitly — "Your saved preference is LTR. LTR scores X. STR scores higher at Y — flag this for your consideration, but LTR remains a strong choice if management simplicity is the priority."

**Recommendation format:**
- PRIMARY strategy (highest weighted score, accounting for investor preference)
- BACKUP strategy
- Why primary wins
- What would change the recommendation
- Specific implementation steps

### Step 8: 5-Year Income Projection (Top 2 Strategies)

Show the compounding difference over 5 years between the top two strategies:

| Year | Strategy A Net Income | Strategy B Net Income | Cumulative Gap |
|------|----------------------|----------------------|----------------|
| Year 1 | | | |
| Year 2 | | | |
| Year 3 | | | |
| Year 4 | | | |
| Year 5 | | | |
| **5-Year Total** | | | |

Include: rent growth assumption (3%), startup cost recoup timeline for higher-cost strategies (furniture, permit).

### Step 9: Save to Cloud Brain

```
Path: brain/rental-analyses/{address-slug}-{YYYY-MM-DD}.md
Tool: mcp__cloud-brain__write_note
```

Tell the user: "Rental analysis saved. Run `rei-deal-analyzer` to combine these rent estimates with your financing for a full go/no-go."

---

## Output Format

```markdown
⚙️ Preferences Active | Investor: [Name] | Strategy Preference: [Saved] | Markets: [Targets]

---

# Rental Market Analysis: {Address or Area}
**Date:** {date}

---

## Property Overview
[Details table]

## Long-Term Rental Market
### Rent Comps
[Comp table]
### Market Summary
- Median rent: $X,XXX | Range: $X,XXX–$X,XXX | Vacancy: X% | Trend: [↑/→/↓]
- Rent-to-price ratio: X.XX%

## Section 8 / Fair Market Rent
[FMR table] | Waitlist: [Open/Closed]

## Short-Term Rental (Airbnb/VRBO)
- ADR: $XXX | Occupancy: XX% | Est. Monthly Gross: $X,XXX
- Legal status: [Legal / Restricted / Banned] | Permit required: [Yes ($XX) / No]

## Midterm Rental (Furnished)
- Est. Monthly Rate: $X,XXX | Demand Drivers: [list]
- Furniture Startup: $X,XXX | Premium over LTR: XX%

## Co-Living / Rent by the Room
- X rooms × $XXX/room = $X,XXX gross | Premium over single-tenant: XX%

## Strategy Comparison
[Full comparison table]

## Recommendation
**Primary: [Strategy] | Est. Monthly Net: $X,XXX**
[Detailed reasoning, including alignment with saved investor preferences]

### Implementation Steps
1. [Step 1]
2. [Step 2]

## 5-Year Income Projection (Top 2 Strategies)
[Projection table showing cumulative income gap]

---
*REInvestor Toolbox — Rental Analysis | Saved to brain/rental-analyses/{slug}*
```

---

## Error Handling

- **No address or area:** Ask for at minimum a city and state.
- **No bedrooms / property type:** Ask — significantly affects rental estimates.
- **WebSearch unavailable:** Inform user; cannot pull current market data or FMR rates.
- **No rental comps for area:** Expand to city-level data and note reduced accuracy.
- **HUD FMR not found:** Direct to huduser.gov; proceed with other strategies.
- **STR regulations not found:** Flag as critical unknown — do not recommend STR until verified with local authorities.
- **Co-living not applicable (e.g., 1BR):** Skip that strategy and note why.
- **Midterm demand drivers not found near property:** Note; suggest insurance displacement or corporate housing as lower-demand alternatives.
