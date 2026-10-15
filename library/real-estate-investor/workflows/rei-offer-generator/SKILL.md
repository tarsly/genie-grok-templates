---
name: rei-offer-generator
description: >-
  Generate a real estate offer package — Letter of Intent (LOI), initial offer letter, purchase price justification, and creative finance term sheets for any deal type (cash, conventional, seller finance, subject-to, wrap, BRRRR). Use after deal analysis when ready to make an offer. Reads saved deal analysis from brain to pre-populate numbers.
---

# REI Offer Generator

> **Disclaimer:** This skill generates offer documents for informational and negotiation purposes only. These are not legally binding contracts. Always have a licensed real estate attorney review and finalize any purchase and sale agreement before signing. Laws governing real estate contracts vary by state.

## Overview

Bridges the gap between deal analysis and making an offer. Generates a professional Letter of Intent (LOI), initial offer letter, and creative finance term sheet — pre-populated with numbers from a saved deal analysis. The investor reviews, adjusts, and delivers. Includes a purchase price justification section the investor can share with the seller to support the offer.

## When This Skill Applies

- User says "make an offer", "draft an LOI", "write up an offer letter"
- User has run deal analysis and is ready to proceed
- User wants to structure creative finance terms on paper
- User needs a subject-to or seller finance term sheet
- User asks "how do I present this offer to the seller?"
- User says "draft my LOI for this deal"
- User needs a purchase price justification to share with a seller

---

## Pre-Flight — Load Data

1. **Load investor preferences:** Use `mcp__cloud-brain__search_notes` with query `REI preferences`. Load investor name, company, phone, email, and address (for offer letterhead).
2. **Load deal analysis:** Use `mcp__cloud-brain__search_notes` with the property address or slug. If found, pull: purchase price, financing type, terms, ARV, rent estimates, cap rate, CoC, cash flow. Note: "Found saved deal analysis from {date} — pre-loading those numbers."
3. **If no prior analysis found:** Ask the user for key deal inputs (purchase price, financing type, terms) before proceeding.
4. **Confirm offer details before drafting:** Present a summary and ask: "Before I draft the offer, confirm these details: Offer price: $X | Financing: [type] | Key terms: [summary]. Adjust anything?"

---

## How It Works

### Step 0: Gather Offer Inputs

| Input | Required | Example |
|-------|----------|---------|
| Property address | Yes | 4521 Elm St, Dallas TX |
| Seller name | Helpful | John Smith |
| Offer price | Yes | $155,000 |
| Financing type | Yes | Cash, conventional, seller finance, subject-to, wrap |
| Earnest money deposit (EMD) | Yes | $1,000–$5,000 |
| Due diligence / inspection period | Yes | 10–15 days typical |
| Financing contingency | If applicable | 21 days (conventional) |
| Closing timeline | Yes | 30 days / 60 days / "on or before {date}" |
| Creative finance terms | If seller finance / subto | Rate, term, balloon, monthly payment |
| Included / excluded items | Optional | Appliances, fixtures |
| Special conditions | Optional | As-is, seller pays closing costs |

### Step 1: Draft Letter of Intent (LOI)

The LOI is non-binding and used to establish deal terms before a formal PSA is drafted. Keep it concise — 1 page.

```
LETTER OF INTENT — REAL ESTATE PURCHASE

Date: [Date]

To: [Seller Name / "Property Owner"]
Re: Property at [Full Address]

From: [Investor Name] | [Company Name] | [Phone] | [Email]

---

Dear [Seller Name / "Property Owner"],

This Letter of Intent outlines the general terms under which [Investor Name / Company Name]
("Buyer") proposes to acquire the above-referenced property. This letter is non-binding
and is intended only to facilitate further negotiation. Final terms are subject to
execution of a mutually acceptable Purchase and Sale Agreement.

PROPOSED TERMS

Purchase Price:          $[Offer Price]
Financing Type:          [Cash / Conventional / Seller Finance / Subject-To / Wrap]
Earnest Money Deposit:   $[EMD Amount] (due within [X] days of PSA execution)
Due Diligence Period:    [X] days from PSA execution
[Financing Contingency:  [X] days (if applicable)]
Closing Date:            On or before [Date], or [X] days after PSA execution
Title / Escrow:          Buyer's choice of title company
Property Condition:      [As-is / Subject to inspection]

[If Seller Finance:]
SELLER FINANCE TERMS
Purchase Price:          $[Price]
Down Payment:            $[Down] ([X]%)
Seller-Carried Note:     $[Note Amount]
Interest Rate:           [X]%
Amortization:            [XX] years
Balloon Due:             [XX months / years]
Monthly Payment (P&I):   $[Payment]
Prepayment Penalty:      [None / specify]

[If Subject-To:]
SUBJECT-TO TERMS
Existing Loan Balance:   $[Balance] (as of [date])
Existing Rate / Payment: [X]% / $[Payment]/mo
Cash to Seller at Close: $[Amount] (purchase price minus loan balance)
Buyer assumes existing mortgage payments as of [close date]

ADDITIONAL TERMS
• Seller to provide clear and marketable title
• Property to be delivered vacant (unless otherwise agreed)
• [Any other key conditions]

This LOI expires at 5:00 PM on [Date + 3–5 business days]. Please indicate acceptance
by signing below or contacting me directly.

Respectfully submitted,

[Investor Name]
[Company Name]
[Phone] | [Email]

---

SELLER ACCEPTANCE (non-binding)
Signature: _________________________ Date: _________
Printed Name: _______________________
```

### Step 2: Draft Purchase Price Justification

A one-page document the investor can share with the seller to support the offer price. Builds credibility and reduces friction.

```
PURCHASE PRICE JUSTIFICATION
Property: [Address]
Offered By: [Investor Name / Company]

WHY [PRICE] IS A FAIR OFFER

[3–4 bullet points explaining the basis for the offer price. Draw from saved deal analysis.]

Examples:
• Recent comparable sales in this area range from $[Low] to $[High].
  At $[Price], this offer is [consistent with / slightly below] the market
  given the property's [condition/needed repairs/age/features].

• Based on an independent analysis, the property in its current condition
  is estimated at $[as-is value]. The offer of $[price] accounts for
  [repairs/carrying costs/financing risk/time to close].

• If the property were listed on the MLS, it would likely need [list work]
  before it would attract retail buyers. I'm offering to take it as-is,
  which saves you [time / realtor commission / repair costs].

• I can close in [X] days with [no financing contingency / cash / minimal
  conditions], which has tangible value compared to a retail buyer requiring
  [30–60 days] and bank approval.

WHAT THIS OFFER SAVES YOU COMPARED TO A RETAIL SALE
[Present a simple comparison if helpful:]
| | Retail MLS Sale | This Offer |
|---|---|---|
| Estimated Sale Price | $[ARV] | $[Offer] |
| Realtor Commission (6%) | ($[Amount]) | $0 |
| Repairs Required | ($[Est. Repairs]) | $0 |
| Days to Close | 60–90 days | [X] days |
| Net to Seller | ~$[Net] | $[Offer] |

[Investor Name]
[Phone] | [Email]
```

### Step 3: Creative Finance Term Sheet (if applicable)

For seller finance or subject-to deals, provide a clean one-page term sheet the seller can review:

```
CREATIVE FINANCE TERM SHEET
Property: [Address]
Proposed By: [Investor Name / Company]
Date: [Date]

[SELLER FINANCE OPTION:]

Purchase Price:        $[Price]
Down Payment:          $[Down] (paid at closing)
Seller-Carried Note:   $[Note] at [X]% interest
Amortization:          [XX] years
Balloon Payment Due:   [Month/Year] (balance: ~$[Estimate])
Monthly Payment:       $[P&I]

What This Means for You:
• You receive $[Down] at closing
• You receive $[Payment]/month for [term] (like a steady income stream)
• At the balloon date, you receive the remaining ~$[Balance]
• Total received: $[Down] + $[Total Payments] = ~$[Grand Total]
• Plus interest income of ~$[Total Interest] over the term

[SUBJECT-TO OPTION:]

You keep the existing mortgage in place. I take over the payments immediately.

Existing Loan Balance:    $[Balance]
Your Existing Payment:    $[Payment]/mo (I handle this going forward)
Cash You Receive at Close: $[Price minus balance]
No bank involvement, no credit check — we close through a title company.

This term sheet is not a contract. All terms are subject to a mutually executed
Purchase and Sale Agreement reviewed by both parties and their attorneys.

[Investor Name] | [Phone] | [Email]
```

### Step 4: Save to Cloud Brain

```
Path: brain/offers/{address-slug}-offer-{YYYY-MM-DD}.md
Tool: mcp__cloud-brain__write_note
```

Tell the user: "Offer package saved. After the seller accepts, run `rei-due-diligence-tracker` to set up your contract-to-close deadline calendar."

---

## Output Delivered

1. **LOI** — ready to email or print
2. **Purchase Price Justification** — share with seller to support the offer
3. **Creative Finance Term Sheet** — if applicable
4. **Delivery note** — suggested subject line and brief cover message for sending the LOI

---

## Error Handling

- **No deal analysis in brain for this address:** Ask for key inputs manually (price, terms, financing type). Recommend running `rei-deal-analyzer` first for stronger justification.
- **No earnest money amount provided:** Default to $1,000 and note — investor should adjust based on market norms and seller relationship.
- **No closing timeline specified:** Default to "on or before [30 days from today]." Confirm with investor.
- **Subject-to but no existing loan details:** Ask for current balance, rate, monthly payment, and remaining term — required to build the term sheet.
- **Seller name unknown:** Use "Property Owner" in all documents; recommend finding the name via county records for a more personal delivery.
- **Investor asks to make the LOI binding:** Explain that a binding contract requires a properly executed Purchase and Sale Agreement reviewed by attorneys, not an LOI. Offer to note which terms should carry forward into the PSA.
