---
name: market-report-generator
description: >-
  Generate a professional monthly or quarterly neighborhood market report for sphere nurture, farming campaigns, and past client touchpoints. Takes raw MLS stats and produces a polished, readable report with trend analysis, insights, and a clear call to action. Use monthly for farming, quarterly for sphere nurture, or any time a client asks 'what's the market doing?' Triggers on: market report, market update, neighborhood stats, what's the market doing, housing market update, local market report, sphere nurture, farming content, market newsletter.
---

# Market Report Generator

> **Note:** Market data should be sourced from your local MLS or a licensed data provider. Always verify statistics before publishing. This skill helps format and analyze data you provide — it does not independently access MLS data. Ensure all published market reports comply with your brokerage's advertising guidelines and include any required license disclosures.

You're generating a **professional neighborhood market report** — a high-value touchpoint that keeps you top of mind with your sphere, farm, and past clients while demonstrating your local expertise.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [Market]. Proceed or update?"

**If not found:** Run the setup interview below, then save.

> To personalize every market report, I need a few details:
> 1. Your full name?
> 2. Brokerage name?
> 3. Primary market / city?
> 4. Your phone and email (for report footer)?
> 5. Primary farm neighborhood or report area?
> 6. Do you send reports via email, print, or both?
> 7. How often do you publish — monthly or quarterly?

```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
  content: |
    # Real Estate Agent Pro — Preferences
    Agent: [name]
    Brokerage: [brokerage]
    Market: [market]
    Phone: [phone]
    Email: [email]
    Primary Farm Area: [neighborhood]
    Report Distribution: [email / print / both]
    Publish Frequency: [monthly / quarterly]
    Updated: [date]
```

Show preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 1: DATA COLLECTION

Ask the agent to provide the following stats for the report period (or flag which they still need to pull from MLS):

**Report Period:**
- Month/quarter and year (e.g., "May 2026" or "Q1 2026")
- Target neighborhood or area (confirm or override the farm area from preferences)

**This Period's Stats:**
- Homes sold this period: count
- Average sale price
- Median sale price
- Average days on market (DOM)
- List-to-sale price ratio (average %)
- Active listings currently on market
- Pending listings currently under contract
- New listings added this period
- Expired or withdrawn listings this period

**Prior Period Comparison Data:**
- Same stats from the previous month/quarter OR same period last year (specify which)
- If agent has both, ask which comparison they prefer to feature

**Tip for agent:** All of these stats are available in your MLS under the market statistics or market snapshot report for the selected area. Most MLS platforms can export a CSV or summary sheet — ask your MLS rep if you need help pulling them.

---

## STEP 2: TREND ANALYSIS

Calculate and interpret the following from the data provided:

**Price Trend:**
- Change from prior period: up/down $[X] (+/-[X]%)
- Flag: 🚨 if swing is greater than 10% — likely worth highlighting as a headline

**DOM Trend:**
- Faster or slower vs. prior period: [+/-X] days
- Interpretation: faster DOM = more competitive for buyers; slower = more negotiating power

**Inventory / Market Type:**
- Months of supply = active listings ÷ (homes sold this period ÷ number of months in period)
- Interpret:
  - Under 3 months → Strong Seller's Market
  - 3–4 months → Seller's Market
  - 4–6 months → Balanced Market
  - Over 6 months → Buyer's Market

**Absorption Rate:**
- Absorption rate = (homes sold ÷ homes available at start of period) × 100
- High absorption (>50%) = fast-moving market

**List-to-Sale Ratio Trend:**
- Above 100% = homes selling over asking price (competitive)
- At 100% = homes selling at asking
- Below 100% = buyers negotiating discounts

Summarize trends in one paragraph: plain English interpretation of what these numbers mean together. Example: "The [neighborhood] market tightened in [month] — homes sold faster, inventory dropped, and buyers paid closer to asking price. If you're thinking about selling, conditions are favorable right now."

---

## STEP 3: GENERATE THE REPORT

### Format A — Email / Digital Version

**Subject Line Options (3 variations for A/B testing):**
1. "[Month] Real Estate Update: What's Happening in [Neighborhood]"
2. "Home values in [Neighborhood] — [Month] market data"
3. "Your [Neighborhood] market snapshot — [Month]"

**Report Body Structure:**

---

**[HEADLINE STAT]**
Lead with the most interesting or surprising number. Pull the agent's attention first.
Example: "Homes in [Neighborhood] sold in [X] days on average last month — [X] days faster than a year ago."

**Market Overview** (2–3 sentences)
What does this data mean in plain English? State the market condition (buyer's, balanced, or seller's) and the single most important takeaway for a homeowner reading this.

**Key Stats Snapshot**

| Metric | [This Period] | [Prior Period] | Change |
|---|---|---|---|
| Homes Sold | [X] | [X] | [+/- X] |
| Avg Sale Price | $[X] | $[X] | [+/- X%] |
| Median Sale Price | $[X] | $[X] | [+/- X%] |
| Avg Days on Market | [X] | [X] | [+/- X days] |
| List-to-Sale Ratio | [X%] | [X%] | [+/- X%] |
| Active Listings | [X] | [X] | [+/- X] |
| Months of Supply | [X] | [X] | [+/- X] |

**What This Means for Buyers:**
1–2 sentences. Example: "Buyers should expect competition on well-priced homes and be prepared to move quickly. Getting pre-approved before you start looking is more important than ever."

**What This Means for Sellers:**
1–2 sentences. Example: "Well-priced homes in [Neighborhood] are attracting multiple offers and selling close to list price. Now is a strong time to list if you've been considering it."

**[Agent's Insight]**
One short paragraph written in first person — personal commentary on what the agent is seeing in showings, conversations, buyer feedback, or offer dynamics. This is what makes the report local and real, not just numbers. If the agent doesn't have specific insight to add, prompt them:
> What have you noticed in the last 30 days that these numbers reflect? Any stories from showings, conversations, or offers that would resonate with homeowners?

Example: "Personally, I've been seeing a lot of buyers from [area] entering our market — they're motivated and well-qualified. That's been driving competition on anything priced under $[X]. If your home falls in that range, you have more leverage than you might think right now."

**Call to Action:**
"Thinking about selling? I can show you specifically what buyers are paying for homes like yours right now — not just averages, but the actual comps on your block. No obligation, no pressure — just data.
[Phone] | [Email]"

**Footer:**
[Agent Name] | [Brokerage] | [Phone] | [Email] | License #[placeholder]
*Data sourced from [MLS name]. Statistics represent [Neighborhood/area] for [period]. Information deemed reliable but not guaranteed.*

---

### Format B — Print / Postcard Version

Condensed to fit on a 6×9 or 8.5×11 postcard. Strip everything to maximum impact:

**[BOLD HEADLINE STAT]**
Example: "Homes in [Neighborhood] are selling in [X] days."

**[Month/Quarter] Snapshot — [Neighborhood]**

| Homes Sold | Avg Price | Avg DOM | List-to-Sale |
|---|---|---|---|
| [X] | $[X] | [X] days | [X%] |

**[2-sentence market insight]**
Example: "[Month] was a strong seller's market in [Neighborhood]. Buyers paid an average of [X%] of asking price and inventory remained tight at [X] months of supply."

**[Photo placeholder: Agent headshot]**
[Agent Name] | [Brokerage] | [Phone]

**Call to Action:**
"What's YOUR home worth? Call for a free market analysis."

---

## STEP 4: DISTRIBUTION PLAN

Based on the agent's preferences from Step 0:

**Email Distribution:**
- Recommended subject line: [offer the winning option from the 3 above based on the data — use the most compelling stat as the hook]
- Best send time: Tuesday–Thursday, 9–11am local time
- Audience reminder: past clients, sphere, farm list, anyone who has asked about the market in the last 12 months

**Print Distribution:**
- Print data should be no more than 2–3 weeks old at delivery — plan print timeline accordingly
- Recommended for: farm area door drops, open house handouts, listing appointments

**Social Media Caption (Instagram / Facebook):**
Generate a short-form social post version:
> "[Month] [Neighborhood] Market Update [chart emoji]
> [Headline stat]
> [1-sentence market context]
> Want to know what your home is worth right now? Drop a comment or DM me — I'll pull the specific data for your street.
> [Agent Name] | [Brokerage] | [phone]"

---

## STEP 5: SAVE TO MEMORY

```
mcp__cloud-brain__write_note:
  path: brain/projects/market-report-[area-slug]-[YYYY-MM].md
  content: |
    # Market Report — [Neighborhood] — [Month/Quarter YYYY]
    Generated: [date]
    Period: [report period]
    
    ## Key Stats
    Homes Sold: [X]
    Avg Price: $[X]
    Median Price: $[X]
    Avg DOM: [X]
    List-to-Sale: [X%]
    Active Listings: [X]
    Months of Supply: [X]
    Market Condition: [Buyer's / Balanced / Seller's]
    
    ## Trend Summary
    [price trend sentence]
    [DOM trend sentence]
    [market condition interpretation]
    
    ## Distribution
    Formats generated: [email / print / social]
    Subject line used: [selected subject line]
```

Confirm to agent: "Report saved to memory. To generate next month's report, I'll reference this one for prior period comparison — you'll only need to enter the new stats."
