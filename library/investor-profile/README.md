# REInvestor Profile

**Plugin version:** 1.0.0
**Publisher:** the provider (the provider)
**Category:** Real Estate Investing

---

## What This Plugin Does

REInvestor Profile is the identity and network layer for real estate investors. It helps you define who you are as an investor (your buy box and lending box), manage your network of buyers and lenders, and generate professional shareable infographics that attract deals and capital to you.

Works standalone or as the profile layer that feeds REInvestor Toolbox — when your buy box is saved here, the Toolbox's deal analyzer automatically scores deals against your personal criteria.

---

## Skills Included

### rei-buy-box
Define your investment criteria through a guided interview informed by well-known creative-finance educators, Jamil Damji (AstroFlipping), and Roland Frasier (EPIC/Business Acquisitions) frameworks. Covers real estate, commercial, and business acquisitions. Includes live market research to identify top-performing markets for your strategy. Generates a professional buy box infographic (HTML card, 1080×1080 — screenshot to share on social).

**Built-in evaluator:** After your buy box is saved, say "does this deal fit my buy box?" with any deal description and receive a scored analysis against your personal criteria.

**Trigger phrases:** "define my buy box," "set up my investor criteria," "update my buy box," "does this deal fit my criteria," "generate my buy box card," "what's my buy box"

### rei-lending-box
Define your private money lending criteria — capital amount, liquidity horizon, risk appetite, and specific lending products (EMD/Gator, transactional funding, fix & flip private money, syndication LP, debt fund, gap/2nd lien). Generates a professional lending box infographic card for sharing with wholesalers, flippers, and REIA groups.

**Built-in evaluator:** Say "does this loan request fit my lending box?" to score any incoming deal request against your saved criteria.

**Trigger phrases:** "define my lending box," "set up my lending criteria," "what loan products do I offer," "does this loan fit my box," "generate my lending box card," "Gator lending," "private money lending box"

### rei-network-buy-box
Manage a searchable database of buyers and investors in your network — each with their specific buy box criteria. Add buyers from text, spreadsheets, or vCards. Match any incoming deal to the best-fit buyers in your database. Draft professional outreach messages that reference the specific deal and how it aligns with each buyer's criteria.

**Trigger phrases:** "save this buyer's criteria," "add to my buyer database," "who buys in [city]," "find buyers for this deal," "draft outreach to matching buyers," "show my buyer list," "what does [name] buy"

---

## Two-Phase Design

The buy box is the foundation. Save it once and it powers deal evaluation across both plugins:

```
rei-buy-box                     rei-lending-box
(your criteria)                 (your lending products)
      ↓                               ↓
brain/preferences/             brain/preferences/
rei-buy-box.md                 rei-lending-box.md
      ↓
REInvestor Toolbox → rei-deal-analyzer reads buy box
                   → scores every deal against YOUR targets
                   → not generic benchmarks
```

---

## Investor Buy Box Framework

The buy box interview covers:

- **Strategy** (1–2 strategies recommended — anti-sprawl guardrail built in)
- **Target markets** (1–3 geographies with boots-on-the-ground access)
- **Property type and size**
- **Financial targets** (price range, min cash flow, min cap rate, min CoC)
- **Condition tolerance** (turnkey / light / medium / heavy)
- **Acquisition methods** (cash / conventional / seller finance / Sub2 / DSCR)
- **Partnership openness** (JV structure, what you bring vs. need)
- **Deal breakers** (absolute disqualifiers)

Outputs a two-list summary: "What I'm Looking For" and "What I Bring to the Table" — formatted for the infographic and for sharing with wholesalers and brokers.

---

## Infographic Format

Both `rei-buy-box` and `rei-lending-box` generate a professional 1080×1080px HTML card:

- Two-column layout: criteria left / value proposition right
- Your name, company, tagline, and contact info
- Color customizable (navy/teal default, charcoal/gold classic, or custom hex)
- To convert to PNG: open the HTML file in any browser → take a screenshot

Suggested sharing channels: Instagram, LinkedIn, Facebook wholesaler groups, Sub2 Owners Club community posts, REIA meetings.

---

## Network Buy Box Use Cases

| Use Case | What to Say |
|----------|-------------|
| Add a buyer from a networking event | "Add Jane Smith's buy box: she buys SFRs in Dallas under $250k, cash, 3BR+, needs help negotiating" |
| Add from a spreadsheet | Paste the row — skill will parse and save |
| Find buyers for a deal | "I have a 3/2 in Fort Worth for $185k, light rehab — who in my network buys this?" |
| Look up a specific buyer | "What does John at ABC Capital buy?" |
| Draft outreach | "Draft outreach to the top 3 matches for this deal" |
| See the full list | "Show me my buyer database" |

---

## Disclaimer

Strategy recommendations and market data are for informational purposes only. Investment strategies involve risk. Private money lending may constitute a securities offering in certain structures — always consult a licensed securities attorney before accepting capital from multiple investors. All projections are estimates. Consult licensed financial and legal advisors before investing.

---

*REInvestor Profile | the provider | v1.0.0*


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
