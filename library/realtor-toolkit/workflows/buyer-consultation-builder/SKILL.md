---
name: buyer-consultation-builder
description: >-
  Generate a complete, personalized buyer consultation package for real estate agents to use with new buyer clients. Post-NAR settlement context makes a professional buyer consultation more critical than ever for explaining agency, getting buyer rep agreements signed, and setting expectations. Builds: welcome agenda, local market snapshot, buying process timeline, agency agreement explainer, how-we-work-together overview, mortgage pre-approval checklist, and discovery questions. Use for any new buyer meeting, buyer intake, or when you need to explain a buyer representation agreement. Triggers on: buyer consult, buyer consultation, new buyer, buyer presentation, buyer rep agreement, buyer intake, working with buyers, buyer meeting, first time buyer.
---

> **Disclaimer:** Buyer representation agreement requirements, agency disclosure obligations, and compensation rules vary significantly by state and have been evolving following the NAR settlement (effective August 2024). This skill provides a professional framework — it is not legal advice. Always verify current requirements with your broker, your state's real estate commission, and a licensed real estate attorney. Compensation must be disclosed in writing per current NAR settlement requirements and state law. Do not present any agency agreement to a client without your broker's review.

# Buyer Consultation Builder

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
    Preferred Lender: [lender-name, phone]
    Default Rep Agreement Duration: [duration]
    Buyer Agent Compensation: [fee structure]
    Updated: [date]
```

**Setup interview questions:**
1. Your full name?
2. Brokerage name?
3. Primary market / city?
4. Preferred lender partner (name, phone) — optional?
5. Default buyer rep agreement duration (e.g., 90 days, 6 months)?
6. Your typical buyer agent compensation / fee structure?

Show a preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## What This Does

Generate a **complete, professional buyer consultation package** from basic buyer info. Post-NAR settlement (Dec 2024+), buyer representation agreements are now **critical** — agents must explicitly sign buyers and explain compensation. This skill automates the entire first meeting: personalized welcome, market snapshot, buying timeline, agency agreement explainer, process walkthrough, pre-approval checklist, and discovery questions.

## What You Need

Provide buyer information:
- **Buyer name(s)** and contact info
- **Pre-approval status** + approximate budget (or range)
- **Target location(s)** (neighborhood, city, zip)
- **Home preferences** (beds/baths, type, must-haves)
- **Timeline** (moving in 60 days? 6 months? open-ended?)
- **Current situation** (renting, own home to sell, relocating, first-time buyer)
- **How they found you** (referral, online, open house, etc.)

## Output: Your Full Consultation Package

### 1. **Personalized Welcome + Agenda**
A warm, professional opening that addresses the buyer by name, acknowledges their unique situation (renting, relocation, first-timer), and outlines the meeting agenda. Sets a collaborative tone. Auto-signed with agent name and brokerage from preferences.

### 2. **Local Market Snapshot**
Ready-to-fill template covering:
- Current inventory levels (# active listings)
- Average days on market (market velocity)
- List-to-sale price ratio (are homes selling above/below ask?)
- Recent price appreciation trend (6-month, 1-year)
- Buyer's vs seller's market indicator
- Guidance on WHERE to pull this data (your local MLS, CAR/NAR reports, Zillow, REALTOR.com)

This section educates buyers on **why timing and strategy matter** in today's market.

### 3. **The Buying Process Timeline**
A clear, visual step-by-step journey from pre-approval to keys:
```
Pre-Approval → Search → Offer → Under Contract → Inspection →
Appraisal → Final Walk-Through → Close → Keys
```
Each stage includes realistic timeframes (e.g., "Search: 2-8 weeks depending on market"). Buyers see the full road ahead and understand where they are at any point.

### 4. **Agency Agreement Explainer (Post-NAR Context)**
Plain-English explanation of the Buyer Representation Agreement:

**What it is:** A contract stating you represent the buyer's interests exclusively for a defined time period (usually [rep agreement duration from preferences]).

**Why it exists (post-NAR settlement):** The National Association of REALTORS settled antitrust claims in Nov 2024. The old MLS commission rule (showing compensation) is gone. Now agents **must have signed buyer rep agreements** and explicitly discuss compensation *before* showing homes. This protects both agent and buyer.

**What it means for the buyer:**
- You have a fiduciary (legal) duty to act in their best interest
- You won't show them properties where you have a conflict
- You'll negotiate on their behalf
- You keep their information confidential

**How compensation works:**
- Seller typically pays commission (split between listing agent and buyer's agent) — this hasn't changed
- If buyer chooses to work with a property discount site or FSBO, they may need to negotiate their own fee
- Buyer's agent fee is typically disclosed by seller's agent or negotiated in the offer

**Common Objections (Pre-Answered):**
- *"What if I find a house on my own?"* → If you find it while we're working together, the agreement covers it. If you cancel and find it 6 months later on your own, it doesn't.
- *"What if I want to fire you?"* → You can end anytime, but the agreement may include an early termination clause. Discuss.
- *"Do I have to pay you?"* → No, seller typically does. If buying FSBO or short-sale, we'll discuss upfront.

### 5. **How We'll Work Together**
Your process overview — auto-populated with agent name, brokerage, and preferred lender from preferences:
- **Communication:** How often you'll touch base (weekly calls? texts when new listings drop?)
- **Listings:** How you send opportunities (email, text, MLS alert link, private showings?)
- **Offer Speed:** Your commitment to moving fast ("I review offers within 2 hours", "We submit within 24 hours")
- **What You Need:** Clear expectations (pre-approval letter, signed rep agreement, loan pre-quals, ID, proof of funds if cash buyer)
- **Market Intelligence:** You'll keep them updated on neighborhood trends, sold comps, rate moves

### 6. **Mortgage Pre-Approval Checklist**
If buyer isn't pre-approved yet, provide a checklist to gather:
- Last 2 years W-2s
- Last 2 months pay stubs
- Last 2 months bank statements (all accounts)
- Copy of ID (driver's license or passport)
- Employment verification letter or recent offer letter
- Explanation letter for any credit issues or employment gaps
- (Optional) Proof of funds if closing with cash

**Lender suggestions:** "Shop with 2-3 lenders to compare rates and fees. Consider your bank, credit union, and a mortgage broker. Pre-approval is free and takes 3-5 business days." Preferred lender from preferences auto-referenced here.

### 7. **Discovery Questions for You to Ask**
Use these to uncover true motivation, urgency, and deal-breakers:

**Timeline & Urgency:**
- When do you ideally want to move in?
- Is there a hard deadline (job start, school year, lease end)?
- Would you rent for 3 months if the perfect house needs 60 days?

**Motivation:**
- What's driving the move now?
- Are you growing a family, downsizing, relocating for work, upgrading?
- What does your ideal home look like?

**Deal-Breakers:**
- What's a must-have vs. nice-to-have?
- Any neighborhoods you absolutely won't consider?
- How far from work/family do you want to be?
- Commute time tolerance?

**Emotional Drivers:**
- Is this your first home, or have you bought before?
- How are you feeling about the buying process?
- Any concerns or anxieties I should know about?

**Offer Strategy:**
- How aggressive would you be in a bidding war?
- Any inspection contingencies non-negotiable for you?
- Would you consider a subject-to-inspection-waiver for the right home?

---

## Why This Matters Post-NAR Settlement

Before December 2024, buyer rep was implicit and compensation was MLS-visible. Now:
- **Explicit agreements required** — avoid liability, set expectations
- **Transparency demanded** — you must discuss compensation upfront
- **Professional positioning critical** — formal consultations differentiate you from discount agents
- **Buyer education essential** — many buyers don't understand the new market rules

A polished, thorough buyer consultation shows you're professional, knowledgeable, and acting in their interest. It converts prospects to signed buyers and reduces friction down the road.

---

## How to Use This Output

1. **Print or share the consultation package** with your buyer before or at the first meeting
2. **Customize the Local Market Snapshot** with your latest MLS data
3. **Walk through the timeline** to reset expectations and reduce surprise at closing
4. **Have the buyer sign the rep agreement** during the meeting (if they're ready)
5. **Keep the discovery questions handy** — they're your roadmap for uncovering their true needs
6. **Follow up** with a polished PDF version and any next steps (loan pre-qual, scheduling showings)

This turns a casual "let's grab coffee" into a **formal, professional buyer onboarding** that protects you and sets your buyers up for success.
