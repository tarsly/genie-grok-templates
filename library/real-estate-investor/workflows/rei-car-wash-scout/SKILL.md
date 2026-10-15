---
name: rei-car-wash-scout
description: >-
  Automates commercial land acquisition research for express-tunnel car washes using a creative-finance / zero-down buy box. Screens cities by demographics, discovers qualifying parcels, scores them against strict site criteria (price, zoning, frontage, traffic, lot size), renders a color-coded map, and saves the report. Use when the user asks to \'find car wash land\', \'research car wash sites\', or \'scout car wash properties\'.
---

# REI Car Wash Site Scout

> **Disclaimer:** This skill produces research and analysis for informational purposes only. It is not a substitute for advice from a licensed real estate professional, attorney, land-use planner, or appraiser. Zoning, traffic, and parcel data must be independently verified with the county and state DOT before making any investment decision.

## Overview

Automates land acquisition research for express-tunnel car washes using a creative-finance / zero-down buy box (subject-to and seller-carry structures, with SBA financing for the build). The workflow runs in stages: screen cities by demographics → discover commercial land parcels → score each parcel against strict site criteria → render an interactive color-coded map → deliver a structured report and save it to Cloud Brain so downstream REI skills can reuse it.

You act as a commercial real estate analyst specializing in car wash land acquisition. The goal is to find, evaluate, and map prospective land opportunities that meet strict demographic, physical, and financial requirements.

## When This Skill Applies

- User says "find car wash land," "research car wash sites," or "scout car wash properties"
- User provides a target state, city, or radius and wants qualifying commercial land parcels
- User wants demographic screening of markets for a car wash rollout
- User wants prospective sites scored against car wash site criteria (price, zoning, frontage, traffic, lot size)
- User wants a map of candidate car wash sites color-coded by pass/borderline/fail
- User is building a Phase 1 target list for creative-finance car wash acquisitions

---

## Pre-Flight — Preferences

1. Use `mcp__cloud-brain__search_notes` with query `REI preferences` to load the shared investor profile.
2. Use `mcp__cloud-brain__search_notes` with query `car wash criteria` to load any saved threshold overrides.
3. **If the shared REI profile is found:** Display a brief preferences banner (investor name, target markets). Confirm or offer to update. Proceed.
4. **If not found:** Ask in ONE message:
   > "Before I scout sites, let me save your profile so every report is personalized. Tell me:
   > - Your name and company (for reports)
   > - Target markets (state / city / radius)
   > - Any changes to the default car wash site criteria (otherwise I'll use the standard buy box below)"

   Save investor identity to `brain/preferences/rei-preferences.md` and any criteria overrides to `brain/preferences/rei-car-wash-criteria.md` via `mcp__cloud-brain__write_note`. Proceed.

5. **Default car wash site criteria** (used unless the user overrides — see `references/criteria-guide.md`):

   | Criterion | Threshold |
   |---|---|
   | City population | ≥ 30,000 |
   | Median household income | ≥ $50,000 |
   | Parcel price | ≤ $1,000,000 (borderline $1,000,001–$1,100,000; disqualify > $1,100,000) |
   | Lot size | 1.5–2.0 acres ideal (0.75 ac minimum for infill) |
   | Road frontage | ≥ 65 linear feet |
   | Traffic count (AADT) | ≥ 12,000 (20,000+ premium) |
   | Zoning | Permits a car wash **by right** (rezoning / special-use / conditional-use = hard disqualifier) |

6. Show a ⚙️ **Preferences Active** banner at the top of output.

---

## How It Works

Consult `references/criteria-guide.md` for detailed criteria explanations and `references/data-sources.md` for recommended data sources and APIs before starting.

### Step 1: Demographic Screening (Phase 1)

Given a target state, city, or radius:

1. Use WebSearch / census data to find incorporated cities and towns in the target area.
2. Keep ONLY cities meeting BOTH thresholds: population ≥ 30,000 **and** median household income ≥ $50,000 (or the user's overrides).
3. Exclude any place that fails either criterion.
4. Output a clean list of qualifying cities. For each, include the data vintage (e.g., ACS 5-year 2019–2023) and a source link (e.g., census.gov QuickFacts).

### Step 2: Property Discovery & Record Lookup

For the qualifying cities, search commercial land listings (WebSearch, listing sites like LoopNet / Crexi / Land.com, or a property-records MCP if one is connected):

1. Look for parcels between **0.75 and 2.0 acres** (1.5–2.0 acres is ideal for express tunnels).
2. Filter by price: **Good** ≤ $1,000,000; **Borderline** $1,000,001–$1,100,000 (flag clearly); **Disqualify** > $1,100,000.
3. Gather property address, APN/Parcel ID, asking price, and lot size.

### Step 3: Site Criteria Evaluation

For each candidate parcel, evaluate the strict criteria using property records, zoning data, and traffic maps:

1. **Zoning** — must permit a car wash **by right** (e.g., C-2, C-3, General/Highway Commercial). Requiring a special-use permit, conditional-use approval, or rezoning is a hard disqualifier.
2. **Frontage** — minimum **65 linear feet** of road frontage for ingress/egress and stacking.
3. **Traffic count (AADT)** — at least **12,000** vehicles per day (20,000+ is premium).
4. **Access** — look for curb cuts and median breaks allowing access from both directions.

Assign each parcel a score: 🟢 PASS (meets all criteria), 🟡 BORDERLINE (near a threshold, e.g. price $1M–$1.1M or AADT near 12k), 🔴 FAIL (fails one or more hard disqualifiers).

### Step 4: Map Visualization (optional; run when `--map` is passed or the user asks for a map)

1. Write the evaluated parcels to a JSON file matching the shape in `assets/sample_properties.json` (each object: `address`, `lat`, `lon`, `price`, `lot_size`, `zoning`, `frontage`, `aadt`, `score`, `notes`).
2. Run the bundled generator via Bash:
   ```
   python3 scripts/generate_map.py --input <parcels.json> --output <sites_map.html>
   ```
   The script is stdlib-only (no pip install needed) and loads Leaflet from a CDN. Markers are color-coded: green = PASS, orange = BORDERLINE, red = FAIL.
3. Give the user the path to the generated HTML map.

### Step 5: Final Report Delivery

Present findings in a structured format:

1. A table of qualifying cities with demographic data (see Output Format).
2. A table of evaluated parcels scored against the site criteria.
3. The map file path (if generated).
4. **Next steps** for each 🟢 green site: obtain the local zoning ordinance to confirm the car wash is permitted by right, and order a title/parcel report to verify dimensions, frontage, and encumbrances before making a creative-finance offer.

### Step 6: Save to Cloud Brain

Save the complete report so downstream skills and future runs can reuse it:

```
Path: brain/car-wash-scout/{area-slug}-{YYYY-MM-DD}.md
Tool: mcp__cloud-brain__write_note
Tags: ["car-wash", "land", "site-scout"]
```

Tell the user where it was saved and that they can run `rei-deal-analyzer` or `rei-investment-calculator` on any green site to model the acquisition.

---

## Data Structure

The saved report note (and on-screen report) follows this template — see `assets/report-template.md`:

```markdown
# Car Wash Site Scout Report

**Target Area:** [State/City/Radius]
**Date:** [YYYY-MM-DD]

## Phase 1: Demographic Gatekeeper Results

| City Name | County | Population | Median HH Income | Source / Vintage |
| :--- | :--- | :--- | :--- | :--- |
| [City] | [County] | [Pop] | $[Income] | [Link] (ACS [Years]) |

*Cities failing either criterion have been excluded.*

## Site Criteria Evaluation

| Address | Price | Lot Size | Zoning | Frontage | AADT | Score | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| [Address] | $[Price] | [Size] ac | [Zoning] | [Frontage] ft | [AADT] | 🟢 PASS | [Notes] |

### Scoring Legend
- 🟢 **PASS** — meets all criteria (price ≤ $1M, zoned by right, frontage ≥ 65 ft, AADT ≥ 12k, size 1.5–2.0 ac)
- 🟡 **BORDERLINE** — close to a threshold (price $1M–$1.1M, AADT near 12k)
- 🔴 **FAIL** — fails one or more hard disqualifiers (price > $1.1M, requires rezoning)

## Map Visualization
**Map File:** `[Path to generated HTML map]` *(green = pass, orange = borderline, red = fail)*

## Next Steps
For each green site: obtain the local zoning ordinance to confirm by-right car wash use, and
order a title/parcel report to verify dimensions and frontage before making an offer.
```

---

## Output Format

```markdown
⚙️ Preferences Active | Investor: [Name] | Markets: [Targets]

---

# Car Wash Site Scout — [Target Area]
**Date:** [YYYY-MM-DD]

## Qualifying Cities
[Demographic table]

## Evaluated Sites
[Scored parcel table with 🟢 / 🟡 / 🔴]

## Map
[Path to generated HTML map, if requested]

## Next Steps
[Per-green-site zoning + title actions]

## Sources
- [URL 1]
- [URL 2]

---
*REInvestor Toolbox — Car Wash Site Scout | Saved to brain/car-wash-scout/{slug}*
```

---

## Example Usage

> **User:** "Scout car wash sites in Central Florida within 50 miles of Orlando."

The skill loads REI preferences, screens Central Florida cities for population ≥ 30k and MHI ≥ $50k, searches commercial land listings in the qualifying cities, scores each parcel against the site criteria, generates a color-coded Leaflet map, presents the qualifying-cities and scored-parcel tables, and saves the report to `brain/car-wash-scout/central-florida-2026-07-13.md`.

---

## Error Handling

- **No target area given:** Ask for a state, city, or radius before proceeding.
- **No qualifying cities:** Report that no cities in the target area cleared the demographic thresholds; offer to widen the radius or relax the population/income floors.
- **WebSearch unavailable:** Inform the user; offer to organize manually provided city/parcel data into the report and map instead.
- **`python3` not available:** Skip the map step and deliver the scored parcel table only; tell the user the map can be generated later once Python is available (no pip packages are required).
- **Fewer parcels than expected:** Note the thin inventory; suggest expanding the size or price band, or adding more qualifying cities.
- **Report already exists for this area/date:** Ask whether to refresh (overwrite) or keep the existing note.
