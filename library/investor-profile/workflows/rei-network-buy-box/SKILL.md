---
name: rei-network-buy-box
description: >-
  Manage a network of real estate investors' buy boxes. Use to save new buy boxes from text, spreadsheets, or vCards, update existing criteria, match a specific deal against the database to find potential buyers, generate a ranked buyer list, and draft professional outreach messages. Use when user says 'save this buy box', 'who in my network buys in Dallas', 'find buyers for this deal', 'add buyer criteria', or 'draft outreach for matching buyers'.
---

# Network Buy Box Manager

## Overview

Maintains a searchable database of real estate investors' buy boxes — specific acquisition criteria for each person in your network. Saves buyer profiles to cloud brain, matches incoming deals against the entire database, and drafts professional outreach to the best-fit buyers.

Built for wholesalers, deal finders, and investors who regularly need to move deals to the right buyer fast.

## When This Skill Applies

- User has a buyer's buy box to add or update
- User says "save this buyer's criteria" or "add to my buyer database"
- User wants to match a deal to potential buyers: "who in my network buys in Dallas?"
- User asks "find buyers for this deal" or "who should I call about this property?"
- User wants to draft outreach to matching buyers
- User asks "show me my buyer list" or "what does [Name] buy?"

---

## Pre-Flight — Identify Action

Determine what the user wants to do:

1. **Add/Update a buyer's buy box** → Section 1
2. **Match a deal to buyers** → Section 2
3. **Draft outreach messages** → Section 3
4. **View/search the buyer database** → Section 4

---

## Section 1: Saving or Updating a Buy Box

### Step 1: Extract the Data

Parse the provided text (pasted text, spreadsheet row, vCard, or conversational description) to extract:

| Field | Required | Notes |
|-------|----------|-------|
| Name | Yes | First and last |
| Company | Optional | |
| Email | Yes if available | Primary contact |
| Phone | Yes if available | |
| Geography | **Critical** | State, metro, radius (e.g., "50mi from Dallas"). If "nationwide," flag it. |
| Property Type | Yes | SFR, MF, commercial, land, mobile home park, etc. |
| Price Range | Yes | Min and max |
| Strategy | Yes | Flip, BRRRR, buy-and-hold, wholesale, etc. |
| Condition Tolerance | Yes | Turnkey, light rehab, heavy rehab, distressed |
| Acquisition Methods | Helpful | Cash, seller finance, Sub2, conventional, DSCR |
| Min Cash Flow / Cap Rate | Helpful | Investor's financial floor |
| Beds / Baths / Units | Helpful | Minimum or preferred |
| Deal Breakers | Helpful | Flood zones, HOA, etc. |
| Partnership Openness | Helpful | JV, co-wholesale, finder's fee |

**If key fields are missing** (especially Geography, Property Type, Price), flag them: *"I was able to save [Name]'s buy box, but I'm missing their [fields]. Worth following up to complete their profile."*

### Step 2: Handle Update Logic

- If user says "add to" or "augment" → **merge** new criteria with existing record for that contact
- If user says "replace" or "new buy box" → **overwrite** entirely
- If contact not found in brain → create new record

### Step 3: Save to Cloud Brain

```
Path: brain/buyer-network/[name-slug].md
Tool: mcp__cloud-brain__write_note
```

**File format:**
```markdown
# Buyer Profile: [Name]
**Company:** [Company]
**Email:** [Email] | **Phone:** [Phone]
**Last Updated:** [Date]

## Buy Box
- **Geography:** [Markets]
- **Property Type:** [Types]
- **Price Range:** $[Min]–$[Max]
- **Strategy:** [Strategies]
- **Condition:** [Tolerance]
- **Acquisition Methods:** [Methods]
- **Min Cash Flow:** $[Amount]/mo (if specified)
- **Min Cap Rate:** [X]% (if specified)
- **Deal Breakers:** [List]
- **Partnership Openness:** [Notes]
- **Other Notes:** [Anything else relevant]
```

Confirm to user: *"[Name]'s buy box saved. [X] buyers now in your network database."*

---

## Section 2: Matching a Deal

When user provides a property description and asks for matches:

### Step 1: Parse the Deal

Extract key attributes from the property description:

| Attribute | Value |
|-----------|-------|
| Address / Location | [City, State] |
| Property Type | [SFR/MF/Land/etc.] |
| Price | $[Amount] |
| Condition | [Turnkey/Rehab/Distressed] |
| Beds/Baths | [X/X] |
| Estimated ARV | $[Amount] |
| Estimated Rent | $[Amount]/mo |
| Strategy Fit | [Best fits: flip / BRRRR / hold / wholesale] |
| Creative Finance | [Available or not, terms] |

### Step 2: Search the Buyer Database

Use `mcp__cloud-brain__search_notes` with queries:
- Property location (state, city)
- Property type
- Price range terms

Load all buyer profiles and compare deal attributes against each buyer's criteria.

**Matching Logic:**

| Match Level | Criteria |
|------------|----------|
| 🟢 **Strong Match** | Geography ✅ + Property Type ✅ + Price ✅ + Condition ✅ |
| 🟡 **Partial Match** | Geography ✅ + 2 of 3 remaining criteria ✅ |
| 🔴 **Weak / No Match** | Geography ❌ or 2+ criteria mismatch |

**Geography rule:** Geography is paramount. A deal in Dallas, TX matches buyers looking in "Texas," "DFW Metro," or "Dallas." It does NOT match a buyer looking only in "Houston" or "California."

### Step 3: Rank and Present Results

```
DEAL MATCH REPORT
Property: [Address] | Price: $[Amount] | Type: [Type] | Condition: [Condition]

STRONG MATCHES (X buyers)
──────────────────────────────────
1. [Name] — [Company]
   📍 Buys in: [Markets]   💰 Range: $[Min]–$[Max]   🏠 [Types]
   ✅ Why matched: [2–3 sentence explanation of specific fit]
   📞 [Phone] | 📧 [Email]

2. [Name] — [Company]
   [same format]

PARTIAL MATCHES (X buyers)
──────────────────────────────────
[Same format; note what didn't fully match]

BUYERS TO ADD TO YOUR PIPELINE
──────────────────────────────────
[Suggestion to reach out to specific buyers not currently in DB if this deal type is underserved]
```

---

## Section 3: Drafting Outreach Messages

For the top matching buyers, generate professional outreach:

**Default Template:**
```
Hi [Contact Name],

I hope you're doing well. I came across a deal in [Location] that looks like a strong fit for your acquisition criteria — specifically your focus on [Property Type] in [their stated market] under [their price cap].

Quick summary:
• Address: [Address]
• Type: [Type] | [Beds/Baths] | [SqFt]
• Price: $[Amount]
• [Key metric: rent, ARV, condition, creative terms if applicable]

I'm not taking this one down myself, but I thought of you immediately and wanted to see if you'd be open to connecting on it. No pressure — happy to send over the full details if there's interest.

Best,
[User Name]
[User Phone]
[User Email]
```

**If creative finance is available:**
> Add: "The seller is open to [seller finance / Subject-To] which I know fits your acquisition style."

**If outreach is for a wholesale assignment:**
> Add: "I have it under contract at $[price]. Assignment fee is $[fee]. Let me know if you want the full deal package."

**Tone adjustments:**
- For well-known contacts: More casual, reference past dealings
- For cold/new contacts: Professional, brief, lead with the deal not the relationship
- For lenders in the database: Frame as "I have a borrower who needs [X]" not "I have a deal"

---

## Section 4: View / Search the Buyer Database

When user asks "show me my buyer list" or "what does [Name] buy?":

**Full list:** Use `mcp__cloud-brain__search_notes` with query `buyer profile` to load all buyer records. Present as a summary table:

```
YOUR BUYER NETWORK ([X] buyers)

NAME              COMPANY          MARKETS           PRICE RANGE    TYPES
────────────────────────────────────────────────────────────────────────
[Name]            [Company]        [Markets]         $[M]–$[M]      [Types]
[Name]            [Company]        [Markets]         $[M]–$[M]      [Types]
...
```

**Single buyer lookup:** Load `brain/buyer-network/[name-slug].md` and display full profile.

**Gap analysis:** If the user asks "who am I missing?", note any strategy/geography combos that are underrepresented. Example: "You have 8 buyers in DFW but none who buy multifamily there. That's a gap worth filling."

---

## Error Handling

- **No buyers in database yet:** Explain the database starts empty — add buyers by sharing their buy box criteria. Offer to add the first one now.
- **Multiple people with same name:** Use company name + phone to disambiguate; ask user to confirm.
- **Geography listed as "nationwide":** Save it but flag — "nationwide" buyers are rarely serious unless they have specific asset class focus. Ask if there's a preferred market they target most.
- **Buyer criteria are very generic (e.g., "buys anything under $500k in USA"):** Save but note: "Generic criteria mean this buyer may not respond to most deals. Try to get more specific criteria on your next call with them."
- **No matches for a deal:** Report honestly. Suggest: expanding to partial matches, adding buyers who specialize in that area, or adjusting the deal structure/price.
- **User asks to send the outreach directly:** Clarify this skill drafts only. For actual sending, use the `comm-email-drafter` skill or their preferred CRM.
