# REInvestor Profile — Agent Capabilities Manifest

*For use by system-03 (Agent Designer) and system-04 (Agent Activator)*
*Last updated: 2026-06-12*

---

## About This Plugin

`investor-profile` is the investor identity layer for the REInvestor platform. It defines who the investor is — what they buy, how they lend, and who they know. The three skills in this plugin are the foundation that makes the entire REInvestor Toolbox function: when an investor defines their buy box here, every deal the Toolbox analyzes is automatically scored against those saved criteria. When they build their buyer network here, they can match any deal to the right buyer instantly.

**Target users:** Any real estate investor who wants to systematize their acquisition criteria, attract deal flow from their network, or present their investor identity professionally. Works equally well for first-time investors who are still figuring out their strategy and experienced investors who want to get their criteria out of their head and into a system.

**Integration with real-estate-investor:** rei-buy-box saves to `brain/preferences/rei-buy-box.md` and merges into `brain/preferences/rei-preferences.md`. Every real-estate-investor skill reads from that path. Installing and running investor-profile first is the recommended starting point before using the Toolbox.

**Plugin version:** 1.0.0 (3 skills at release)

---

## Available Skills Catalog

Each entry lists: what the skill does, what preferences it collects at activation, its recommended schedule, and which other skills it pairs with naturally.

---

### 1. rei-buy-box

**What it does:** Guides an investor through a structured 5-stage interview to define their investment criteria (their "buy box") — the filter they apply to every deal to decide if it's worth pursuing. Covers goals and risk tolerance, strategy selection (with a built-in decision table covering BRRRR, buy-and-hold, flip, seller finance, subject-to, wholesale, and more), live market research for the investor's target geography using WebSearch, and specific acquisition criteria (price range, property type, location, minimum equity, condition, cash flow targets). Generates a professional social-media-ready infographic (HTML file, 1080×1080px) for Instagram, LinkedIn, and Facebook. Includes a built-in deal evaluator — the investor can ask "does this deal fit my buy box?" and get a scored 🟢/🟡/🔴 breakdown against each saved criterion.

**Preferences collected at activation:**
- Full name and company name
- Life stage / primary investing goal (wealth building / income / portfolio diversification / legacy)
- Time horizon (short-term 1–3y / medium 3–7y / long-term 7y+)
- Tolerance for active involvement (fully active / semi-passive / fully passive)
- Primary strategy (determined via decision table during interview)
- Target geography (city/state — 1–3 markets)
- Target property types (SFR / 2–4 unit / small MF / mobile home / land)
- Price range (min/max)
- Minimum equity requirement (%)
- Condition tolerance (turnkey / cosmetic / full renovation)
- Target cash flow per door ($/month)
- Target equity capture at purchase (%)
- Target ARV ($ or range)
- Deal breakers (what the investor will never touch)

**Suggested schedule:** On-demand (with annual review). Run once on setup, then update when strategy or market focus changes. system-04 can schedule an annual "buy box review" prompt.

**Natural pairings:** rei-deal-analyzer in real-estate-investor (buy box criteria pre-load automatically for deal scoring), rei-network-buy-box (buy box defines what this investor is looking for, useful when entering the buyer database), rei-lending-box (investors often define both at the same time).

---

### 2. rei-lending-box

**What it does:** Guides an investor who also lends capital through a structured 6-stage interview to define their lending criteria — what kinds of loans they make, at what terms, in what markets, and at what risk profile. Covers capital available, lending goals, risk tolerance, strategy selection (covering Gator/EMD, transactional funding, private money, syndications, funds, and fractional), live market research for target lending geographies, and 2–4 specific lending products (e.g., "I do 6-month EMD loans up to $25K at 10% annualized, SFR only, 50 miles of Phoenix"). Generates a professional lending-box infographic (HTML file, 1080×1080px). Includes a built-in loan request evaluator — lenders can ask "does this loan request fit my lending box?" and get a 🟢/🟡/🔴 scorecard.

**Preferences collected at activation:**
- Full name and company name
- Capital available for lending (range)
- Lending goal (passive income / portfolio support / network building / business)
- Risk tolerance (conservative / moderate / growth)
- Lending strategy (determined via decision table)
- Target lending geographies (1–3 markets or nationwide)
- 2–4 specific lending products (each with: loan type, amount range, rate, term, collateral type)
- Hard deal-breakers (what the lender will never fund)

**Suggested schedule:** On-demand (with annual review). Run once on setup, then update when capital position or lending strategy changes.

**Natural pairings:** rei-buy-box (many investors both buy and lend — profiles are often built together), rei-network-buy-box (lending criteria can be added to buyer network profile), rei-offer-generator in real-estate-investor (understanding available private money helps structure creative finance offers).

---

### 3. rei-network-buy-box

**What it does:** Manages the investor's buyer and capital partner network. Handles four actions: (1) Add or update a buyer/lender profile — saves their criteria to cloud brain; (2) Match a specific deal — scores the deal against every saved profile and returns Strong/Partial/Weak matches sorted by strength; (3) Draft outreach — generates personalized outreach messages for matched buyers; (4) View database — shows all saved profiles in a summary table with a gap analysis (underrepresented strategy or geography combos in the network). Outreach templates vary by deal type: wholesale, creative finance, or capital raise.

**Preferences collected at activation:**
- Investor full name (as network manager)
- Company name

**Suggested schedule:** On-demand — triggered when the investor has a deal to move, is adding a new contact to the network, or wants to review network coverage.

**Natural pairings:** rei-buy-box (the investor's own criteria serve as a reference template when adding peers), rei-deal-analyzer and rei-deal-marketing in real-estate-investor (deal data from analyzer can be passed directly into network match and outreach).

---

## Preferences Registry

All preferences collectible at investor activation, organized by category. system-04 should take the union of all fields needed by the assigned skills and run one consolidated interview.

**Core Identity — needed by all skills:**
- Investor full name
- Company name

**Buy Box Criteria — needed by rei-buy-box:**
- Life stage / investing goal
- Time horizon
- Tolerance for active involvement
- Primary strategy
- Target geography (1–3 markets)
- Target property types
- Price range (min/max)
- Minimum equity requirement (%)
- Condition tolerance
- Target cash flow per door ($/month)
- Target equity capture at purchase (%)
- Target ARV ($ or range)
- Deal breakers

**Lending Criteria — needed by rei-lending-box:**
- Capital available for lending (range)
- Lending goal
- Risk tolerance
- Lending strategy
- Target lending geographies
- 2–4 specific lending products (type, amount, rate, term, collateral)
- Hard deal-breakers

**Network Management — needed by rei-network-buy-box:**
- No additional activation fields beyond Core Identity. Network contacts are collected as job inputs per-session, not stored as preferences.

---

## Suggested Agent Configurations

**These are starting-point suggestions only.** Skills can be assigned to any agent in any combination.

---

### Option A: Solo Profile Agent (One AI Assistant)

One "Investor Identity" agent handles all three skills.

**Best for:** Any investor setting up for the first time. Run buy-box and lending-box in one session, then add network contacts over time.

**Activation interview:** Core Identity + Buy Box Criteria + Lending Criteria (only collect Lending Criteria if the investor also lends capital). Approximately 10–15 minutes for the full profile.

---

### Option B: Two-Agent Setup

**Identity Agent**
Skills: rei-buy-box, rei-lending-box
Activation fields: Core Identity, Buy Box Criteria, Lending Criteria
*Note: Run this agent first. The saved profiles feed directly into the Toolbox.*

**Network Agent**
Skills: rei-network-buy-box
Activation fields: Core Identity only (network contacts are job inputs, not preferences)

---

## Recommended Schedules

| Skill | Recommended Schedule | Notes |
|---|---|---|
| rei-buy-box | Annual review prompt | Run once on setup. Flag for update if strategy or market focus shifts. system-04 can schedule a "time to review your buy box?" prompt once per year. |
| rei-lending-box | Annual review prompt | Same as buy box — run once, review annually or when capital position changes. |
| rei-network-buy-box | On-demand | Triggered when the investor has a deal to move, a new contact to add, or wants a network gap report. |

---

## How system-03 Should Use This Document

When a client's brain context indicates they are a real estate investor (from system-02 business blueprint):

1. **Always surface investor-profile before real-estate-investor.** This plugin establishes the investor's identity — the buy box and preferences that make every Toolbox skill work correctly. Without it, the Toolbox skills will ask for criteria from scratch every run.
2. **Check if buy box already exists.** If the client's brain has an existing `brain/preferences/rei-buy-box.md`, they may have already run this skill. Show them the existing profile and offer to update it rather than starting fresh.
3. **The decision to include rei-lending-box depends on whether the client lends capital.** Ask directly: "Do you also lend private money to other investors?" If yes, include it. If no, skip it.
4. **rei-network-buy-box is valuable for any investor with deal flow.** If the client mentions they wholesale deals, flip and need buyers, or run a buyers list, include this skill. It's also useful for investors who want to track lending partners.
5. **Pair with real-estate-investor.** In most configurations, this plugin and the Toolbox belong to related agents or the same agent. The Profile defines identity; the Toolbox does the work. Suggest both together unless the client explicitly only wants one.

---

## How system-04 Should Use This Document

When activating an agent assigned `investor-profile` skills:

1. **Identify which skills are assigned to this agent** from the system-03 design output.
2. **For rei-buy-box:** Run the full 5-stage interview (Goals/Risk → Strategy → Market Research → Criteria → Synthesis). The interview is embedded in the skill itself — system-04 triggers the skill and the skill handles the interview flow. Save result to `brain/preferences/rei-buy-box.md` and `brain/preferences/rei-preferences.md`.
3. **For rei-lending-box:** Run the 6-stage interview only if the investor lends capital. Save to `brain/preferences/rei-lending-box.md`.
4. **For rei-network-buy-box:** Collect only Core Identity (name and company). The skill's job inputs (individual buyer contacts) are collected per-session — do not try to pre-collect them during activation.
5. **Do not schedule recurring runs for buy-box or lending-box.** These skills are designed to be run on-demand at setup and updated when strategy changes. Optionally create a lightweight annual reminder: "It's been a year since you set up your buy box — want to review it?"
6. **Write preferences to:**
   ```
   brain/preferences/rei-buy-box.md       ← buy box profile
   brain/preferences/rei-lending-box.md   ← lending profile
   brain/preferences/rei-preferences.md   ← merged master profile (buy box merges here automatically)
   ```
