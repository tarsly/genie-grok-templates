---
name: open-house-optimizer
description: >-
  Complete open house automation for top-performing real estate agents — handles the full lifecycle from pre-event prep through post-event follow-up. Use this skill for: open house prep, creating an open house announcement, building a sign-in sheet, neighborhood comparison sheets, agent talking points, property leave-behinds, post-showing lead segmentation, follow-up sequences for hot/warm/cold leads, and generating a seller debrief report after any open house or showing event. Also triggers on: open house flow, visitor feedback, open house debrief, lead nurture after open house.
---

> **Note:** Financing talking points and rate information referenced in this skill should be verified with a licensed mortgage professional before sharing with clients. Always comply with your brokerage's open house policies and applicable fair housing laws when conducting and marketing open houses.

# Open House Optimizer

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
    Phone: [phone]
    Email: [email]
    Preferred CRM: [crm]
    Hot Lead Follow-Up Cadence: [cadence]
    Updated: [date]
```

**Setup interview questions:**
1. Your full name?
2. Brokerage name?
3. Primary market / city?
4. Your contact phone number (for leave-behind materials)?
5. Your contact email?
6. Preferred CRM for routing leads (e.g., Follow Up Boss, Lofty, GHL, manual)?
7. Preferred follow-up cadence for hot leads (e.g., same-day text + next-day call)?

Show a preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

Your personal assistant for eliminating the manual busywork around open houses. This skill automates the complete lifecycle: pre-event preparation, during-event logistics, and post-event follow-up sequences.

## Why This Matters

Open houses generate leads but are notoriously time-intensive. You spend hours creating marketing materials, follow-up templates, and buyer segmentation. This skill does that work in minutes, so you can focus on actual selling.

---

## PHASE 1: PRE-EVENT PREPARATION

### 1A. Branded Open House Announcement

**Your Input:**
- Property address
- Open house date and time
- 3-5 key property features (e.g., "renovated kitchen", "private backyard", "hardwood floors")
- Neighborhood highlights (e.g., "top-rated schools", "walkable downtown", "new retail district")

**What I Generate:**
- Professional multi-paragraph announcement (for email, social, print) — auto-signed with agent name, brokerage, phone, and email from preferences
- Social media post versions (Instagram, Facebook, LinkedIn)
- SMS-friendly teaser text
- Website copy snippet

**Why:** Branded materials project professionalism and drive foot traffic. Prevents the 30-minute writing session.

---

### 1B. Printable Sign-In Sheet Template

**Your Input:**
- Property address
- Open house date

**What I Generate:**
A professional, print-ready spreadsheet template with columns for:
- Visitor Name
- Email Address
- Phone Number
- Current Housing Status (Renting / Own)
- Timeline to Buy (ASAP / 3-6 months / 6-12 months / Just Looking)
- Pre-Approved for Financing? (Yes / No / Unclear)
- Agent Representation? (Yes / Agent Name / No)
- Notes/Interest Level (High / Medium / Low)

**Why:** Standardized collection = faster qualification + better lead routing.

---

### 1C. Neighborhood Comparison Sheet

**Your Input:**
- Property address
- Target buyer profile (first-time homebuyer, family, investor, etc.)

**What I Generate:**
- Recent comparable sales (nearby sold properties, prices, DOM)
- Active competition (other listings in area, days on market)
- School district ratings and test scores
- Walkability index + nearby amenities (parks, restaurants, transit)
- Community highlights (events, local businesses, demographics)
- Tax rates and HOA info if applicable
- Commute times to major employment centers

**Why:** Buyers research neighborhoods online first. Give them curated, one-page proof that this area is the right fit. Differentiates you from agents who just show a house.

---

### 1D. Talking Points Document

**Your Input:**
- Property features and updates
- Buyer concerns you typically hear (e.g., "older neighborhood", "smaller lot")
- Your target buyer profile

**What I Generate:**
A structured document with:
- **Property Highlights** — 5-7 unique selling points with context
- **Objection Handlers** — Anticipated concerns + credible responses
- **Financing Talking Points** — Latest rates, loan product explanations, first-time homebuyer programs
- **Neighborhood Selling Points** — Lifestyle angle, future growth, community fit
- **Qualifying Questions** — Open-ended questions to disqualify or identify hot leads

**Why:** Consistency. You won't forget your pitch, and you'll guide conversations toward deal-critical information (timeline, financing, motivation).

---

### 1E. Property Feature Sheet / Leave-Behind

**Your Input:**
- Full property details (beds, baths, square footage, lot size, year built, updates, systems)
- Listing price

**What I Generate:**
A professional one-page handout with:
- Hero image placeholder / formatted layout
- Property specs and highlights
- Mortgage pre-qualification calculator reference
- Agent name, phone, email, photo/logo — auto-populated from preferences
- Next steps language (e.g., "Call today for a private showing")

**Why:** Visitors forget details. A leave-behind keeps your property top-of-mind and drives follow-up conversations.

---

## PHASE 2: DURING THE EVENT

### 2A. Digital Sign-In Instructions

**Your Input:**
- None required (I generate standard instructions)

**What I Generate:**
Text-based instructions for directing visitors to a digital sign-in form (Google Form, Typeform, etc.):
- QR code placement guidance
- Verbal instructions ("Please sign in using this form before touring")
- Benefits reminder ("Sign in for follow-up on price changes and new listings")

**Why:** Digital sign-in is faster, more complete, and auto-populates your CRM. Reduces transcription errors.

---

### 2B. Open House Flow Strategy

**Your Input:**
- Property layout (room count, flow, any weak points)
- Property style (luxury, first-time buyer, investment)

**What I Generate:**
A tactical tour sequence including:
- **Room Order** — Logical flow that builds momentum
- **Conversation Starters** — What to say as you enter each room
- **Timing Cues** — How long to spend where
- **Qualifying Questions** — When to ask (before tour, during, at exit)
- **Handling Groups** — Solo buyers vs. couples vs. investor groups

**Why:** Tour flow controls narrative. Poor flow kills deals. Structure ensures you qualify leads while maintaining energy.

---

## PHASE 3: POST-EVENT FOLLOW-UP

### 3A. Lead Segmentation & Follow-Up Sequences

**Your Input:**
- Sign-in list (names, emails, phones, timeline, pre-approval status, visit frequency if applicable)

**What I Generate:**
Three distinct follow-up tracks:

#### Hot Leads
- **Profile:** Visited 2+ times, pre-approved, timeline is ASAP-3 months
- **Sequence:**
  - Same-day text: "Thanks for visiting [property]. Let's talk financing options." — auto-signed with agent name, phone from preferences
  - Next-day email: Personalized message + mortgage calculator + ask for showing/offer
  - 3-day phone call (if no response)

#### Warm Leads
- **Profile:** Interested but not pre-approved, 6-12 month timeline
- **Sequence:**
  - 2-day email: "Your interest + lender intro" (pre-qualified referral)
  - 1-week check-in: Neighborhood content + open house highlights video
  - Bi-weekly nurture: Market updates, new listing alerts, financing education

#### Cold Leads
- **Profile:** "Just browsing," 12+ month timeline, low engagement signals
- **Sequence:**
  - 1-week email: Neighborhood + buyer education (no hard sell)
  - Monthly nurture: Market trends, local events, community highlights
  - Quarterly re-engagement: "Still planning to buy? Let's talk options"

**Why:** Personalized cadence = higher conversion than one-size-fits-all blasts. Hot leads get immediate pressure, warm leads get nurture, cold leads stay warm without friction.

---

### 3B. Seller Debrief Report

**Your Input:**
- Attendance count
- General visitor sentiment (engaged, curious, skeptical, etc.)
- Feedback themes you heard (e.g., "kitchen feels small", "location is perfect", "price is high")
- How you felt the open house went overall

**What I Generate:**
A professional debrief for the seller including:
- **Attendance Summary** — Visitor count, source (sign-in digital, walk-by, agent-brought)
- **Interest Level Breakdown** — # of hot, warm, cold leads
- **Feedback Themes** — Top 3 concerns, top 3 compliments
- **Pricing Sentiment** — Are buyers willing to negotiate? Is price a barrier?
- **Next Steps Recommendations** — Price adjustment, staging changes, second showing timing, pricing strategy

**Why:** Sellers want transparency. Honest feedback + recommendations build trust and often lead to price corrections that clear the market.

---

## HOW TO USE THIS SKILL

### Before Your Open House
1. Provide property address, features, and neighborhood details
2. Request: announcement, sign-in sheet, talking points
3. Review, customize, and launch marketing

### During Your Open House
1. Use talking points as your guide
2. Direct visitors to digital sign-in (or use printed sheet)
3. Follow the tour flow strategy
4. Take notes on standout visitors

### After Your Open House
1. Download sign-in list or transcribe from printed sheet
2. Provide the list to this skill
3. Get personalized follow-up sequences (copy/paste ready)
4. Provide seller feedback for debrief report

### Optional Deep Dives
- Ask for a detailed **market analysis** for a specific buyer segment
- Request **financing talking points** customized for first-time homebuyers
- Get **competitive positioning** vs. similar active listings
- Create **follow-up email templates** for specific objections

---

## KEY PRINCIPLES

**Speed Over Perfection:** You can customize anything I generate. The goal is to eliminate blank-page syndrome and give you a professional starting point in minutes, not hours.

**Lead Quality First:** The sign-in sheet and follow-up sequences assume you're qualifying for motivation, timeline, and financing. This filters noise and focuses your energy.

**Seller Confidence:** The debrief report builds trust. Be honest about feedback and positioning. It often unlocks pricing flexibility.

**Consistency Wins:** Branded materials, talking points, and follow-up cadences make you look professional and feel organized. Buyers respond to that.

---

## NEXT STEPS

Ready to automate your open houses? Tell me:
- "Prep my open house for [address] on [date]"
- "Create follow-up sequences for my sign-in list"
- "Generate a debrief report for [seller name]"
- "Build a neighborhood comparison for [address]"

I'll handle the grunt work. You focus on selling.
