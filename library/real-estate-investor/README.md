# REInvestor Toolbox

**Plugin version:** 1.1.0
**Publisher:** the provider (the provider)
**Category:** Real Estate Investing

---

## What This Plugin Does

REInvestor Toolbox is a complete deal acquisition toolkit for active real estate investors. It handles the full workflow from researching a property to drafting the offer — and everything in between.

Every skill reads your saved investor preferences from the first run, so you never re-enter your name, targets, or thresholds. Each skill also saves its output to cloud brain, meaning skills can hand off to each other automatically. Run property research → it pre-loads into deal analysis. Run deal analysis → it pre-loads into the investment calculator and offer generator.

---

## Skills Included

### rei-property-research
Research any property address — ownership, tax records, estimated value, sale history, comparable sales, neighborhood analysis, school ratings, flood zones, and seller motivation scoring (1–5 scale based on equity, listing history, delinquency, and absentee status).

**Trigger phrases:** "research this address," "pull property info," "who owns this," "what's this house worth," "run property research"

### rei-deal-analyzer
Full go/no-go analysis scored against your saved buy box. Pulls cap rate, cash-on-cash return, DSCR, cash flow, equity position, and a color-coded scorecard comparing every metric to your personal targets (not generic benchmarks). Auto-loads prior property research from brain.

**Trigger phrases:** "analyze this deal," "run the numbers," "does this deal work," "cash flow analysis," "cap rate on this property"

### rei-investment-calculator
Deep financial modeling: BRRRR, flip P&L, buy-and-hold projections (5/10/20/30 year), refinance analysis, seller finance amortization, lease-option analysis, and side-by-side scenario comparison. Includes depreciation estimates, equity build-up curves, and an opportunity cost comparison (same cash in S&P 500 and HYSA vs. real estate).

**Trigger phrases:** "run BRRRR analysis," "flip numbers," "amortization table," "10-year projection," "refinance analysis," "model this deal"

### rei-rental-analysis
Five-strategy rental comparison (long-term, Section 8, midterm/furnished, Airbnb/STR, co-living) with weighted scoring, 5-year income projection comparing top two strategies, STR legality check, HUD Fair Market Rent data, and strategy recommendation aligned with your saved preferences.

**Trigger phrases:** "what can I rent this for," "Airbnb vs. long-term," "rental analysis," "Section 8 rates," "rent comps," "midterm rental potential"

### rei-seller-outreach
Personalized outreach drafts for every channel — physical letters, text/SMS, voicemail scripts, emails, and cold call frameworks. Handles pre-foreclosure, probate, absentee owner, tired landlord, tax delinquent, vacant, divorce, and estate situations. Reads your saved contact info and brand voice so every piece is ready to send.

**Trigger phrases:** "write a letter to this seller," "draft outreach," "cold call script," "text this owner," "yellow letter," "seller communication"

### rei-deal-marketing
Complete marketing package for any deal — one-page deal summary, social media posts (Instagram, Facebook, X/Twitter), Instagram Story/Reel script, email blast, investor narrative pitch, and listing descriptions (short and long). Reads verified deal numbers from brain when available.

**Trigger phrases:** "market this deal," "one-pager," "deal summary," "social media posts," "email blast," "investor pitch," "listing description"

### rei-offer-generator *(New)*
Generates a full offer package: Letter of Intent (LOI), purchase price justification, and creative finance term sheet for seller finance or subject-to deals. Pre-loads deal analysis numbers from brain. Includes retail-sale comparison table to support the offer price with a seller.

**Trigger phrases:** "draft an LOI," "make an offer," "offer letter," "seller finance term sheet," "subject-to offer," "present this offer"

### rei-due-diligence-tracker *(New)*
Contract-to-close deadline tracker. Enter a close date and contingency periods — the skill calculates every critical date (EMD, inspection, financing, appraisal, title review, walk-through, close), assigns urgency (🚨 RED / ⚠️ YELLOW / 🟢 GREEN), and saves a live status file to brain. Dashboard view across all active transactions when you ask "what's due this week?"

**Trigger phrases:** "we're under contract," "set up contract tracking," "what deadlines are due," "mark inspection complete," "show my open transactions," "what's due this week"

### rei-car-wash-scout *(New)*
Commercial land acquisition scout for express-tunnel car washes using a creative-finance / zero-down buy box. Screens cities by demographics (population ≥ 30K, median HH income ≥ $50K), discovers qualifying commercial land parcels, scores each against strict site criteria (price, zoning by-right, road frontage, traffic count, lot size), generates a color-coded interactive map (zero-dependency Leaflet — no pip install), and saves the report to cloud brain. Green sites hand off to rei-deal-analyzer and rei-investment-calculator.

**Trigger phrases:** "find car wash land," "research car wash sites," "scout car wash properties," "car wash site scout"

---

## Two-Phase Design

Several skills form a natural pipeline:

```
rei-property-research  →  rei-deal-analyzer  →  rei-investment-calculator
                                ↓                        ↓
                     rei-rental-analysis        rei-offer-generator
                                                         ↓
                                             rei-due-diligence-tracker
                                                         ↓
                                              rei-deal-marketing
```

Each skill saves its output to cloud brain at a standardized path. The next skill in the pipeline reads that output automatically — no copy/paste required.

---

## Investor Preferences

On your first run of any skill, you'll be asked to set up your investor profile:

- **Name and contact info** (used in outreach and marketing materials)
- **Target markets** (1–3 cities or states)
- **Strategy focus** (LTR, BRRRR, flip, creative finance, etc.)
- **Property types** (SFR, multifamily, commercial, etc.)
- **Target price range**
- **Minimum cash flow per door**
- **Target cash-on-cash return**
- **Target cap rate**
- **Minimum DSCR**
- **Financing preference**
- **Brand voice** (for outreach and marketing)

Preferences are saved to `brain/preferences/rei-preferences.md` and auto-applied by every skill from that point on. You'll see a ⚙️ banner at the top of each output confirming your active preferences. To update preferences, say "update my investor preferences."

---

## Updating Your Preferences

Say "update my REI preferences" at any time. The skill will load your current preferences, display them, and ask which you want to change.

---

## Disclaimer

All analysis produced by this plugin is for informational purposes only. Financial projections are estimates and are not guaranteed. Real estate investing involves risk of loss. Always consult licensed real estate professionals, financial advisors, and attorneys before making investment decisions. Tax estimates are illustrative only — consult a qualified CPA before making tax-related decisions. STR projections should be verified against local short-term rental regulations.

---

*REInvestor Toolbox | the provider | v1.1.0*


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
