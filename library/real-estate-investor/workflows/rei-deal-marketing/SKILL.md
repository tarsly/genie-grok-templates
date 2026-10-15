---
name: rei-deal-marketing
description: >-
  Create deal marketing materials — flyers, one-pagers, listing descriptions, investor pitches, email blasts, social media posts, and deal summaries for real estate deals (wholesale, flip, creative finance), or wants to create a flyer, pitch deck, or email blast for a specific deal.
---

# REI Deal Marketing Package

## Overview

Generates a complete set of marketing materials for any real estate deal. Reads investor contact info, brand voice, and deal preferences from saved preferences so materials are personalized on the first run. Reads prior deal analysis from cloud brain when available, so the one-pager numbers are pulled from a verified analysis rather than re-entered manually.

**Output includes:**
1. Professional one-page deal summary
2. Social media posts (Instagram, Facebook, X/Twitter)
3. Instagram story / Reel script
4. Email blast template
5. Investor narrative pitch
6. Listing descriptions (short + long)

## When This Skill Applies

- User has a deal and needs marketing materials created
- User asks for a "one-pager" or "deal summary" for a property
- User wants social media posts for a deal
- User needs an email blast for a buyer list
- User wants an investor pitch or presentation
- User asks for listing copy or property description
- User says "market this deal" or "blast this out"
- User needs wholesale, flip, or creative finance deal marketing

---

## Pre-Flight — Investor Preferences

1. Use `mcp__cloud-brain__search_notes` with query `REI preferences`
2. **If found:** Load investor name, company, phone, email, website, and brand voice preference. Display ⚙️ banner. Confirm: "Using your saved contact info — [Name] | [Phone] | [Website]. Still correct?"
3. **If not found:** Ask in ONE message:
   > "Let me save your marketing profile so materials are ready to use without edits. Tell me:
   > - Your name and company name
   > - Phone, email, and website
   > - Brand voice: Professional, Conversational, or Bold/Direct
   > - Any tagline or positioning statement you want included?"

   Save to `brain/preferences/rei-preferences.md`. Proceed.

4. **Check for prior deal analysis:** Use `mcp__cloud-brain__search_notes` with the property address slug. If a deal analysis exists, pull the numbers (cash flow, CoC, cap rate, terms) automatically into the one-pager and social posts. Note: "Found saved deal analysis — pulling verified numbers."

---

## How It Works

### Step 0: Gather Deal Information

| Input | Required | Example |
|-------|----------|---------|
| Property address | Yes | 4521 Elm St, Dallas TX |
| Property type | Yes | SFR, duplex, condo, land |
| Beds / baths | Yes | 3/2 |
| Square footage | Helpful | 1,450 sqft |
| Lot size / year built | Helpful | 0.18 ac / 1985 |
| Purchase / asking price | Yes | $165,000 |
| Estimated ARV or market value | Helpful | $210,000 |
| Estimated rents | Helpful | $1,650/mo |
| Deal type | Yes | Wholesale, flip, buy-and-hold, seller finance, subject-to |
| Rehab estimate | If applicable | $25,000 |
| Creative finance terms | If applicable | $0 down, 5% interest, 30yr amort, 5yr balloon |
| Target audience | Helpful | Cash buyers, investors, owner-occupants, landlords |
| Unique selling points | Helpful | New roof, large backyard, Section 8 ready |

If key details are missing, ask. Don't guess on price, address, or deal terms.

### Step 1: One-Page Deal Summary

```
INVESTMENT OPPORTUNITY
[Property Address]
[City, State ZIP]

━━━━━━━━━━━━━━━━━━━━━━━━━

PROPERTY DETAILS
Type:           [SFR / Duplex / etc.]
Beds/Baths:     [X/X]
SqFt:           [X,XXX]
Lot Size:       [X.XX acres]
Year Built:     [XXXX]
Condition:      [Turnkey / Light Rehab / Full Gut]

━━━━━━━━━━━━━━━━━━━━━━━━━

DEAL TERMS
Asking Price:        $XXX,XXX
ARV / Market Value:  $XXX,XXX
Estimated Rehab:     $XX,XXX
Estimated Rent:      $X,XXX/mo

[If creative finance:]
Financing Available:  [Seller Finance / Subject-To / Wrap]
Down Payment:        $XX,XXX
Interest Rate:       X.XX%
Monthly Payment:     $X,XXX
Term:               XX years / XX-month balloon

━━━━━━━━━━━━━━━━━━━━━━━━━

THE NUMBERS
Monthly Rent:           $X,XXX
Monthly Payment:        $X,XXX
Est. Monthly Expenses:  $XXX
Monthly Cash Flow:      $XXX
Cash-on-Cash Return:    XX.X%
Cap Rate:              X.X%

━━━━━━━━━━━━━━━━━━━━━━━━━

WHY THIS DEAL
• [Strongest selling point]
• [Financial advantage]
• [Location / market advantage]
• [Unique feature]

━━━━━━━━━━━━━━━━━━━━━━━━━

CONTACT
[Investor Name — from preferences]
[Phone — from preferences]
[Email — from preferences]
[Website — from preferences]
```

If numbers came from a saved deal analysis, add a note: *"Numbers verified via deal analysis — {date}."*

### Step 2: Social Media Posts

**Instagram (Feed):**
```
[HOOK LINE — specific to deal type]

[Property type] | [City, State]
[Beds/Baths] | [SqFt] sqft | Built [Year]

[Deal-type pitch — 2–3 sentences]

[Key numbers — price, rent, cash flow, returns]

[CTA]

[10–15 relevant hashtags]
```

**Hook lines by deal type:**
- Wholesale: "DEAL ALERT: Under-market property just hit my desk."
- Cash flow: "Cash flow from day one. Here's the breakdown."
- Seller finance: "No bank needed. Seller financing available."
- Subject-To: "Take over payments. No qualifying."
- Flip: "Built-in equity. $XX,XXX spread on this one."
- BRRRR: "Buy, rehab, rent, refi, repeat. This one checks every box."

**Instagram Story / Reel Script:**
```
Opening (3 sec): "I just locked up a deal in [City] that you need to see."
The deal (10 sec): "[Property type], [beds/baths], asking [price]. Rents for [rent]. That's [cash flow] a month cash flow."
The punch (5 sec): "[Unique selling point or creative finance hook]"
CTA (3 sec): "DM me 'DEAL' for the full breakdown."
```

**Facebook (Groups-friendly, longer format):**
```
[HOOK — question or bold statement]

I've got a [property type] in [City, State] that [primary selling point].

Quick breakdown:
[Address]
[Beds/Baths] | [SqFt] sqft
Asking: $XXX,XXX
[Rent / ARV / Cash flow details]

[2–3 sentences on why this is a good deal — speak to the target buyer]

[Deal type specifics — financing terms, assignment fee, etc.]

[CTA — DM, comment, call]

#RealEstate #[DealType] #[City] #Investing
```

**X / Twitter (Concise, numbers-forward):**
```
[Deal type] alert — [City, State]

[Property type] | [Beds/Baths] | $XXX,XXX
Rent: $X,XXX/mo | Cash flow: $XXX/mo
[Key metric: XX% CoC / $XXK equity / creative terms]

[One-line hook]

DM for details.

#RealEstate #[DealType] #[City]
```

### Step 3: Email Blast

```
Subject: [DEAL ALERT] [Property Type] in [City, State] — [Key Hook]

Hi [First Name / Investor],

New deal on my desk — wanted to get it to you first.

[Property Address]
[City, State ZIP]

Property Details:
• Type: [SFR / Duplex / etc.]
• Beds/Baths: [X/X] | SqFt: [X,XXX] | Year Built: [XXXX]
• Condition: [Turnkey / Needs Work]

Deal Terms:
• Price: $XXX,XXX
• [ARV: $XXX,XXX / Rent: $X,XXX/mo]
• [Creative finance terms if applicable]

Why This Deal:
• [Strongest point]
• [Financial advantage]
• [Unique feature]

[Deal-type financials]:
- Wholesale: "Assignment fee: $XX,XXX. Proof of funds required."
- Seller finance: "Terms: $XX,XXX down, X.XX%, XX-year amortization."
- Rental: "Cash-on-cash: XX.X%. Cap rate: X.X%. Cash flow positive from day 1."

Reply here or call/text [phone from preferences] if you're interested. This one won't last.

[Investor Name — from preferences]
[Phone]
[Email]
[Website]
```

### Step 4: Investor Narrative Pitch

```markdown
## Investment Opportunity: [Address]

### The Opportunity
[2–3 paragraphs: why does this deal exist? Why is it undervalued or well-positioned?
What is the strategy and exit? Written as if explaining face-to-face to an investor.]

### Market Context
[1–2 paragraphs on why this area or market supports the strategy.
Pull from web search if available — population growth, job market, rent growth.]

### The Numbers
[Clean financial summary — total investment, projected returns, timeline, exit strategy]

### Risk Mitigation
- What if rents don't hit projections? (conservative underwriting used)
- What if vacancy is higher? (break-even analysis)
- What if market dips? (bought below market, equity cushion)
- What if repairs cost more? (contingency built in)

### Exit Strategies
1. Primary: [e.g., hold for cash flow, refinance, flip]
2. Backup: [e.g., sell at market, assign, lease-option]
3. Worst case: [floor scenario description]

### The Ask (if raising capital)
- Capital needed: $XX,XXX
- Timeline: XX months
- Projected return: XX%
- Structure: [JV split, preferred return, equity share]
```

### Step 5: Listing Descriptions

**Short (social media / quick posts — ~150 words):**
```
[Compelling opening line capturing the property's best feature]
[Key details: beds/baths, sqft, lot, year built]
[Top 3–4 features in flowing prose — not a list]
[Location highlights: proximity to schools, transit, employers]
[CTA with urgency]
```

**Long (MLS / listing sites / deal packages — 400+ words):**
```
[Evocative opening paragraph — paint a picture, create desire]
[Detailed property description — room-by-room flow, upgrades, materials, finishes]
[Outdoor space, lot, garage, parking]
[Neighborhood and location — walkability, amenities, commute, school district]
[Investment angle — rental income, equity, development potential]
[Closing paragraph with urgency and CTA]
```

### Step 6: Save to Cloud Brain

```
Path: brain/deal-marketing/{address-slug}-{YYYY-MM-DD}.md
Tool: mcp__cloud-brain__write_note
```

If numbers were tight (negative cash flow or low returns), adjust the angle toward equity, appreciation, or creative finance benefits — and flag to user: "Cash flow numbers are tight. I've angled the marketing toward [alternative selling point] instead. Review before distributing."

---

## Error Handling

- **No property address:** Ask before proceeding.
- **No asking price:** Ask — can't build the numbers section without it.
- **No deal type:** Ask — determines the entire positioning.
- **No investor contact info:** Load from preferences or ask.
- **Incomplete property details:** Proceed with available info; note placeholders clearly.
- **Marketing package already exists for address:** Ask to overwrite or create v2.
- **Deal numbers unattractive:** Still create materials; adjust angle to equity, location, or creative terms. Flag to user.
- **User asks for video script or podcast talking points:** Adapt investor pitch narrative to requested format.
