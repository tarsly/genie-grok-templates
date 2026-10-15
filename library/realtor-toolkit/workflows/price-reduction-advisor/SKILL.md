---
name: price-reduction-advisor
description: >-
  Help a top real estate listing agent decide exactly when and how much to reduce a listing price — then prepare a complete, data-backed seller conversation package including a reduction script, talking points, objection handlers, and a visual one-pager. Use this skill whenever a listing is stale, DOM is too high, there are no offers, showing activity has dropped, or you need to have a price conversation with a seller. Triggers on: price reduction, price drop, cut the price, listing not selling, DOM too high, no offers, stale listing, reduce price, seller conversation about price, pricing strategy, overpriced listing.
---

> **Disclaimer:** Price reduction recommendations are professional guidance based on the market data and showing feedback you provide. They are not a guarantee of outcome and do not constitute appraisal or investment advice. Real estate markets are dynamic — always apply your own market knowledge and consult with your broker before advising clients on pricing decisions.

# Price Reduction Advisor

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [Market]. Proceed or update?"

**If not found:** Run the setup interview below, then save:

```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
  content: |
    # Real Estate Agent Pro — Preferences
    Agent: [name]
    Brokerage: [brokerage]
    Market: [market]
    DOM Benchmark: [days before price review warranted]
    Showing-to-Offer Ratio: [e.g., "10 showings, no offer = overpriced"]
    Updated: [date]
```

**Setup interview questions:**
1. Your full name?
2. Brokerage name?
3. Primary market / city?
4. Average DOM in your market before a price review is typically warranted? (Common: 21 days)
5. Typical showing-to-offer ratio in your market (e.g., "10 showings, still no offer = overpriced")?

Show a preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

Help your sellers make the RIGHT decision on price with data, not emotion. This skill walks you through analysis in minutes and gives you everything you need for a confident, empathetic seller conversation.

## STEP 1: Gather the Situation

Ask for (or parse from what you tell me):

**Core Numbers:**
- Current list price
- Days on market (DOM)
- Total showings to date
- Showings per week (recent trend)
- Offers received (count and status)

**Market Data:**
- Recent comparable sales in the area (3-5 sales, last 30-60 days)
- Original list price (if it's been reduced before)
- Seller's timeline / financial motivation

**Feedback Themes:**
- What are showing agents hearing? ("Too high for the neighborhood," "Beautiful but priced $30k over," etc.)
- Any specific feedback about condition, location, or comparables?

You don't need all of this. Give me what you have, and I'll ask for what matters.

## STEP 2: Run the Decision Engine

I'll analyze using these five benchmarks:

### 1. Days On Market (DOM) Signal
- **Benchmark:** [Use market benchmark from preferences — default 21 days] with no offers + showing activity dropping = pricing problem
- **Why it matters:** The market speaks fast. If serious buyers have seen it and passed, they're telling you the price.
- **Action:** If DOM > [benchmark] with <3 showings/week, price review is urgent.

### 2. Showing-to-Offer Ratio
- **Benchmark:** [Use market ratio from preferences — default: 10+ showings, zero offers = likely 5-10% overpriced]
- **Why it matters:** Plenty of interest but no offers means buyers like it but won't bite at this price.
- **Action:** This is the clearest signal. 15+ showings, no offers? Price cut is coming.

### 3. Price Band Analysis
- **Benchmark:** Is it sitting just above a psychological search threshold?
- **Examples:** $505k when most searches stop at $500k; $2.95M when buyers filter at $2.9M
- **Why it matters:** Buyers anchor to round numbers. Being $15k above a threshold can cost you 30% of your traffic.
- **Action:** I'll identify if you're just above a band and recommend crossing below it.

### 4. Comp Drift
- **Benchmark:** Have recent closed sales come in 3-5% lower than when you priced this?
- **Why it matters:** Market moves fast. Last month's comp-based price may already be yesterday's number.
- **Action:** If 3+ recent sales are lower, your price already aged.

### 5. Absorption Rate
- **Benchmark:** How many competing homes went under contract this week in your neighborhood?
- **Why it matters:** If 5+ homes sold and yours sits, you're being rejected by a hot market.
- **Action:** In an active market, being on the market long is a major red flag.

## STEP 3: Give You a Recommendation

I will provide:

1. **Specific price reduction** (dollar amount AND percentage)
   - Example: "Reduce from $575k to $549k (-4.5%)"

2. **Plain English reasoning**
   - "You've had 14 showings in 2 weeks with zero offers. Feedback says 'priced $20-25k high.' Recent comps average $550k. The market is saying $549-552k."

3. **New price band**
   - "This lands you in the $549k band, below the $550k search threshold, which could add 20-30% showing traffic."

4. **Expected impact projection**
   - "At $549k, you should see showing activity jump to 4-5 per week. Anticipate an offer within 7-10 days."

## STEP 4: Generate the Seller Conversation Package

### A. Price Reduction Script
Data-backed, empathetic, NOT defensive. Auto-signed with agent name from preferences.

I'll give you exact language that:
- Acknowledges their emotional investment
- Explains what the market is actually saying (feedback + numbers)
- Positions the reduction as a STRATEGY, not a failure
- Moves to action without over-explaining

**Example tone:** "Here's what I'm seeing: We've had 14 qualified buyers through. They love the home. But feedback is consistent — they're seeing it as $20-25k above where it needs to be. That tells me we have a pricing opportunity, not a property problem."

### B. Key Talking Points
- What the showing agents are saying (the market's voice)
- Recent comp data (not your opinion, the data)
- DOM comparison to neighborhood average
- Showing-to-offer ratio (hard signal)
- Impact of crossing a price band threshold
- Timeline: How quickly you expect to see movement

### C. Objection Handlers
Pre-loaded responses for the five things sellers always say:

1. **"But Zillow says it's worth $600k"**
   - Zillow is backward-looking. Your recent comps are here-and-now. We use actual closed sales, not Zestimate.

2. **"I need to get $575k to break even on my mortgage"**
   - I understand. Let's run the math: What's your timeline? How much longer can you carry the home? [Calculate carrying costs vs. price reduction]. Usually the faster sale saves money.

3. **"Let's just wait. The market will come to us."**
   - We could wait, but here's the risk: Every week, this home ages. Showing traffic drops. We lose momentum. We're fighting that now at $575k. [Show DOM trend line.]

4. **"I'll reduce $5k and see what happens"**
   - A $5k cut on a $575k home is .8%. That's not enough to reset buyer perception or cross a price band. I recommend hitting $549k to get real traction. Here's why: [Show feedback summary + comp clustering.]

5. **"I want to price it high and take what I can get."**
   - That strategy works in seller's markets, but we're in [market condition]. Right now, price discipline = faster sales + fewer days on market + stronger offers. High price + long DOM signals distress.

### D. One-Page Visual Summary
A clean, text-based visual the seller can take home. Includes:

**Price Reduction Summary**
- Current: $575k | DOM: 32 | Showings: 14 | Offers: 0
- Recommended: $549k (-$26k, -4.5%)
- Comp range: $548–555k
- Benchmark: 21 DOM before price review / Your market: 32 DOM

**Why This Price Works**
- ✓ Aligns with recent sales
- ✓ Crosses below $550k threshold (adds search visibility)
- ✓ Matches showing feedback ($20–25k high)
- ✓ Competes with 3 new actives in your price band

**What to Expect**
- Showing activity: 4–5 per week (up from 2–3)
- Timeline to offer: 7–10 days
- Offer quality: Full-price or near-full offers likely

---

## How to Use This Skill

**Tell me what you've got:**
- "My listing in Hillside is at $575k, 32 days, 14 showings, no offers. Feedback says it's overpriced. Comp sales from last month are averaging $550k."

OR

- "Price check on 42 Oak Street. It's been sitting 3 weeks, showing twice last week, very few calls. What should I do?"

**I'll immediately:**
1. Run the five benchmarks
2. Give you a specific price reduction + reasoning
3. Generate the full seller conversation package (script, talking points, objections, visual)
4. You'll have everything in under 5 minutes

**Use the output to:**
- Walk into the seller conversation fully prepared
- Show the seller the data (not your gut feeling)
- Handle pushback with tested language
- Move the listing fast with confidence

---

## Why This Works

Top agents know: **The first price is the most important decision.** But when the first price was wrong, the second decision is speed. Every day a home sits unsold, it ages in the market. Buyer perception shifts. Competition increases.

This skill turns a tough seller conversation into a data-backed strategy session. You're not asking them to accept a loss. You're showing them the path to the fastest sale at the best price the market will bear right now.

**Urgency:** Use this skill the moment DOM exceeds your market's benchmark or showing activity stalls. Don't wait for the 4th or 5th week of no offers.
