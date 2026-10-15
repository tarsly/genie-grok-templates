# REInvestor Toolbox — Agent Capabilities Manifest

*For use by system-03 (Agent Designer) and system-04 (Agent Activator)*
*Last updated: 2026-06-12*

---

## About This Plugin

`real-estate-investor` is a complete real estate investor acquisition toolkit. It covers every stage of the deal pipeline — from first look at a property through signed offer and contract-to-close management. Skills share a common investor preferences profile so the investor's name, targets, and buy box thresholds are pre-loaded across every interaction. Skills also hand off data to each other through cloud brain: property research feeds deal analysis, deal analysis feeds the investment calculator and rental analysis, deal analysis feeds the offer generator, and the offer generator feeds the due diligence tracker.

**Target users:** Active real estate investors pursuing residential, small multifamily, or creative finance acquisitions (SFR, 2–4 unit, house hacking, BRRRR, sub2, seller finance, wholesale, flip). Investors who are running multiple deals simultaneously or want a systematized acquisition process.

**Plugin version:** 1.1.0 (9 skills)

---

## Available Skills Catalog

Each entry lists: what the skill does, what preferences it collects at activation, its recommended schedule, and which other skills it pairs with naturally.

---

### 1. rei-property-research

**What it does:** Researches any property address — ownership records, tax assessment history, estimated value, comparable sales, sale history, neighborhood analysis, school ratings, flood zone status, and a seller motivation score (1–5 based on equity signals, listing history, tax delinquency, and absentee ownership status). Saves output to cloud brain for automatic use by downstream skills.

**Preferences collected at activation:**
- Investor name and company name
- Target markets (cities/states — 1–3)
- Strategy focus (BRRRR, buy-and-hold, flip, creative finance, wholesale)
- Property types of interest (SFR, small MF, commercial, land)

**Suggested schedule:** On-demand — triggered when evaluating a new property.

**Natural pairings:** rei-deal-analyzer (property research output feeds directly into deal analysis), rei-seller-outreach (motivation score informs outreach angle), rei-rental-analysis (property details pre-load into rental comps).

---

### 2. rei-deal-analyzer

**What it does:** Full go/no-go analysis for any deal — cash flow, cap rate, cash-on-cash return, DSCR, equity position, and a color-coded scorecard comparing every metric to the investor's personal saved targets (not generic benchmarks). Auto-loads prior property research from cloud brain. Supports cash, conventional, DSCR, seller finance, subject-to, and wrap analysis. Saves output for use by investment calculator and offer generator.

**Preferences collected at activation:**
- Minimum acceptable cash flow per door ($)
- Target cash-on-cash return (%)
- Target cap rate (%)
- Minimum DSCR
- Financing preference (cash / conventional / DSCR / creative)
- Risk tolerance (conservative / moderate / aggressive)

**Suggested schedule:** On-demand — triggered when evaluating any deal after initial research.

**Natural pairings:** rei-property-research (research feeds in), rei-investment-calculator (deep modeling after go/no-go confirmed), rei-rental-analysis (rent estimates feed into analysis), rei-offer-generator (analysis feeds into offer package).

---

### 3. rei-investment-calculator

**What it does:** Deep financial modeling for any strategy — BRRRR, flip P&L, buy-and-hold projections (5/10/20/30 year), refinance analysis, seller finance amortization, lease-option analysis, and side-by-side scenario comparison. Includes depreciation estimates, equity build-up curves, opportunity cost comparison (RE vs. S&P 500 vs. HYSA), and balloon payment calculations. Auto-loads prior deal analysis from cloud brain.

**Preferences collected at activation:**
- Default appreciation assumption (% per year — e.g., 3%)
- Default rent growth assumption (% per year — e.g., 3%)
- Federal marginal tax rate (for depreciation savings estimates)
- Primary modeling use case (BRRRR / flip / hold / creative / compare)

**Suggested schedule:** On-demand — triggered when a deal passes the analyzer and the investor wants deep modeling.

**Natural pairings:** rei-deal-analyzer (feeds in from analysis), rei-rental-analysis (rental income figures feed into projections), rei-offer-generator (modeled returns inform offer price).

---

### 4. rei-rental-analysis

**What it does:** Five-strategy rental comparison (long-term, Section 8/FMR, midterm/furnished, short-term/Airbnb, co-living) with weighted scoring aligned to the investor's saved strategy preference. Includes HUD Fair Market Rent data, STR legality check, 5-year income projection comparing the top two strategies, and a primary strategy recommendation. Auto-loads prior property research from cloud brain.

**Preferences collected at activation:**
- Preferred rental strategy (LTR / STR / midterm / co-living / no preference)
- Open to Section 8 tenants? (yes/no)
- Open to short-term rental management intensity? (yes/no)
- Management preference (self-manage / property manager / either)

**Suggested schedule:** On-demand — triggered when evaluating a property's income potential or deciding which rental strategy to pursue.

**Natural pairings:** rei-property-research (property details pre-load), rei-deal-analyzer (rent estimates feed into deal analysis), rei-investment-calculator (rental income feeds into projections).

---

### 5. rei-seller-outreach

**What it does:** Drafts personalized outreach to property sellers across every channel — physical letters, text/SMS, voicemail scripts, emails, and cold call frameworks. Handles pre-foreclosure, probate, absentee owner, tired landlord, tax delinquent, vacant, divorce, and estate situations. Each piece uses empathy-appropriate tone and the investor's saved contact info and brand voice so materials are ready to send immediately.

**Preferences collected at activation:**
- Investor phone number (for outreach materials)
- Investor email
- Investor website
- Company name
- One-line credibility statement (e.g., "Local investor, 5 years in the Dallas market")
- Brand voice preference (professional / friendly / direct)

**Suggested schedule:** On-demand — triggered when the investor has a lead to contact.

**Natural pairings:** rei-property-research (seller motivation score informs outreach angle), rei-offer-generator (after outreach leads to a conversation, an offer is next).

---

### 6. rei-deal-marketing

**What it does:** Complete marketing package for any deal — one-page deal summary, social media posts (Instagram, Facebook, X/Twitter), Instagram Story/Reel script, email blast, investor narrative pitch, and listing descriptions (short and long). Reads verified deal analysis numbers from cloud brain to avoid re-entry. Supports wholesale, flip, buy-and-hold, seller finance, and creative finance deal types.

**Preferences collected at activation:**
- Investor phone number, email, website
- Social media platforms used for marketing
- Brand voice preference (professional / conversational / bold)

**Suggested schedule:** On-demand — triggered when the investor has a deal to move and needs marketing materials.

**Natural pairings:** rei-deal-analyzer (verified numbers pre-load from brain), rei-offer-generator (for deals the investor is acquiring rather than wholesaling).

---

### 7. rei-offer-generator

**What it does:** Generates a full offer package after a deal passes analysis — a Letter of Intent (LOI), purchase price justification document (shareable with the seller), and creative finance term sheet for seller finance or subject-to deals. Reads deal analysis from cloud brain to pre-populate all numbers. Includes a retail-sale comparison table to support the offer price in seller conversations.

**Preferences collected at activation:**
- Investor name and company name (for LOI letterhead)
- Investor phone and email

**Suggested schedule:** On-demand — triggered when the investor is ready to make an offer.

**Natural pairings:** rei-deal-analyzer (analysis feeds in automatically), rei-due-diligence-tracker (after seller accepts, tracker is set up immediately).

---

### 8. rei-due-diligence-tracker

**What it does:** Contract-to-close deadline tracker. Calculates every critical date (EMD, inspection, financing, appraisal, title review, HOA docs, final walk-through, close of escrow) from the contract acceptance date and close date. Assigns urgency (🚨 RED ≤3 days / ⚠️ YELLOW 4–7 days / 🟢 GREEN 8+ days). Saves a live status file to cloud brain. Provides a dashboard view across all active transactions when the investor asks "what's due this week?"

**Preferences collected at activation:**
- Preferred title company contact (name, phone) — optional
- Preferred lender contact (name, phone) — optional
- Preferred inspector contact (name, phone) — optional
- Default inspection period (days — e.g., 10)
- Default financing contingency period (days — e.g., 21)

**Suggested schedule:** Daily (morning) — deadline check across all open transactions every morning.

**Natural pairings:** rei-offer-generator (prompts tracker setup when offer is accepted), rei-deal-analyzer (closed deals can be archived and tracked against original projections).

---

### 9. rei-car-wash-scout

**What it does:** Commercial land acquisition scout for express-tunnel car washes using a creative-finance / zero-down buy box. Runs a demographic gatekeeper (city population ≥ 30K and median household income ≥ $50K), discovers commercial land parcels in the qualifying cities, and scores each against five hard site criteria — price (≤ $1M), zoning permitted by right, road frontage (≥ 65 ft), traffic count (AADT ≥ 12K), and lot size (1.5–2.0 acres). Generates a color-coded interactive map (stdlib-only Leaflet HTML — no pip dependency) and saves the report to cloud brain at `brain/car-wash-scout/{area-slug}-{date}.md`. This is the plugin's commercial-land capability; every other skill targets residential/small-multifamily.

**Preferences collected at activation:**
- Investor name and company name (for reports)
- Target markets (state / city / radius)
- Car wash site criteria overrides (optional — defaults to the standard buy box; saved to `brain/preferences/rei-car-wash-criteria.md`)

**Suggested schedule:** On-demand — triggered when building a Phase 1 target list for a market.

**Natural pairings:** rei-deal-analyzer and rei-investment-calculator (green sites hand off for acquisition modeling), rei-seller-outreach (approach owners of qualifying parcels).

---

## Preferences Registry

All preferences collectible at investor activation, organized by category. system-04 should take the union of all fields needed by the assigned skills and run one consolidated interview.

**Core Identity — needed by all skills:**
- Investor full name
- Company name
- Phone number
- Email address
- Website (optional)

**Investment Strategy — needed by deal analysis and property research skills:**
- Target markets (1–3 cities or states)
- Primary strategy focus (BRRRR / buy-and-hold / flip / creative finance / wholesale / mixed)
- Property types of interest (SFR / 2–4 unit / multifamily / commercial / land)

**Financial Targets — needed by rei-deal-analyzer:**
- Minimum acceptable cash flow per door ($/month)
- Target cash-on-cash return (%)
- Target cap rate (%)
- Minimum DSCR
- Target price range (min/max)
- Financing preference (cash / conventional / DSCR / seller finance / subject-to)
- Risk tolerance (conservative / moderate / aggressive)

**Financial Modeling — needed by rei-investment-calculator:**
- Default appreciation assumption (% per year)
- Default rent growth assumption (% per year)
- Federal marginal tax rate (for depreciation estimates)

**Rental Strategy — needed by rei-rental-analysis:**
- Preferred rental strategy (LTR / STR / midterm / co-living / no preference)
- Open to Section 8 tenants? (yes/no)
- Open to STR management intensity? (yes/no)
- Management preference (self-manage / property manager / either)

**Marketing and Outreach — needed by rei-seller-outreach and rei-deal-marketing:**
- One-line credibility statement
- Brand voice preference (professional / friendly / direct)
- Social media platforms used

**Contract Management — needed by rei-due-diligence-tracker:**
- Default inspection period (days)
- Default financing contingency period (days)
- Preferred title company contact
- Preferred lender contact
- Preferred inspector contact

---

## Suggested Agent Configurations

**These are starting-point suggestions only.** Skills can be assigned to any agent in any combination that makes sense for how the investor thinks about their work.

---

### Option A: Solo Investor (One AI Assistant)

One "Deal Analyst" agent handles everything. All 8 skills assigned to one agent.

**Best for:** Solo investors, first-time AI agent users, or investors who want simplicity and will expand later.

**Activation interview:** All fields from the Preferences Registry. Takes approximately 8–10 minutes. All 8 skills pre-loaded and ready.

---

### Option B: Two-Agent Setup (Recommended Starting Point)

**Acquisition Agent**
Skills: rei-property-research, rei-deal-analyzer, rei-investment-calculator, rei-rental-analysis, rei-offer-generator
Activation fields: Core Identity, Investment Strategy, Financial Targets, Financial Modeling, Rental Strategy

**Deals & Marketing Agent**
Skills: rei-seller-outreach, rei-deal-marketing, rei-due-diligence-tracker
Activation fields: Core Identity, Marketing and Outreach, Contract Management

---

### Option C: Three-Agent Team (For Active Investors Running Multiple Deals)

**Research & Analysis Agent**
Skills: rei-property-research, rei-deal-analyzer, rei-rental-analysis
Activation fields: Core Identity, Investment Strategy, Financial Targets, Rental Strategy

**Modeling & Offer Agent**
Skills: rei-investment-calculator, rei-offer-generator, rei-due-diligence-tracker
Activation fields: Core Identity, Financial Targets, Financial Modeling, Contract Management

**Marketing & Outreach Agent**
Skills: rei-seller-outreach, rei-deal-marketing
Activation fields: Core Identity, Marketing and Outreach

---

## Recommended Schedules

| Skill | Recommended Schedule | Notes |
|---|---|---|
| rei-due-diligence-tracker | Daily, morning | Deadline check across all open transactions. Critical for investors with multiple deals under contract. |
| rei-property-research | On-demand | Triggered when evaluating a new address. |
| rei-deal-analyzer | On-demand | Triggered when a property passes initial interest threshold. |
| rei-rental-analysis | On-demand | Triggered when evaluating income potential. |
| rei-investment-calculator | On-demand | Triggered after deal analysis confirms a deal is worth modeling deeply. |
| rei-seller-outreach | On-demand | Triggered when reaching out to a seller lead. |
| rei-deal-marketing | On-demand | Triggered when marketing a deal to buyers or investors. |
| rei-offer-generator | On-demand | Triggered when ready to make an offer. |
| rei-car-wash-scout | On-demand | Triggered when building a Phase 1 car wash land target list for a market. |

---

## How system-03 Should Use This Document

When a client's brain context indicates they are a real estate investor (from system-02 business blueprint):

1. **Reference the Skills Catalog** to understand what capabilities are available — do not assume; check here.
2. **Surface capabilities based on what the client says they need** — if they say "I need help analyzing deals faster," point them to rei-property-research → rei-deal-analyzer → rei-investment-calculator. If they say "I need to move deals to buyers," point them to rei-deal-marketing.
3. **Highlight the cross-skill handoff pipeline** — this is a key differentiator. The skills are designed to feed each other through cloud brain. Explain: "Run property research once and every downstream skill picks up the data automatically."
4. **Check which plugin is more relevant:** If the investor primarily needs to define their criteria and build their investor identity (buy box, lending box, network), suggest `investor-profile` first. If they're ready to analyze and close deals, suggest `real-estate-investor`.
5. **Note the daily tracker:** rei-due-diligence-tracker is the only skill with a recommended daily cadence — surface this when the investor mentions having deals under contract.

---

## How system-04 Should Use This Document

When activating an agent assigned `real-estate-investor` skills:

1. **Identify which skills are assigned to this agent** from the system-03 design output.
2. **Look up each assigned skill in the Skills Catalog** to get its "Preferences collected at activation" list.
3. **Take the union of all preference fields** across all assigned skills — this is the complete set to collect. Do not run a separate interview per skill.
4. **Cross-reference the Preferences Registry** — Financial Targets are shared across deal-analyzer, investment-calculator, and offer-generator. Collect them once.
5. **Run one consolidated setup interview** — not one per skill. Group naturally: identity → markets and strategy → financial targets → rental and modeling preferences → marketing and outreach → contract management.
6. **Write all preferences to one shared file:**
   ```
   brain/preferences/rei-preferences.md
   ```
   All skills in this plugin read from this single path.
7. **Reference the Recommended Schedules table** — surface the daily tracker schedule for any investor with open transactions. For all other skills, confirm on-demand is the preference or if the client wants any recurring runs.
