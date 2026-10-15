---
name: seller-net-sheet-calculator
description: >-
  Fast, accurate seller net proceeds calculator — what will the seller actually walk away with after commissions, closing costs, liens, repairs, and prorated expenses? Run multiple scenarios (list high / at market / to sell fast) side by side so sellers make informed pricing decisions. Use at listing appointments, price reduction conversations, and any time a seller asks 'how much will I net?' Triggers on: net sheet, seller net, how much will I make, seller proceeds, closing costs, what will I walk away with, net proceeds, seller closing costs, how much do I net.
---

# Seller Net Sheet Calculator

> **Disclaimer:** Net sheet calculations are estimates based on the information provided and typical closing cost ranges. Actual proceeds will vary based on final sale price, lender payoff amounts, title fees, local transfer taxes, prorations, and negotiated repairs. This skill is a professional planning tool — not a guarantee of proceeds. Always have your title company generate an official seller's estimated closing statement (HUD-1 or ALTA settlement statement) before closing. Consult a CPA or financial advisor regarding tax implications of the sale.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [Market/State]. Listing commission: [X]%. Buyer's agent commission: [X]%. Proceed or update?"

**If not found:** Run the setup interview below, then save.

**Setup Interview — ask all questions before proceeding:**

1. Your full name?
2. Brokerage name?
3. Primary state and market/city? (State is needed for transfer tax estimates.)
4. Your typical listing commission rate?
5. Typical buyer's agent commission offered in your market?
6. Any local transfer taxes or fees standard in your market? (e.g., $X per $1,000 of sale price, or a set percentage — check with your title company if unsure. If not known, we'll flag as "verify with title.")

Save preferences:

```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
  content: |
    # Real Estate Agent Pro — Preferences
    Agent: [name]
    Brokerage: [brokerage]
    State: [state]
    Market: [market]
    Listing Commission Rate: [rate]%
    Buyer's Agent Commission: [rate]%
    Local Transfer Tax: [rate or "verify with title"]
    Updated: [date]
```

Show a preferences banner at the top of every output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 1: COLLECT SELLER'S SITUATION

Ask the following questions. Mark optional items clearly — the seller doesn't have to provide them, but more detail = more accurate output.

**Required:**
1. Property address?
2. Estimated sale price — or a range if they're uncertain? (This will anchor the three scenarios in Step 4.)

**Mortgage and Liens (enter what you know — estimates are fine):**
3. Approximate current mortgage balance (first lien)? If they're not sure, ask for their most recent statement balance.
4. Second mortgage or HELOC balance (if applicable)?
5. Any other known liens — tax liens, HOA liens, judgment liens?
6. Do they have a solar panel system? If yes: owned outright or leased? (Leased solar requires payoff or transfer — affects net.)

**Other Costs:**
7. HOA — is there one? If yes, estimated HOA transfer fee or estoppel fee? (Often $100–$400, but varies widely.)
8. Any repairs or credits likely to be negotiated? (If they're not sure, use $0 and note it as a variable.)
9. Expected close date? (Needed for property tax proration estimate. If unknown, estimate 45–60 days from today.)
10. Approximate annual property taxes?
11. Monthly HOA dues (for proration, if applicable)?
12. Are they planning to offer a home warranty to the buyer?

---

## STEP 2: COST BREAKDOWN

Calculate and display each cost category. Flag every estimate clearly with "(est.)" and note items that require title company confirmation.

---

### A. Real Estate Commissions

*(Use rates from preferences)*

| | Amount |
|---|---|
| Listing Agent Commission ([X]%) | $[X] |
| Buyer's Agent Commission ([X]%) | $[X] |
| **Total Commission** | **$[X]** |

*Note: Post-NAR settlement, buyer's agent compensation is negotiable and may be structured differently. Confirm current market practice with your brokerage.*

---

### B. Closing Costs — Seller's Side

*(Ranges shown are estimates. Actual amounts vary by state, county, and title company. Items flagged "verify" should be confirmed with your title company before presenting to the seller as firm numbers.)*

| Cost Item | Low Estimate | High Estimate | Notes |
|---|---|---|---|
| Owner's Title Insurance | 0.5% | 1.0% of sale price | Varies by state — some states buyer pays; verify |
| Escrow / Settlement Fee | $500 | $1,500 | est. |
| Transfer Taxes | [rate from preferences] | [rate] | [verify with title if unknown] |
| Recording Fees | $50 | $200 | est. |
| Attorney Fees | $0 | $1,500 | Required in some states — flag if applicable |
| HOA Transfer / Estoppel Fee | [amount] | [amount] | If applicable |
| Home Warranty (if offering) | $400 | $700 | est. — optional |

**Seller's Closing Cost Subtotal (est.):** $[midpoint of range]

*For the net sheet calculation, use the midpoint of each range as the working estimate. Flag to seller that final numbers come from the title company.*

---

### C. Prorations

**Property Tax Proration**
Annual taxes ÷ 365 × number of days remaining in the tax year at close.

- Estimated annual taxes: $[from seller input]
- Estimated close date: [date]
- Days remaining in tax year from close: [X]
- **Property Tax Proration (credit to buyer at close): $[X] (est.)**

*Note: Proration direction (credit to buyer vs. credit to seller) depends on whether taxes are paid in arrears or in advance in your state. Confirm with title.*

**HOA Dues Proration** (if applicable)
- Monthly HOA dues: $[X]
- Days remaining in the closing month: [X]
- **HOA Proration: $[X] (est.)**

---

### D. Repairs and Concessions

- Estimated repair requests or buyer credits: $[from seller input, or $0 if unknown]
- Pre-listing staging/repair costs already incurred: $[from seller input, or $0]

*Note: Repair negotiations happen after inspection. This line is a planning estimate only.*

---

### E. Mortgage Payoff

- Outstanding balance: $[from seller input]
- Estimated accrued interest (30 days): approximately $[balance × (interest rate ÷ 12)] — ask seller for current rate if possible
- Lender payoff fee: $0–$500 (est.)
- **Estimated Mortgage Payoff: $[total] (est.)**

*For accurate payoff, seller should request a payoff statement from their lender specifying the anticipated payoff date.*

**Second Mortgage / HELOC Payoff** (if applicable): $[balance from seller input]

---

### F. Solar Payoff (if applicable)

- If **owned:** No impact on net proceeds (value may add to sale price).
- If **leased:** Payoff amount to terminate lease or transfer to buyer. ⚠️ Verify payoff amount with solar company — this is often $5,000–$30,000+ and catches sellers off guard.

**Estimated Solar Payoff:** $[amount or "verify with solar company"]

---

### G. Other Liens

- Tax liens: $[amount if known]
- Judgment liens: $[amount if known]
- HOA delinquency liens: $[amount if known]

*Note: All liens must be satisfied at or before closing. Title company will identify liens through title search.*

---

## STEP 3: SINGLE-SCENARIO NET PROCEEDS

If the seller has one specific sale price in mind, calculate and display the full net sheet for that price:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SELLER NET SHEET — [Property Address]
Prepared by: [Agent Name] | [Brokerage]
Date: [Date]      Estimated Close: [Date]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Sale Price:                        $[X]

DEDUCTIONS:
  Total Commission ([X]%):        -$[X]
  Seller's Closing Costs (est.):  -$[X]
  Property Tax Proration (est.):  -$[X]
  HOA Proration (est.):           -$[X]
  Repairs / Buyer Credits:        -$[X]
  Mortgage Payoff (est.):         -$[X]
  Second Lien Payoff:             -$[X]
  Solar Payoff:                   -$[X]
  Other Liens:                    -$[X]
                                ─────────
ESTIMATED NET PROCEEDS:            $[X]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠ All figures are ESTIMATES for planning purposes.
Final net proceeds will be determined by your
title company's official closing statement.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 4: THREE-SCENARIO COMPARISON

Run the calculation simultaneously for three pricing scenarios. Use the seller's estimated sale price as the anchor for the "Neutral" scenario, then calculate +5% and -5% (or agent-recommended range) for Aggressive and Competitive.

*(If the agent has a CMA-based range, use those specific prices instead.)*

| | Aggressive | Neutral | Competitive |
|---|---|---|---|
| **List Price** | $[HIGH] | $[TARGET] | $[LOW] |
| Total Commission | -$[X] | -$[X] | -$[X] |
| Seller's Closing Costs | -$[X] | -$[X] | -$[X] |
| Tax/HOA Prorations | -$[X] | -$[X] | -$[X] |
| Repairs / Concessions | -$[X] | -$[X] | -$[X] |
| Mortgage Payoff | -$[X] | -$[X] | -$[X] |
| Other Liens | -$[X] | -$[X] | -$[X] |
| **Estimated Net** | **$[X]** | **$[X]** | **$[X]** |
| Net Difference vs. Neutral | +$[X] | — | -$[X] |
| Est. Days to Offer | 30–60+ days | 15–35 days | 7–21 days |

**Agent's Note on the Three Scenarios:**

*(Personalize this interpretation based on current market conditions from preferences and any seller-specific context.)*

"The Aggressive scenario gets you $[X] more if everything works perfectly — but homes priced above market in [market] are currently averaging [X] days on market and often require price reductions that erode that advantage. The Competitive price gets you in front of the most buyers fastest — often triggering multiple offers that push the final price above asking. The Neutral price is where I'd recommend starting based on current comps, with a clear plan to review at 2 weeks if we're not seeing the activity we expect."

---

## STEP 5: CARRYING COST ANALYSIS

Use this when the seller is weighing a higher list price against the risk of holding longer.

**Calculate monthly carrying costs:**

| Cost | Monthly Amount |
|---|---|
| Mortgage Payment (P+I+taxes+insurance) | $[seller provides or estimate] |
| HOA Dues | $[amount] |
| Utilities (estimate) | $200–$400 |
| **Total Monthly Carrying Cost** | **$[total] (est.)** |

**The Math:**
"Every month you hold the home costs approximately $[X]. If the Aggressive pricing strategy results in your home sitting 45 days longer than the Neutral price, that's an additional $[X × 1.5 months] in carrying costs — which directly reduces your net.

Additionally, price reductions carry a psychological cost: buyers wonder why it was reduced and often submit lower offers after a reduction than they would have at an original correct price.

Here's the net comparison including estimated carrying costs:

- Aggressive (45 extra days, 2% price reduction after 30 days): Net = $[X]
- Neutral (on market 25 days): Net = $[X]

The difference in your favor from Neutral vs. Aggressive: approximately $[X]."

---

## STEP 6: FORMATTED OUTPUT FOR SELLER MEETING

Generate a clean, presentation-ready net sheet for use at the listing appointment or price reduction conversation.

**Formatting guidelines:**
- Brand the header with agent name and brokerage
- Use the single-scenario format for clean one-price discussions
- Use the three-scenario table when presenting pricing options side by side
- Always include the disclaimer prominently — sellers should understand these are estimates
- Print-ready: clean layout, no clutter, easy to read across a table

**Suggested talking point when presenting the net sheet:**

"This is your estimated net sheet. I want to walk through each line so there are no surprises at the closing table. The goal is for you to understand exactly what you're working with — so you can make a confident pricing decision and know what to expect when the final settlement statement arrives.

The most important line is your estimated net — and the most important thing to understand is that every dollar you add on sale price doesn't flow straight through to you. About [X]% of any additional sale price is absorbed by commissions, taxes, and closing costs. So a $10,000 higher sale price means approximately $[X] more in your pocket — which is still worth pursuing, but it's good to have clear expectations."

---

## SAVING TO MEMORY (optional)

When the agent requests it, save the net sheet for reference:

```
mcp__cloud-brain__write_note:
  path: brain/projects/net-sheet-[address-slug]-[YYYY-MM-DD].md
  content: |
    # Seller Net Sheet — [Address]
    Date: [date]
    Seller: [name(s)]
    
    Scenarios:
    - Aggressive ($[X]): Est. Net $[X]
    - Neutral ($[X]): Est. Net $[X]
    - Competitive ($[X]): Est. Net $[X]
    
    Mortgage Payoff: $[X] (est.)
    Other Liens: $[X]
    Monthly Carrying Cost: $[X]
    
    Notes: [any specific seller concerns or follow-up items]
```
