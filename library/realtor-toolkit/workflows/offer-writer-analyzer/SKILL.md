---
name: offer-writer-analyzer
description: >-
  Two-mode offer tool for real estate agents. Buyer agent mode: write a competitive offer strategy with escalation clauses, cover letter, contingency recommendations, and seller-friendly terms that win in any market. Listing agent mode: analyze multiple offers side by side and generate a seller presentation to make an informed decision. Use whenever writing or reviewing a real estate purchase offer. Triggers on: write an offer, make an offer, offer strategy, escalation clause, offer letter, multiple offers, analyze offers, compare offers, present offers to seller, offer review, best offer, which offer should we take, bidding war.
---

# Offer Writer & Analyzer

> **Disclaimer:** Offer writing and analysis is a professional skill requiring knowledge of your local market, contract law, and negotiation strategy. This skill generates templates and frameworks — all offer documents must be reviewed by a licensed real estate agent or attorney before submission. Contract terms, contingencies, and legal requirements vary by state. Never submit an offer generated solely by this tool without your professional review and compliance with your state's contract requirements. This is not legal advice.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [Market]. Market condition: [buyer's/balanced/seller's]. Proceed or update?"

**If not found:** Run the setup interview below, then save.

**Setup Interview — ask all questions before proceeding:**

1. Your full name?
2. Brokerage name?
3. Primary market / city?
4. Is your market currently a buyer's market, balanced market, or seller's market?
5. Typical inspection contingency period in your market (number of days)?
6. Typical financing contingency period in your market (number of days)?
7. Standard earnest money in your market (as % of purchase price or flat amount)?

Save preferences:

```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
  content: |
    # Real Estate Agent Pro — Preferences
    Agent: [name]
    Brokerage: [brokerage]
    Market: [market]
    Market Condition: [buyer's / balanced / seller's]
    Standard Inspection Period: [X] days
    Standard Financing Contingency: [X] days
    Standard Earnest Money: [X% or $X]
    Updated: [date]
```

Show a preferences banner at the top of every output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 1: SELECT MODE

**Ask the agent:**

"Are you writing an offer for a buyer, or analyzing offers as the listing agent?"

- **A — Buyer's Agent:** Go to Mode 1
- **B — Listing Agent:** Go to Mode 2

---

---

# MODE 1: BUYER'S AGENT — WRITE A COMPETITIVE OFFER

---

## 1A. Offer Intelligence Gathering

Collect everything needed before making any recommendations. Do not skip fields — every piece of information changes the strategy.

1. Property address and list price?
2. Buyer's target offer price? Buyer's absolute maximum price?
3. Buyer's financing type? (Conventional / FHA / VA / Cash / Other — specify lender if known)
4. Pre-approval status? (Fully pre-approved with letter / pre-approval in process / not yet started)
5. Has the buyer visited the property? Any red flags observed?
6. Is there an offer deadline? If yes, what date/time?
7. How many other offers are expected? (Any intel from the listing agent?)
8. Any known seller preferences or hot buttons? (Close date, leaseback, AS-IS, specific inclusions, sensitive about inspection period?)
9. How flexible is the buyer on contingencies? (Walk through each: financing, inspection, appraisal)
10. Does the buyer need to sell a home first? (If yes: do NOT waive financing contingency, strategy shifts significantly)

---

## 1B. Offer Strategy Recommendation

Based on market conditions from preferences + intel gathered above, generate a complete offer strategy with clear reasoning for each recommendation.

---

### Price Recommendation

*(Calibrate based on market condition from preferences and competition level.)*

**Seller's market / multiple offers expected:**
- Target price: [X]% above list. Reasoning: [based on recent comps, offer competition, urgency]
- Consider escalation clause (see below).

**Balanced market:**
- Target price: list price to [X]% above depending on days on market and comp support.
- Escalation generally not needed unless agent has intel about multiple offers.

**Buyer's market:**
- Target price: list price to [X]% below list depending on days on market and seller motivation.
- Focus on terms and timeline rather than price.

---

### Escalation Clause (if competitive situation)

*(Recommend when: multiple offers are expected or confirmed. Explain to buyer before using.)*

**How it works:** The offer starts at $[starting bid] and automatically escalates above any competing offer by $[increment] up to a maximum of $[cap]. The seller must provide proof of the competing offer.

**Recommended structure:**
- Starting bid: $[buyer's honest opening price — not artificially low]
- Increment: $[X] above any competing bona fide offer (typically $1,000–$5,000 depending on price range)
- Cap: $[buyer's absolute maximum — only use cap the buyer is genuinely willing to pay]

**Agent note:** An escalation clause shows the buyer's cards. In some situations (limited competition, savvy listing agent), it's smarter to lead with your strongest price without an escalation. Use judgment based on the specific situation.

**Risk to explain to buyer:** If the seller has another offer at or above your cap, the escalation clause doesn't help — and it reveals your ceiling. Confirm the buyer understands this.

---

### Earnest Money Recommendation

**Standard market:** [Standard EMD from preferences — X% or $X]
**Competitive market:** Consider 2–3x standard EMD to signal commitment.
**Cash offer:** Larger EMD is expected and adds credibility.

*Explanation for buyer: "EMD is not extra money — it comes off your down payment at closing. Higher EMD just shows the seller you're serious and have skin in the game."*

---

### Financing Contingency

*(Use standard period from preferences as the baseline.)*

| Market Condition | Recommendation | Risk Level |
|---|---|---|
| Buyer's market | Keep — standard period | Low risk |
| Balanced market | Keep — consider shortening by 3–5 days | Low-Medium |
| Seller's market (conventional/well-qualified) | Consider shortening to [X] days | Medium |
| Seller's market (cash offer) | Waive — not applicable | — |
| FHA/VA | Keep — cannot waive program contingencies | Required |

**If recommending waiver or shortening:** Explain the risk clearly: "If your financing falls through after waiving this contingency, you could lose your earnest money. Only recommend this for buyers with exceptional loan profiles and lenders you trust to close fast."

---

### Inspection Contingency

*(Each option has trade-offs — present them to the buyer, don't assume.)*

| Option | Description | Best For |
|---|---|---|
| **Full inspection contingency** | Standard period. Buyer can request repairs or walk away. | Buyer's market; first-time buyers; older properties |
| **Shortened inspection** | Reduce to [X] days (vs. standard [X]). Still full rights. | Competitive market; shows seller faster close |
| **Information-only inspection** | Buyer gets inspection results but cannot request repairs — only walk away for major findings. | Competitive situations; experienced buyers |
| **Major systems only** | Buyer waives minor repairs, retains right to walk for structural, roof, plumbing, electrical, HVAC. | Very competitive; buyer wants certainty but not blind |
| **Waive inspection** | No inspection contingency. Buyer takes AS-IS. | Cash buyers; investors; extreme competition only. Risky. |

**If recommending waiver or limitation:** "Make sure the buyer understands they're accepting the property in its current condition. I'd strongly encourage at least a pre-inspection before submitting if waiving inspection rights."

---

### Appraisal Contingency

| Option | Description |
|---|---|
| **Keep appraisal contingency** | If home appraises below purchase price, buyer can renegotiate or exit. Standard protection. |
| **Appraisal gap coverage** | Buyer commits to cover a gap between appraised value and purchase price up to $[X]. Example: "Buyer will pay up to $10,000 above appraised value." |
| **Waive appraisal** | Buyer agrees to pay purchase price regardless of appraisal. Risk: buyer owes the gap out of pocket or deal dies. Only for cash buyers or well-capitalized conventional buyers. |

**When to recommend gap coverage:** In seller's markets where offers frequently exceed appraised value. It's stronger than a full waiver because it's honest about risk — and sellers often respect a buyer who shows they've thought it through.

**Explaining gap coverage to buyer:** "If the home appraises at $420k and you offer $435k with a $15k gap guarantee, you're committing to bring an extra $15k to closing out of pocket beyond your normal down payment. Make sure you have that cash available."

---

### Close Date

- **If seller mentioned preferred timeline:** Match it exactly. This is often more important to sellers than $5,000 in price.
- **If no preference known:** Target [30–35 days] for conventional financing; [21–25 days] for cash.
- **Leaseback:** If seller needs time after close to move, offer [14–30 day] free leaseback. This is a powerful differentiator — it costs the buyer almost nothing (seller pays rent or buyer waives it as a goodwill gesture) and can win the listing.

---

### Personal Property and Inclusions

- Confirm what is included in the MLS listing (appliances, fixtures, etc.)
- Ask buyer if there are any specific inclusions to request (washer/dryer, refrigerator, outdoor furniture, hot tub)
- Caution: in a competitive situation, personal property requests can feel like nickel-and-diming. Consider keeping the offer clean and discussing inclusions separately.

---

## 1C. Buyer Cover Letter (Personalized)

Generate only if the agent wants one. **Preface with the fair housing note below.**

> **Fair Housing Note:** Some listing agents request no cover letters due to fair housing concerns — buyer letters can inadvertently reveal protected class characteristics (religion, family status, national origin) that could influence seller decisions. Ask the listing agent if they will present a letter before writing one. This skill will generate the letter — you decide whether to use it.

---

**Cover Letter Template**

*(Agent provides: who the buyers are, 2–3 specific reasons they love this home, any connection to the neighborhood)*

---

Dear [Seller Name(s)],

We wanted to take a moment to introduce ourselves — we are [buyer description: e.g., "a young family looking for our forever home," "a couple relocating from [city] for work," "a first-time buyer who has been dreaming of this neighborhood"].

From the moment we walked through [address], we knew it was different. [Specific reason 1 — personalized to the home: e.g., "The kitchen was exactly what we've been looking for — we can already picture Sunday mornings there."] [Specific reason 2 — e.g., "The backyard is something we've searched a long time to find."] [Specific reason 3 if applicable — e.g., "We love [specific neighborhood feature] and the [school/park/community] is exactly the kind of place we want our kids to grow up."]

We have been pre-approved with [lender name] and are financially prepared to close on schedule. Our agent, [Agent Name], has kept us well-informed throughout this process and will make the transaction as smooth as possible.

We understand you may have multiple offers, and we've done our best to put forward our strongest offer. We hope you'll give us the opportunity to make [address] our home.

Sincerely,
[Buyer Name(s)]

---

## 1D. Offer Summary Sheet

A one-page summary for the agent to review before submission — and for the buyer to understand exactly what they're signing.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OFFER SUMMARY — [Property Address]
Prepared by: [Agent Name] | [Brokerage]
Date: [Date]    Expiration: [Offer deadline]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

BUYERS:                [Names]
OFFER PRICE:          $[X]
ESCALATION:           [Yes — up to $[cap] in $[increment] increments / No]
FINANCING TYPE:       [Conventional / FHA / VA / Cash]
DOWN PAYMENT:         $[X] ([X]%)
LENDER:               [Name]
PRE-APPROVAL STATUS:  [Fully approved / In process]

EARNEST MONEY:        $[X] ([X]% of purchase price)

CONTINGENCIES:
  Financing:          [Yes — [X] days / Shortened to [X] days / Waived]
  Inspection:         [Full [X] days / Info-only / Major systems / Waived]
  Appraisal:          [Yes / Gap coverage up to $[X] / Waived]
  Home Sale:          [Yes / No]

CLOSE DATE:           [Date / ~[X] days from acceptance]
POSSESSION:           [At close / Leaseback [X] days]
INCLUSIONS:           [List or "per MLS"]
EXCLUSIONS:           [List or "none"]

RISK ASSESSMENT:
  For the buyer:      [Low / Medium / High] — [1-sentence reason]

STRATEGY NOTE:        [1–2 sentences on the overall approach and why]

CONFIDENCE RATING:    [Low / Medium / High] — [based on price vs. comps,
                      competition level, buyer strength]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠ Review all terms with your client before submission.
Contract requirements vary by state.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

---

# MODE 2: LISTING AGENT — ANALYZE MULTIPLE OFFERS

---

## 2A. Offer Input Collection

For each offer received, collect the following. Ask the agent to go through each offer one at a time.

**For each offer:**

1. Buyer name and buyer's agent name/brokerage?
2. Offer price?
3. Financing type? (Cash / Conventional / FHA / VA / Other)
4. Down payment amount and percentage?
5. Lender name? (Known local/reputable lender vs. unknown vs. online lender)
6. Pre-approval status? (Full approval / Pre-qual only / No letter)
7. Earnest money amount?
8. Financing contingency? (Yes/No — if yes, how many days?)
9. Inspection contingency? (Yes/No — if yes: full rights / info-only / limited scope / waived)
10. Appraisal contingency? (Yes/No — if yes: standard / gap coverage up to $[X] / waived)
11. Proposed close date?
12. Leaseback requested? (If yes: how many days, at what rate?)
13. Any escalation clause? (If yes: starting bid, increment, cap)
14. Any unusual terms or special conditions?

*(Collect all offers before generating the comparison.)*

---

## 2B. Side-by-Side Offer Comparison Table

| | **Offer 1** | **Offer 2** | **Offer 3** | **Offer 4** |
|---|---|---|---|---|
| **Buyer / Agent** | | | | |
| **Offer Price** | | | | |
| **Escalation Cap** | | | | |
| **Effective Max Price** | | | | |
| Financing Type | | | | |
| Down Payment | | | | |
| Lender / Approval | | | | |
| Earnest Money | | | | |
| Financing Contingency | | | | |
| Inspection Contingency | | | | |
| Appraisal Contingency | | | | |
| Close Date | | | | |
| Leaseback | | | | |
| Special Terms | | | | |
| **Risk Score** | | | | |
| **Net to Seller (est.)** | | | | |
| **Overall Rank** | | | | |

*Note: "Effective Max Price" accounts for escalation clause caps. "Net to Seller" deducts any leaseback fees, concessions, or known differences in closing cost allocation.*

---

## 2C. Risk Scoring Per Offer

Score each offer **Low / Medium / High** risk. Generate a clear explanation for each.

**Risk factors and weights:**

| Factor | Lower Risk | Higher Risk |
|---|---|---|
| Financing type | Cash → Conventional high down → Conventional lower down → VA → FHA | FHA / low down / unusual programs |
| Down payment | 20%+ | Under 10% |
| Lender quality | Known local lender, strong relationships | Unknown online lender, first-time working with them |
| Pre-approval | Full underwriter approval | Pre-qualification letter only |
| Earnest money | High relative to purchase price | Minimal EMD |
| Financing contingency | Waived (with strong buyer) | Extended contingency period |
| Inspection contingency | Waived or limited | Full rights, extended period |
| Appraisal contingency | Waived or gap coverage | Standard, no gap |
| Escalation clause | Clean escalation with proof requirement | Escalation without competing offer proof |
| Special conditions | None | Multiple contingencies, sale of home required |

**Risk Score:** Low = 0–2 risk factors. Medium = 3–4. High = 5+.

**For each offer, generate a plain-language summary:**

*Example: "Offer 2 — MEDIUM RISK. Strong price at $430k with no escalation needed. Conventional financing with 20% down from a lender I know. The risk is the full inspection contingency — buyer has 10 days and full rights. Given the home's condition, this is manageable. Leaseback request for 14 days is workable."*

---

## 2D. Seller Presentation Script

Script for presenting the offers to the seller — professionally, neutrally, and clearly.

---

**Opening:**
"Thank you for your patience — it's a good problem to have. We received [X] offers and I want to walk you through each one carefully so you can make the best decision for your situation. I'm going to present each offer factually first, then share my analysis and recommendation. There are no wrong questions."

---

**Presenting each offer (use the comparison table as the visual):**

"[Offer 1] comes from [Buyer/Agent]. The offer price is $[X] [with an escalation clause that can go up to $[cap]]. They're using [financing type] with [X]% down through [lender]. Earnest money is $[X]. [Inspection contingency: full/limited/waived]. [Appraisal: standard/gap/$X/waived]. They're proposing to close on [date] [and have requested a [X]-day leaseback / with no leaseback request]. My risk assessment is [Low/Medium/High]. Here's why..."

*(Repeat for each offer.)*

---

**Summarizing the choice:**

"Now that you've heard all [X] offers, let me summarize the key trade-offs.

The **highest net to you** is Offer [X] at $[amount]. The **lowest risk** (most likely to close) is Offer [X]. The **fastest close** is Offer [X].

In my professional opinion, I'd recommend [Offer X] for the following reasons: [2–3 specific reasons: price, financing strength, timeline, terms].

Before I share my recommendation more fully: what matters most to you — the highest price, the fastest close, or the smoothest transaction with the lowest chance of falling out?"

*(Let the seller answer — this often changes the recommendation.)*

---

**If two offers are very close:**

"These two offers are genuinely competitive. Here's how I'd differentiate them: [specific comparison of the one or two key differences]. If the price difference is within $[X], I'd take [the cleaner terms / the stronger buyer / the faster close] because [reason]."

---

## 2E. Counter-Offer Strategy

If none of the offers is ideal, or the agent wants to create urgency with the top offer(s):

---

**Which offer(s) to counter:**
Recommend countering [Offer X] because: [it's the strongest offer with one fixable issue] OR [multiple offers are close and a counter can push the winner higher].

**Suggested counter terms:**
- Price: counter at $[X] (if price is the gap)
- Contingency modification: request [inspection period shortened to X days / appraisal contingency waived / financing period shortened]
- Close date: adjust to [date] for seller's preferred timeline
- Leaseback: if seller needs it, request [X days] at $[X]/day (or free)

**Multiple counter strategy:**
If two offers are very competitive: issue identical counters to both, highest and best due by [date/time]. This creates a clean competitive process.

**Script for presenting the counter decision to seller:**

"We have a strong offer in Offer [X], but there's one piece we'd like to tighten up: [specific term]. I'd recommend we counter with $[X] / asking them to [modify term]. This is a reasonable ask that a motivated buyer will accept. If they push back, we can decide at that point whether to hold or find middle ground. Are you comfortable with me sending that counter?"

---

## SAVING OUTPUTS TO MEMORY

When the agent requests it, save offer analysis for the file:

```
mcp__cloud-brain__write_note:
  path: brain/projects/offers-[address-slug]-[YYYY-MM-DD].md
  content: |
    # Offer Analysis — [Address]
    Date: [date]
    Number of Offers: [X]
    
    Offer Summary:
    - Offer 1: $[price] | [financing] | Risk: [level]
    - Offer 2: $[price] | [financing] | Risk: [level]
    - Offer 3: $[price] | [financing] | Risk: [level]
    
    Recommended Offer: [#] — [brief reason]
    
    Seller Decision: [accepted / countered / multiple counters]
    Outcome: [accepted offer details]
    Notes: [any relevant context]
```
