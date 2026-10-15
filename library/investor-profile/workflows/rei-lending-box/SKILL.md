---
name: rei-lending-box
description: >-
  Real estate and private money Lending Box definition and infographic generation skill. Use when a user wants to: (1) Define their lending criteria through a guided interview, (2) Determine which lending strategy fits their capital and risk tolerance (Gator/EMD, Transactional, Private Money, Syndications, Funds, Fractional), (3) Save their lending box to the brain for future deal evaluation, (4) Generate a professional shareable lending box infographic (HTML). Informed by creative-finance lending frameworks (e.g. the Gator Method) and traditional private money frameworks.
---

# Real Estate Lending Box Skill

> **Disclaimer:** Private money lending and hard money lending involve risk of loss. Loan terms, returns, and lien positions described herein are illustrative. Consult a licensed attorney and financial advisor before deploying capital as a private lender. Securities laws may apply to certain lending structures — always consult legal counsel.

## Overview

A **Lending Box** is a private lender's filter — the specific criteria used to deploy capital into real estate or business deals. It tells wholesalers, flippers, and operators exactly what types of loans will be funded, preventing wasted time on deals outside the lender's risk profile.

**Core philosophy:** Capital has different velocities. Some investors want high-velocity short-term returns (Gator Method / EMD lending); others want long-term passive wealth preservation (Syndications / Funds). This skill helps lenders find their lane and communicate it clearly.

## Workflow

Three phases, completed in order:
- **Phase 1 — Interview & Strategy Matching:** Conversational interview to define the lending box.
- **Phase 2 — Brain Save:** Save finalized lending box to cloud brain.
- **Phase 3 — Infographic:** Generate a professional shareable lending box card (HTML file).

---

## Pre-Flight — Check for Existing Lending Box

1. Use `mcp__cloud-brain__search_notes` with query `lending box criteria`
2. **If found:** Display ⚙️ banner. Confirm: "Found your saved lending box from [date]. Want to (1) update it, (2) start fresh, or (3) evaluate a loan request against it?"
3. **If not found:** Proceed to Phase 1 interview.
4. **If user provides a loan request to evaluate:** Jump directly to the Evaluator.

---

## Phase 1: The Lending Box Interview

Conduct conversationally — ask 1–2 questions at a time, wait for answers, then adapt.

### Stage 1: Capital, Goals & Risk Tolerance

1. **Capital Available:** "How much capital are you looking to deploy right now? (Under $10k, $50k–$100k, $100k–$500k, or $500k+)"
2. **Liquidity Needs:** "How quickly do you need this money back? Short-term velocity (days/weeks), medium-term (6–12 months), or long-term passive (3–5+ years)?"
3. **Involvement Level:** "Do you want to be active (reviewing individual deals, networking with wholesalers) or completely passive (fund or syndication)?"
4. **Risk Appetite:** "Are you focused on capital preservation (1st lien, very safe) or willing to take on more risk for higher returns or equity upside?"

### Stage 2: Strategy Matching

Based on Stage 1, recommend 1–2 strategies:

| If the lender has/wants... | Recommend |
|---|---|
| Low capital (<$20k) + Active | EMD Lending (Gator 1.0) or Fractional Debt |
| High capital + Wants velocity (days) | Transactional Funding (Double Close) |
| Moderate capital ($50k+) + 6–12 month horizon | Fix & Flip / BRRRR Private Money Lending |
| High capital + Wants passive / hands-off | Real Estate Syndications or Private Debt Funds |
| Wants equity upside | Gap Funding / 2nd Lien Lending |

**Strategy profiles:**

| Strategy | Capital Needed | Typical Returns | Risk | Horizon |
|----------|---------------|----------------|------|---------|
| **EMD Lending (Gator 1.0)** | $1k–$50k | $500–$2k flat / deal | Low (refundable EMD) | Days–2 weeks |
| **Transactional Funding** | $25k–$500k | 1–2% flat fee | Low (hours exposure) | Same-day–48 hrs |
| **Fix & Flip Private Money** | $50k–$500k | 10–15% + 1–3 pts | Medium (1st lien) | 3–12 months |
| **BRRRR Private Money** | $50k–$300k | 8–12% | Medium (1st lien) | 6–18 months |
| **Gap / 2nd Lien** | $10k–$100k | 15–25% | Higher (2nd lien) | 3–12 months |
| **Syndication LP** | $25k–$500k | 6–10% pref + equity | Medium-Low | 3–7 years |
| **Private Debt Fund** | $50k–$1M+ | 8–12% | Low–Medium | 1–3 years |
| **Fractional Debt** | $5k–$25k | 8–12% | Medium (fractional notes) | 6–24 months |

### Stage 3: Live Market Research (Optional)

Once 1–2 strategies selected, offer: *"Would you like me to run a quick market analysis on current rates and trends for [Strategy]?"*

If yes: Use `WebSearch`:
- `"private money lending rates [strategy] 2025 2026"`
- `"hard money lending rates [state] [year]"`
- `"EMD lending Gator method returns [year]"`
- `"real estate syndication returns [year]"`

Present current typical returns, LTV requirements, and popular asset classes for the lending type.

### Stage 4: Define Specific Lending Products

Help the user define 2–4 specific **Lending Products** — a product is a specific combination of loan type, amount, terms, and asset class.

*"Let's define your specific Lending Products — think of these like items on a menu a borrower can choose from."*

For each product, define:
1. **Product Name:** (e.g., "Transactional Funding", "Fix & Flip Private Money")
2. **Max Amount:** (e.g., "Up to $25k", "Up to $250k")
3. **Asset Class / Lien Position:** (e.g., "SFR only, 1st lien position")
4. **Terms / Rate:** (e.g., "1–2% flat fee", "12% + 2 points, 12-month max")
5. **Geographic Reach:** (e.g., "Texas only", "Nationwide")

### Stage 5: Value Proposition & Deal Breakers

1. "What makes you a great lending partner? (Speed of funding, no credit checks, flexible draws, nationwide reach, simple terms)"
2. "Any absolute deal breakers? (No 2nd liens, no rural properties, no spec construction)"

### Stage 6: Synthesis — The Two Lists

Two focused lists for the infographic. **Quality over quantity — 3–4 sharp items, no filler.**

**List 1: My Lending Products** — specific products from Stage 4.
Format: `Bold Product Name: specific details`
Example: `Transactional Funding: up to $25k SFR, same-day double-close, 1-2% flat fee`

**List 2: What I Offer** — value propositions.
Format: `Bold Topic: specific detail`
Example: `Speed: funding decisions within 24 hours, no committees`

Present and ask to confirm before saving.

---

## Phase 2: Save to Cloud Brain

```
Path: brain/preferences/rei-lending-box.md
Tool: mcp__cloud-brain__write_note
```

**Content to save:**
- Lender name and contact info
- Strategies (1–2)
- Capital available (range)
- Liquidity horizon
- Risk appetite
- Lending products (all 2–4 with full details)
- Deal breakers
- "Lending Products" list (from Stage 6)
- "What I Offer" list (from Stage 6)
- Date saved

Tell the user: *"Your Lending Box is saved. When a wholesaler or flipper brings you a deal, just say 'Does this loan request fit my lending box?' and I'll evaluate it for you."*

---

## Phase 3: Lending Box Infographic

Generate a professional, shareable lending box card as an HTML file.

### Step 1: Collect Branding Information (if not already in preferences)

Ask:
- Name and company name
- Tagline (e.g., "Fast capital for serious investors")
- Phone, email, social handle, website
- Color preference: Navy/Gold (classic), Slate/Teal (modern), or custom hex codes

### Step 2: Generate HTML Infographic

Use the `Write` tool to create an HTML file at `brain/lending-box-infographic-[slug]-[date].html`.

The HTML renders a 1080×1080px card with:

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { width: 1080px; height: 1080px; font-family: 'Georgia', serif; background: [header_bg_color]; }
  .header { background: [header_bg_color]; color: white; padding: 40px 50px 30px; }
  .lender-name { font-size: 36px; font-weight: bold; letter-spacing: 2px; }
  .company { font-size: 20px; color: [accent_color]; margin-top: 4px; }
  .tagline { font-size: 16px; color: rgba(255,255,255,0.7); margin-top: 10px; font-style: italic; }
  .badge { display: inline-block; background: [accent_color]; color: [header_bg_color];
    font-size: 11px; font-weight: bold; letter-spacing: 2px; padding: 4px 12px;
    text-transform: uppercase; margin-top: 12px; }
  .divider { height: 4px; background: [accent_color]; }
  .body { background: #FAFAFA; padding: 30px 50px; }
  .section-title { font-size: 13px; letter-spacing: 3px; text-transform: uppercase;
    color: [header_bg_color]; margin-bottom: 16px; padding-bottom: 8px;
    border-bottom: 2px solid [accent_color]; }
  .product-item { padding: 10px 0; border-bottom: 1px solid #E8E8E8; font-size: 15px; color: #333; line-height: 1.5; }
  .product-item strong { color: [header_bg_color]; }
  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; margin-top: 24px; }
  .footer { background: [header_bg_color]; color: white; padding: 20px 50px;
    display: flex; justify-content: space-between; align-items: center; }
  .contact-item { font-size: 13px; color: rgba(255,255,255,0.8); }
  .accent { color: [accent_color]; }
</style>
</head>
<body>
  <div class="header">
    <div class="lender-name">[LENDER NAME]</div>
    <div class="company">[COMPANY NAME]</div>
    <div class="tagline">[TAGLINE]</div>
    <div class="badge">Private Lender</div>
  </div>
  <div class="divider"></div>
  <div class="body">
    <div class="two-col">
      <div>
        <div class="section-title">My Lending Products</div>
        [LENDING_PRODUCTS items as .product-item divs]
      </div>
      <div>
        <div class="section-title">What I Offer</div>
        [WHAT_I_OFFER items as .product-item divs]
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

Tell the user: *"Your Lending Box card has been generated. Open the HTML file in any browser and take a screenshot to get a shareable image. Share in Gator Communities, wholesaler Facebook groups, REIA meetings, and LinkedIn."*

---

## Loan Request Evaluator (On-Demand)

When user says "does this loan request fit my lending box?":

1. Load lending box from `mcp__cloud-brain__search_notes` with query `lending box criteria`
2. Score the request:

| Criteria | My Lending Box | Request Details | Match? |
|----------|---------------|----------------|--------|
| Product type | [Products] | [Request type] | 🟢/🟡/🔴 |
| Loan amount | Up to $[Max] | $[Requested] | 🟢/🟡/🔴 |
| Asset class | [Saved class] | [Deal type] | 🟢/🟡/🔴 |
| Lien position | [Saved] | [Requested] | 🟢/🟡/🔴 |
| Geographic reach | [Saved] | [Location] | 🟢/🟡/🔴 |
| Deal breakers | [List] | None / [Flag] | ✅ / 🚨 |

3. Output: Overall match score (0–100), verdict, and recommended terms or why it's a pass.

---

## Error Handling

- **User isn't sure what type of lender they want to be:** Walk through Stage 1 more slowly. Ask about capital amount and liquidity first — those two alone narrow it significantly.
- **User has mixed strategies (short and long term):** Allow 2 products max per lending box; note they can always update.
- **WebSearch unavailable:** Skip market research; proceed with user-provided knowledge.
- **No lending box in brain when evaluating:** Ask if they want to define one, or evaluate ad hoc against criteria they provide now.
- **User asks about securities laws (fund structures, syndications):** Note that certain lending structures constitute securities offerings requiring compliance with SEC regulations. Always recommend consulting a securities attorney before accepting capital from multiple investors.
