---
name: rei-buy-box
description: >-
  Real estate and business investor Buy Box definition and infographic generation skill. Use when a user wants to: (1) Define their investment criteria (buy box) through a guided interview, (2) Determine which investment strategy fits their goals, risk tolerance, and superpowers (STR, LTR, multifamily, mobile home parks, RV parks, co-living, house flipping, business acquisitions, notes, or consulting for equity), (3) Save their buy box to the brain so other skills can evaluate deals against it, (4) Generate a professional shareable buy box infographic (HTML) for social media. Informed by well-known creative-finance educators, Jamil Damji (AstroFlipping), and Roland Frasier (EPIC/Business Acquisitions) frameworks.
---

# Real Estate & Business Buy Box Skill

> **Disclaimer:** Investment strategy recommendations and market data are for informational purposes only. All investment decisions involve risk. Consult licensed financial and legal advisors before investing.

## Overview

A **buy box** is an investor's personal filter — specific criteria an asset must meet to be worth pursuing. Having a defined buy box prevents analysis paralysis, communicates needs to wholesalers and brokers, and keeps investors focused.

**Core philosophy (leading deal-making educators):** Focus beats diversification. Guide users toward 1–2 strategies and 1–2 markets/industries maximum. Leverage "superpowers" (existing skills) to find unique deals like consulting for equity.

## Workflow

Three phases, completed in order:
- **Phase 1 — Interview & Market Research:** Guide the investor through a conversational interview to define their buy box.
- **Phase 2 — Brain Save:** Save finalized buy box to cloud brain so all other skills can evaluate deals against it.
- **Phase 3 — Infographic:** Generate a professional, shareable buy box card (HTML file) using the Write tool.

---

## Pre-Flight — Check for Existing Buy Box

1. Use `mcp__cloud-brain__search_notes` with query `buy box criteria`
2. **If found:** Display ⚙️ banner. Confirm: "Found your saved buy box from [date]. Want to (1) update it, (2) start fresh, or (3) evaluate a deal against it?"
3. **If not found:** Proceed to Phase 1 interview.
4. **If user provides a deal to evaluate:** Jump directly to the Evaluator (end of this skill).

---

## Phase 1: The Buy Box Interview

Conduct conversationally — ask 1–2 questions at a time, wait for answers, then adapt. Never dump all questions at once.

### Stage 1: Goals, Risk & Superpowers

1. **The "Why":** "What is your primary goal for investing? (e.g., monthly cash flow to replace W2, long-term wealth, quick capital generation, tax benefits, or leveraging a specific skill)"
2. **Asset Preference:** "Are you primarily interested in traditional real estate (houses, apartments), commercial assets (mobile home parks, storage), or operating businesses (car washes, home services, consulting for equity)?"
3. **The "Superpower":** "What is your unique superpower? (e.g., AI implementation, digital marketing, managing contractors, operational systems)"
4. **Involvement Level:** "How hands-on do you want to be — passive investment, active management, or somewhere between?"
5. **Capital Access:** "Do you have cash to deploy, or are you focused on creative finance (Subject-To, Seller Finance) because capital is limited?"

### Stage 2: Strategy Matching

Based on Stage 1, recommend 1–2 strategies:

| If the investor says... | Recommend |
|---|---|
| High tech/AI skills, low capital | Consulting for Equity (Sweat Equity) |
| Wants recurring revenue, low labor | Car Washes or Self-Storage |
| High income, wants tax shelter | Short-Term Rentals (STR) |
| Wants passive, steady cash flow | Long-Term Rentals (SFR or Multifamily) |
| Wants to scale fast, affordable housing | Mobile Home Parks or RV Parks |
| Wants max cash flow on one property | Co-Living |
| Wants quick capital, active | House Flipping or Wholesaling |
| Truly passive (be the bank) | Mortgage Notes |
| Limited capital/credit | Sub2 / Seller Finance on ANY strategy |

**Strategy profiles:**

| Strategy | Ideal Investor | Typical Criteria | Typical Returns |
|----------|---------------|-----------------|----------------|
| **Long-Term Rental (SFR/MF)** | Passive, wealth builder | 1% rule or better, positive CF, landlord-friendly state | 6–12% CoC |
| **Short-Term Rental (STR)** | Active, hospitality-minded | Tourist/business market, STR-legal zone, furnished | 15–25% CoC |
| **House Flipping** | Active, contractors, capital available | 70% rule (MAO = ARV × 70% − rehab), 3–6 month project | 15–30% ROI per deal |
| **BRRRR** | Patient, systems-oriented | Below-market purchase, rehab potential, strong rental market | Goal: infinite return |
| **Mobile Home Parks** | Patient, management-focused | 25+ pads, tenant-owned homes, under $30K/pad | 8–12% cap rate |
| **RV Parks** | Hospitality, land-focused | Highway access, hookups, growing Sun Belt / Mountain West | 10–15% cap rate |
| **Self-Storage** | Passive, minimal management | 300+ units, climate control, suburban or rural | 8–12% cap rate |
| **Wholesaling** | Active, networker, deal finder | 25–40% discount to ARV, motivated seller, quick close | $5K–$30K/assignment |
| **Mortgage Notes** | Truly passive, analytical | 1st lien position, 65–75% LTV, performing or non-performing | 10–15% yield |
| **Consulting for Equity** | High-skill operator (AI, ops, marketing) | Business with $1M–$5M revenue lacking systems | 10–25% equity stake |
| **Car Wash / Laundromat** | Systems-oriented, passive | Automated, high-traffic location, EBITDA multiple ≤4x | 20–30% CoC |

**Anti-Sprawl Guardrail:** If the user wants to do everything, intervene: *"That's a great long-term vision. However, the most successful investors recommend mastering just 1–2 strategies first. A buy box that includes everything makes it impossible for wholesalers to know what to send you. Which ONE is your absolute top priority right now?"*

### Stage 3: Live Market Research (Optional)

Once 1–2 strategies selected, offer: *"Before we lock in your criteria, would you like me to run a live market analysis on which cities/states are currently performing best for [Strategy]?"*

If yes: Use `WebSearch` to research:
- `"best markets for [strategy] real estate 2025 2026"`
- `"[strategy] market data [state] [year]"`
- `"landlord friendly states [year]"` (for LTR)
- `"STR regulations [city]"` (for STR)

Present top 3–5 markets with supporting data. Ask investor to pick 1–2 to use in their buy box.

### Stage 4: Core Criteria Definition

**For Real Estate / Commercial:**
1. **Geography:** Where? *(If "Nationwide" or 10+ states: "Operating nationwide is very difficult without boots on the ground. Let's narrow to 1–3 specific markets where you can actually execute.")*
2. **Property Type & Size:** What type? Minimum beds/baths or unit count?
3. **Financial Targets:** Purchase price range? Minimum cash flow per door? Target cap rate?
4. **Condition Tolerance:** Turnkey / light cosmetic / medium rehab / heavy distressed?
5. **Acquisition Methods:** Sub2, Seller Finance, Cash, Conventional, DSCR?
6. **Partnership Openness:** Open to JVs? What do you bring / what do you need?

**For Business Acquisitions / Consulting for Equity:**
1. **Industry/Niche:** What industries? (Home services, SaaS, local retail)
2. **Business Size:** Target revenue or EBITDA range?
3. **The "Wedge":** What specific problem do you solve for the owner?
4. **Deal Structure:** Buy 100% via seller finance, or minority equity stake for services?

**Universal:**
- **Deal Breakers:** Anything that immediately disqualifies a deal? (flood zones, foundation issues, rent control, etc.)

### Stage 5: Synthesis — The Two Lists

Synthesize into two focused lists. **Less is more — 3–4 sharp, specific items are better than 5 generic ones. Never pad with filler.**

**List 1: What I'm Looking For** — Specific criteria for an ideal deal.
Format: `Bold Topic: specific detail`
Example: `Multifamily 20+ units: 8%+ cap rate, landlord-friendly state, Sub2 or Seller Finance`

**List 2: What I Bring to the Table** — Compelling value propositions for a seller or partner.
Format: `Bold Topic: specific detail`
Example: `Creative finance expertise: Subject-To, Seller Finance, JV structuring`

Present the lists and ask the investor to confirm or adjust before saving.

---

## Phase 2: Save to Cloud Brain

Once the investor confirms, save to cloud brain:

```
Path: brain/preferences/rei-buy-box.md
Tool: mcp__cloud-brain__write_note
```

**Content to save:**
- Investor name and contact info
- Strategies (1–2)
- Target markets (1–3)
- Property type / business type
- Price range
- Financial targets (min cash flow, min CoC, min cap rate)
- Condition tolerance
- Acquisition method preferences
- Partnership openness
- Deal breakers
- "Looking for" list (from Stage 5)
- "Bring to the table" list (from Stage 5)
- Date saved

Tell the user: *"Your Buy Box is saved. Now when you find a deal, just say 'Does this fit my buy box?' and I'll score it against your saved criteria."*

Also save to the investor's main preferences file to make it available to other toolbox skills:
```
Path: brain/preferences/rei-preferences.md
Tool: mcp__cloud-brain__write_note (merge with existing if found)
```

---

## Phase 3: Buy Box Infographic

Generate a professional, shareable buy box card as an HTML file.

### Step 1: Collect Branding Information (if not already in preferences)

Ask:
- Name and company name
- Tagline or short mission statement
- Phone, email, social handle, website
- Color preference: Navy/Teal (default), Charcoal/Gold (classic), or custom hex codes

### Step 2: Generate HTML Infographic

Use the `Write` tool to create an HTML file at `brain/buy-box-infographic-[slug]-[date].html`.

The HTML should render a 1080×1080px infographic (suitable for screenshots / social sharing) with:

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { width: 1080px; height: 1080px; font-family: 'Georgia', serif; background: [header_bg_color]; }
  .header { background: [header_bg_color]; color: white; padding: 40px 50px 30px; }
  .investor-name { font-size: 36px; font-weight: bold; letter-spacing: 2px; }
  .company { font-size: 20px; color: [accent_color]; margin-top: 4px; }
  .tagline { font-size: 16px; color: rgba(255,255,255,0.7); margin-top: 10px; font-style: italic; }
  .divider { height: 4px; background: [accent_color]; margin: 0; }
  .body { background: #FAFAFA; padding: 30px 50px; }
  .section-title { font-size: 13px; letter-spacing: 3px; text-transform: uppercase;
    color: [header_bg_color]; margin-bottom: 16px; padding-bottom: 8px;
    border-bottom: 2px solid [accent_color]; }
  .criteria-item { padding: 10px 0; border-bottom: 1px solid #E8E8E8; font-size: 15px; color: #333; line-height: 1.5; }
  .criteria-item strong { color: [header_bg_color]; }
  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; margin-top: 24px; }
  .footer { background: [header_bg_color]; color: white; padding: 20px 50px;
    display: flex; justify-content: space-between; align-items: center; margin-top: auto; }
  .contact-item { font-size: 13px; color: rgba(255,255,255,0.8); }
  .accent { color: [accent_color]; }
</style>
</head>
<body>
  <div class="header">
    <div class="investor-name">[INVESTOR NAME]</div>
    <div class="company">[COMPANY NAME]</div>
    <div class="tagline">[TAGLINE]</div>
  </div>
  <div class="divider"></div>
  <div class="body">
    <div class="two-col">
      <div>
        <div class="section-title">What I'm Looking For</div>
        [LOOKING_FOR items as .criteria-item divs with <strong>Topic</strong>: detail format]
      </div>
      <div>
        <div class="section-title">What I Bring to the Table</div>
        [BRING_TO_TABLE items as .criteria-item divs]
      </div>
    </div>
  </div>
  <div class="footer">
    <span class="contact-item">[PHONE]</span>
    <span class="contact-item accent">[WEBSITE]</span>
    <span class="contact-item">[SOCIAL HANDLE]</span>
    <span class="contact-item">[EMAIL]</span>
  </div>
</body>
</html>
```

Populate every placeholder with the investor's actual data from Phase 1 and the branding info collected. Render the "Looking For" and "Bring to Table" lists as proper HTML items.

Tell the user: *"Your Buy Box card has been generated. Open the HTML file in any browser and take a screenshot to get a shareable image for Instagram, LinkedIn, or Facebook groups. Suggest posting in local REIA Facebook groups and Sub2 Communities."*

---

## Deal Evaluator (On-Demand)

When the user provides a deal and asks "does this fit my buy box?":

1. Load buy box from `mcp__cloud-brain__search_notes` with query `buy box criteria`
2. Score the deal across all saved criteria:

| Criteria | Investor's Target | Deal's Value | Match? |
|----------|-----------------|--------------|--------|
| Geography | [Saved markets] | [Deal location] | 🟢/🟡/🔴 |
| Property type | [Saved type] | [Deal type] | 🟢/🟡/🔴 |
| Price range | $[Min]–$[Max] | $[Deal price] | 🟢/🟡/🔴 |
| Cash flow | $[Min]/door | $[Deal CF] | 🟢/🟡/🔴 |
| Cap rate | [Min]% | [Deal cap rate]% | 🟢/🟡/🔴 |
| Condition | [Saved pref] | [Deal condition] | 🟢/🟡/🔴 |
| Deal breakers | [List] | None / [Flag] | ✅ / 🚨 |

3. Output: Overall match score (0–100), verdict (Strong Fit / Partial Fit / Not a Fit), and 2–3 sentences on why.

---

## Error Handling

- **User wants to do everything:** Apply the anti-sprawl guardrail firmly but kindly.
- **No geography specified:** Ask before proceeding — drives all market research.
- **WebSearch unavailable:** Skip market research phase; proceed with user-provided knowledge.
- **No buy box in brain when evaluating:** Ask if they want to define one first or provide criteria ad hoc.
- **User wants infographic as PDF or PNG:** Explain the HTML file can be screenshot in any browser to produce a PNG; offer to note this is the export format.
