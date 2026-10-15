---
name: creative-vs-traditional-analyzer
description: >-
  Analyze whether a seller is a candidate for creative financing (seller finance, subject-to, sub2, owner finance, lease option, wrap mortgage) versus a traditional MLS sale. Score the seller situation, generate a side-by-side comparison, explain how agents still get paid on creative deals, highlight installment sale capital gains tax benefits, and build a one-page seller presentation. Use for motivated sellers, as-is properties, sellers facing foreclosure, behind on payments, capital gains concerns, or sellers who need speed over top dollar. Triggers on: creative vs traditional, seller finance candidate, subject-to candidate, creative financing analysis, installment sale, as-is sale options, how does agent get paid on creative.
---

> **Disclaimer:** Creative financing analysis involves legal, tax, and financial considerations specific to your state and situation. Installment sale tax treatment (IRC § 453) must be reviewed by a licensed CPA — tax savings are real but the structure must be correctly executed. Commission structures on creative transactions must be disclosed in writing and approved by your broker. Subject-to transactions carry due-on-sale clause risk that must be disclosed to all parties. This skill is a professional analysis framework — not legal, tax, or financial advice. Always work with a real estate attorney, CPA, and your broker before presenting or executing any creative financing arrangement.

# Creative vs. Traditional Financing Analyzer

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [State]. Proceed or update?"

**If not found:** Run the setup interview below, then save:

```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
  content: |
    # Real Estate Agent Pro — Preferences
    Agent: [name]
    Brokerage: [brokerage]
    State: [state]
    Brokerage Creative Finance Approval: [yes/no/checking]
    Commission Rate: [percent]
    Updated: [date]
```

**Setup interview questions:**
1. Your full name?
2. Brokerage name?
3. Primary state (affects installment sale and commission disclosure rules)?
4. Brokerage approval confirmed for creative finance transactions? (yes/no/checking)
5. Your typical commission rate (%)?

Show a preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [State]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Why This Skill Exists

Most agents default to traditional MLS sales. But sellers often have situations where **creative financing dramatically improves their outcome** — and agents still earn full commission (just structured differently).

This skill equips you to:
- **Identify** when a seller is a prime candidate for creative options
- **Score** the seller's situation objectively
- **Present** a professional comparison showing net proceeds, timeline, tax impact, and agent commission structure
- **Answer** the #1 agent fear: "How do I still get paid?"

---

## STEP 1: COLLECT SELLER SITUATION

Ask or parse the agent's description for:

| Item | Why It Matters |
|------|---|
| **Current mortgage balance** | Determines equity. Is it positive or underwater? Subject-To only works with equity. |
| **Estimated market value** | Baseline for all financing calculations. |
| **Mortgage interest rate** (if existing) | Below 5% = Subject-To is very attractive. High rate = Seller Finance spreads tax burden. |
| **Monthly payment (PITI)** | Shows seller's cash flow pain point. Subject-To eliminates this immediately. |
| **Seller motivation** | Divorce, relocation, tired landlord, probate, foreclosure, financial hardship, wants income not lump sum. Shapes best option. |
| **Timeline** | Days/weeks = Subject-To. Flexible = Seller Finance. |
| **Property condition** | Needs repairs = Buyer pays for condition in price (creative finance) OR investor buys as-is (Subject-To). |
| **Primary residence?** | Affects capital gains tax. Non-primary = higher tax risk in traditional sale. Installment sale treatment becomes powerful. |
| **Behind on payments / foreclosure?** | Subject-To is primary lifeline. Stops foreclosure, avoids credit damage. |
| **Does seller NEED all equity now?** | No = Seller Finance creates monthly income stream + massive tax deferral. Yes = Traditional or Subject-To (cash at close). |

---

## STEP 2: CANDIDATE SCORING

Score the seller 0–10 on each criterion. Then output:

**EXCELLENT (24+):**
- Motivated + positive equity + doesn't need lump sum OR behind on payments OR has capital gains concern
- High confidence in creative option closing

**GOOD (18–23):**
- Good equity position + some motivation + open to creative
- Creative option likely stronger than traditional

**POSSIBLE (12–17):**
- Neutral motivation + okay equity OR high motivation + marginal equity
- Worth presenting comparison

**LOW (0–11):**
- No urgency + small equity OR underwater
- Traditional sale likely best fit

**Scoring Factors (1–10 scale):**
1. **Motivation score** (1=none, 10=desperate urgency)
2. **Equity strength** (1=underwater, 10=substantial)
3. **Tax concern** (1=none, 10=major capital gains)
4. **Cash flow pain** (1=none, 10=behind on payments)
5. **Timeline flexibility** (1=must close in 7 days, 10=completely flexible)

---

## STEP 3: SIDE-BY-SIDE COMPARISON TABLE

Generate a table comparing AT LEAST: Traditional Sale, Seller Finance, Subject-To (add Lease-Option if seller is landlord-inclined).

### Comparison Dimensions

| Dimension | Notes |
|-----------|-------|
| **Estimated Net to Seller** | After commission, closing costs, repairs (if needed). Be specific with numbers. |
| **Timeline to Close** | Days / weeks. Sub-To closes fastest. |
| **Repairs Funded By** | Seller (traditional/seller finance) or buyer absorbs (Subject-To, as-is). |
| **Monthly Income Potential** | $0 (trad/Sub-To) vs. $X per month (seller finance/lease-option). |
| **Capital Gains Tax Impact** | Lump sum in year 1 (traditional) vs. installment spread over years (seller finance note). |
| **Buyer Pool Size** | Large (traditional) vs. smaller (Sub-To investors) vs. medium (seller finance buyers who can't qualify for banks). |
| **Likelihood of Closing** | Traditional: 80–85%. Creative: 90–95% (fewer contingencies, no appraisal delays). |
| **Agent Commission Structure** | THIS IS CRITICAL — see Step 4. Auto-populated with commission rate from preferences. |

---

## STEP 4: AGENT COMMISSION — THE TRUTH

**This is why agents hesitate on creative deals. Show them the numbers.** All examples below auto-populate your commission rate from preferences.

### Traditional Sale
- **Commission:** [commission rate from preferences]% of purchase price, paid at closing from seller proceeds
- **Reality:** If seller has little equity or repairs drain cash, this commission eats into net
- **Example:** $300k sale at [commission rate]% = $[calculated amount]

### Seller Finance (Owner Finance)
- **Commission:** [commission rate from preferences]% of **contract price** (the full amount buyer will eventually pay), paid at closing
- **Source:** From buyer's down payment funds OR structured as a deferred note (agent becomes a lienholder for unpaid commission)
- **Why it works:** Buyer is paying over time. Agent commission is paid upfront from the down payment portion, or the agent receives a note and gets paid as the buyer pays the seller
- **State note:** Many states allow agent commission via deferred note; check local rules and get broker approval
- **Example:** $300k seller finance (buyer pays down payment + agent commission at close), agent earns $[calculated amount]

### Subject-To
- **Commission:** Paid by seller from **equity pickup** at closing (the difference between sale price and remaining mortgage)
- **If equity is thin:** Commission negotiated as a note (deferred), or buyer brings cash to cover commission, or commission is percentage of equity only
- **Key:** In Sub-To, the agent must be transparent and have explicit broker approval. Commission structures are negotiated with buyer upfront
- **Example:** Seller has $50k equity. Buyer purchases property, takes over loan. Seller nets $50k, pays agent $3,500 (7% of equity) or as a negotiated note

### Wrap Mortgage / All-Inclusive Trust Deed
- **Commission:** [commission rate from preferences]% paid from buyer's down payment at closing (same as seller finance)
- **Source:** Structured into deal financing or buyer brings cash

### Lease-Option
- **Commission:** Can be earned **twice**
  - At option execution: Option fee split (state-dependent; some allow 50%+ to agent)
  - At purchase: Full commission on sale price if option is exercised
- **Example:** $300k property, 10% option fee ($30k), 1-year option. Agent earns $3,000 at option. If buyer exercises, agent earns more at full commission rate

---

## CRITICAL COMMISSION DISCLOSURE

**"Always have your broker review any creative transaction compensation structure before presenting to the seller. Creative deals require written disclosure of compensation in most states. Commission must be transparent and agreed in writing by all parties."**

Agents who hide creative commission structures risk disciplinary action. Transparency = professional credibility.

---

## STEP 5: SELLER BENEFITS DEEP DIVE

### Subject-To Benefits
**Use this when:** Seller is behind on payments, facing foreclosure, needs to close fast, property needs repairs
- Seller gets **immediate relief** from mortgage payments (investor takes over)
- No 45-day traditional sale timeline — closes in 7–14 days
- Investor buys **as-is** — no repairs required
- Avoids foreclosure + credit damage (if behind on payments)
- **Equity pickup:** Seller still receives any equity difference at closing

### Seller Finance (Owner Finance) Benefits
**Use this when:** Seller doesn't need all cash at once, has capital gains concern, wants monthly income, or property has condition issues

**TAX BENEFIT — INSTALLMENT SALE (IRC § 453):**
This is the biggest seller benefit most agents miss.

- **Traditional sale:** Seller receives all proceeds in year 1. Capital gains tax is due on profit in year 1. Example: $150k gain = $30k+ capital gains tax owed immediately (assuming 20% federal rate + state tax).
- **Seller Finance:** Seller receives payments over time. Tax is paid **only on the portion received each year**. Example: $150k gain spread over 10-year note. Year 1: seller receives $50k down + $X monthly = pays tax only on that year's amount (~$7–8k instead of $30k lump sum). Defers tens of thousands of dollars in tax.

**Other Seller Finance Benefits:**
- **Monthly income stream:** Seller earns 6–8% interest on the unpaid note balance. On a $200k note at 7% = $14k/year passive income
- **Full asking price:** Buyer pays for the favorable terms. Seller can price at full market value (or above) because buyer is getting bank-alternative financing
- **Faster close:** No bank appraisal, no lender delays, no underwriting. Close in 14–21 days
- **Larger buyer pool:** Buyers with jobs/income but credit issues or insufficient down payment can now buy
- **As-is accepted:** Buyer accepts condition because they're not getting bank inspection

---

## STEP 6: GENERATE THE SELLER PRESENTATION

Create a 1-page talking points document. Agent name, brokerage, state, and commission rate auto-populated from preferences throughout.

### Seller's Situation Summary
- Mortgage balance: $X | Market value: $X | Equity: $X
- Motivation: [From Step 1]
- Key pain point: [From Step 1]

### Recommended Strategy
- **Option:** [Seller Finance / Subject-To / Wrap / Lease-Option]
- **Why it fits:** [Specific to seller's situation — speed, tax, income, no repairs, etc.]
- **Your outcome:** [Specific net to seller, timeline, and benefit]

### Simplified Comparison Table
| | Traditional | **RECOMMENDED** |
|---|---|---|
| Est. net to seller | $X | $X |
| Timeline | 45 days | 21 days |
| Repairs required? | Yes | No |
| Monthly income? | $0 | $X |
| Tax deferral? | No | Yes (5 years+) |
| **Agent commission** | [rate]% from proceeds | [rate]% from down payment / note |

### Key Benefit for THIS Seller
(Pick one and explain in 2–3 sentences)
- **Speed:** Closes in [timeframe]. No appraisal delays.
- **Tax savings:** Spreads capital gains over [years]. Saves ~$[amount] vs. lump sum tax in year 1.
- **Monthly income:** Creates $X/month passive income for [years].
- **As-is:** Investor buys condition as-is. No repair investment required.
- **Avoid foreclosure:** Stops foreclosure process. Preserves credit.

### Agent Commission (Transparency)
"My commission on this deal will be [commission rate from preferences]% on the contract price. If structured as [down payment / deferred note], here's exactly how it works: [explain]. This is transparent and disclosed in writing."

### Call to Action
"Does this sound like it might work better for your situation than listing on the MLS? Would you like me to run the numbers on what terms could look like?"

---

## QUICK REFERENCE: WHEN TO USE EACH OPTION

| Seller Situation | Best Option | Why |
|---|---|---|
| **Behind on payments / foreclosure risk** | Subject-To | Stops foreclosure. Closes in days. |
| **Wants monthly income + has capital gains** | Seller Finance | Tax deferral + passive income. |
| **Needs repairs paid by buyer** | Seller Finance / Subject-To | Investor accepts as-is or buyer finances repairs. |
| **Small equity, low motivation** | Traditional | Not enough upside for creative hassle. |
| **Flexible timeline, doesn't need cash** | Seller Finance / Lease-Option | Tax benefits shine. Monthly income. |
| **Wants to stay involved, collect rent** | Lease-Option | Monthly payments. Option fee upfront. Two commission events. |
| **Low interest rate mortgage + motivated** | Subject-To | Keep seller's rate. Close fast. Investor gains. |

---

## WHY AGENTS WIN WITH CREATIVE

- **Commission:** You still earn [commission rate from preferences]%, just structured differently
- **Closing rate:** 90–95% vs. 80–85% for traditional (fewer contingencies)
- **Timeline:** Faster close = faster commission
- **Relationships:** Seller sees you as problem-solver, not just lister
- **Repeat business:** Solved sellers refer you to friends in similar situations

**The real edge:** Most agents only know traditional. You become the expert who opens doors sellers didn't know existed.
