---
name: rei-property-research
description: >-
  Research any property address — ownership, tax assessment, sale history, comparable sales, estimated value, neighborhood analysis, school ratings, flood zones, crime stats, seller motivation signals, property profile, or needs ownership history, comparable sales, neighborhood data, or seller motivation signals on a specific address.
---

# REI Property Research

> **Disclaimer:** This skill produces research and analysis for informational purposes only. It is not a substitute for advice from a licensed real estate professional, attorney, or appraiser. All data should be independently verified before making investment decisions.

## Overview

Performs a comprehensive deep dive on any property address. Uses web search to compile everything available into a single research dossier: ownership, last sale, tax records, estimated value, comps, neighborhood demographics, schools, crime, flood zone, and a seller motivation score. Saves to cloud brain so deal-analyzer and investment-calculator can read this data without re-entering it.

## When This Skill Applies

- User provides a property address and wants to know everything about it
- User says "research this address" or "pull up property data"
- User wants pre-offer due diligence research
- User asks about property value, tax records, or sale history
- User needs comparable sales for an address
- User asks about neighborhood quality, schools, or crime
- User needs a property profile to present to a buyer or investor
- User wants flood zone, HOA, or zoning information
- User is preparing for a seller meeting and needs property intel

---

## Pre-Flight — Investor Preferences

1. Use `mcp__cloud-brain__search_notes` with query `REI preferences`
2. **If found:** Display a brief preferences banner (investor name, target markets, strategy focus). Confirm or offer to update. Proceed.
3. **If not found:** Ask in ONE message:
   > "Before I run this research, let me save your investor profile so I can personalize every output. Tell me:
   > - Your name and contact info (for reports and outreach)
   > - Target markets (city/state)
   > - Investment strategy focus (LTR, STR, BRRRR, flip, creative finance, or mixed)
   > - Property types you focus on (SFR, multifamily, commercial)
   > - Target price range (e.g., $100K–$400K)
   > - Min monthly cash flow per door, target cash-on-cash return, target cap rate
   > - Financing preference (conventional, creative, cash)"

   Save to `brain/preferences/rei-preferences.md` via `mcp__cloud-brain__write_note`. Proceed.

4. Show a ⚙️ **Preferences Active** banner at top of output.

---

## How It Works

### Step 0: Parse the Address

Extract and standardize: street address, city, state, zip, county. If partial, use WebSearch to resolve the full address. If multiple matches, present options.

### Step 1: Property Details & Records

**Search queries:**
- `"{full address}" property records`
- `"{full address}" Zillow`
- `"{full address}" Redfin`
- `"{full address}" county assessor`
- `"{full address}" tax records`

**Capture:**

| Detail | Source |
|--------|--------|
| Property type | Assessor / Zillow |
| Bedrooms / Bathrooms | Listings / assessor |
| Square footage | Assessor |
| Lot size | Assessor |
| Year built | Assessor |
| Garage, pool, stories | Assessor / listing |
| Construction / roof type | Assessor |
| Zoning | County / city |
| Parcel / APN | Assessor |

### Step 2: Valuation & Sale History

**Search queries:**
- `"{full address}" sold price history`
- `"{full address}" Zestimate estimated value`
- `"{county} {state} recorder deeds {street address}"`

**Capture:**

| Date | Event | Price | Source |
|------|-------|-------|--------|
| | Last sale | | |
| | Previous sale | | |
| | Tax assessed value | | |
| | Zillow Zestimate | | |
| | Redfin Estimate | | |

Calculate: annual appreciation rate from last sale, price/sqft vs. area average.

### Step 3: Tax Information

**Search:** `"{full address}" property tax amount`, `"{county} {state} property tax rate"`

Capture: annual tax, assessed value (land/improvement/total), effective rate, exemptions, delinquencies, special assessments.

### Step 4: Current / Recent Listings

**Search:** `"{full address}" for sale`, `"{full address}" listing history`

Capture: Is it listed? List price / DOM / price reductions. Previous listing attempts (expired/withdrawn = motivation signal). Listing agent and brokerage.

### Step 5: Neighborhood Analysis

**Search:** `"{zip code} neighborhood demographics"`, `"Niche.com {zip code}"`, `"AreaVibes {city} {state} {zip}"`

| Metric | Value |
|--------|-------|
| Median household income | |
| Median home value | |
| Population growth trend | |
| Owner-occupied vs. renter % | |
| Walkability / livability score | |
| Cost of living index | |

### Step 6: School Ratings

**Search:** `"GreatSchools {city} {state} {zip code}"`, `"schools near {full address}"`

| School | Type | Rating | Distance |
|--------|------|--------|----------|
| | Elementary | /10 | |
| | Middle | /10 | |
| | High | /10 | |

### Step 7: Crime & Safety

**Search:** `"CrimeGrade {zip code}"`, `"crime rate {zip code} {year}"`

| Metric | Value | vs. National Avg |
|--------|-------|-----------------|
| Overall crime grade | | |
| Violent crime rate | | Above/Below |
| Property crime rate | | Above/Below |
| Crime trend | | |

### Step 8: Flood Zone & Environmental

**Search:** `"FEMA flood zone {full address}"`, `"{county} {state} flood zone map"`

| Factor | Status |
|--------|--------|
| FEMA flood zone | Zone X / A / AE / V |
| Flood insurance required | Yes/No |
| Estimated flood insurance cost | |
| Wildfire risk | Low/Med/High |
| Earthquake risk | Low/Med/High |

### Step 9: Comparable Sales

**Search:** `"recently sold homes near {full address}"`, `"Redfin sold {city} {state} {zip} {beds} bed"`

| # | Address | Beds/Ba | SqFt | Sale Price | $/SqFt | Sale Date | Distance |
|---|---------|---------|------|-----------|--------|-----------|----------|
| 1 | | | | | | | |
| 2 | | | | | | | |
| 3 | | | | | | | |
| 4 | | | | | | | |
| 5 | | | | | | | |

Comp analysis: average sale price, average $/sqft, subject property estimated value, above/below market.

### Step 10: Seller Motivation Score

Score the seller's likely motivation 1–5 based on signals found in research:

| Signal | Weight | Finding |
|--------|--------|---------|
| Days since last sale (longer = more equity, possible fatigue) | | |
| Price reductions on current/prior listing | | |
| Multiple expired/withdrawn listings | | |
| Delinquent taxes | | |
| Code violations (if found) | | |
| Absentee owner (assessed address ≠ property address) | | |
| Estate / probate indicators | | |
| Long ownership with no refinance activity | | |

**Score:**
- 1–2: No obvious motivation — exploratory approach only
- 3: Moderate motivation — worth reaching out
- 4: Strong motivation — prioritize this property
- 5: High urgency — move quickly

### Step 11: Save to Cloud Brain

Save the complete dossier:

```
Path: brain/property-research/{address-slug}-{YYYY-MM-DD}.md
Tool: mcp__cloud-brain__write_note
```

Tell the user: "Research saved. Run `rei-deal-analyzer` next to build a full go/no-go on this property using this data."

---

## Output Format

```markdown
⚙️ Preferences Active | Investor: [Name] | Strategy: [Focus] | Markets: [Targets]

---

# Property Research Dossier: {Address}
**Date:** {date}

---

## Property Summary
[Details table]

## Valuation
[Multi-source value table + comp-based estimate]

## Sale History
[Sale history table]

## Tax Information
[Tax detail table]

## Current Listing Status
[Active listing details or "Not currently listed"]

## Comparable Sales
[Comp table + average $/sqft + comp-based value conclusion]

## Neighborhood Profile
[Demographics, livability scores]

## Schools
[Rating table]

## Crime & Safety
[Crime stats table]

## Flood Zone & Environmental
[Environmental risk table]

## Seller Motivation Score
**Score: X / 5 — [No Motivation / Moderate / Strong / High Urgency]**
[Signals found that drove the score]

## Investment Snapshot
| Metric | Value |
|--------|-------|
| Price/SqFt vs. Area Average | |
| Estimated Rent Range | |
| Gross Rent-to-Price Ratio | |
| Annual Property Tax | |
| School District Quality | |
| Crime Level | |
| Flood Risk | |

## Key Findings & Flags
- ✅ [Positive finding]
- ⚠️ [Risk or concern]

## Sources
- [URL 1]
- [URL 2]

---
*REInvestor Toolbox — Property Research | Saved to brain/property-research/{slug}*
```

---

## Error Handling

- **No full address:** Ask for complete address (street, city, state, zip).
- **Partial address:** Use WebSearch to resolve; present options if multiple matches.
- **WebSearch unavailable:** Inform user; offer to organize manually provided data into dossier format.
- **Fewer than 3 comps:** Expand radius/timeframe; note the reduced accuracy.
- **Flood zone not found:** Flag as open item; direct to fema.gov/flood-maps.
- **Dossier already exists for this address:** Ask whether to refresh or keep existing.
