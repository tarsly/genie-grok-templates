---
name: showing-scheduler
description: >-
  Eliminate showing scheduling back-and-forth and automate feedback collection for real estate agents. For buyer agents: generate optimized showing itineraries, draft MLS showing requests, send buyer confirmation messages, and build property comparison worksheets. For listing agents: send professional feedback requests after each showing, compile buyer sentiment reports, and generate seller debrief presentations. Use this skill any time you need to schedule showings, create a buyer tour itinerary, collect showing feedback, or create a seller report from showing activity. Triggers on: schedule showings, showing request, showing feedback, buyer tour, feedback from showings, showing itinerary, no feedback on my listing, showing debrief, seller feedback report.
---

> **Note:** Showing request protocols and feedback collection practices should comply with your MLS rules and brokerage policy. Always respect seller showing instructions and local fair housing laws when scheduling tours.

# Showing Scheduler — Automate Buyer Tours & Listing Feedback

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
    Showing Platform: [platform]
    Default Notice Required: [notice]
    Feedback Follow-Up Timing: [timing]
    Updated: [date]
```

**Setup interview questions:**
1. Your full name?
2. Brokerage name?
3. Primary market / city?
4. Preferred showing platform (ShowingTime / CSS / MLS direct / manual)?
5. Default notice required for showings (e.g., 24 hours, 48 hours)?
6. Typical feedback follow-up timing (e.g., same-day, next morning)?

Show a preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Overview

Real estate agents waste hours on two repetitive tasks: (1) scheduling property showings across multiple addresses with buyers, and (2) chasing showing feedback from buyer's agents after each one. Showing Scheduler eliminates the back-and-forth for both buyer's agents and listing agents.

**For buyer's agents:** Input properties + buyer availability, get an optimized itinerary, draft showing requests for each listing agent, and a buyer confirmation message.

**For listing agents:** Input the listing and buyer's agent contacts, generate professional feedback request messages, compile feedback into a structured sentiment report, and create a seller debrief presentation ready to show at a pricing consultation.

This skill automates the work because feedback collection directly informs pricing decisions, contingency removal negotiations, and seller confidence.

---

## When This Skill Applies

**Buyer's Agent Use Case:**
- "Schedule showings for my buyer"
- "Create a showing itinerary"
- "Generate showing requests"
- "Buyer tour" or "property tour"
- "Organize multiple property showings"
- "Draft buyer confirmation for showings"

**Listing Agent Use Case:**
- "Get showing feedback"
- "Collect buyer feedback on my listing"
- "No feedback on my listing — help me reach out"
- "Create feedback request messages"
- "Compile showing feedback"
- "Showing debrief" or "seller feedback report"
- "What did buyers think?"
- "Analyze showing feedback"

---

## USE CASE 1: BUYER'S AGENT — Schedule Showings

### What The Skill Does

Given a list of properties to show and buyer's available dates/times, the skill generates:

1. **Optimized Showing Itinerary** — Properties ordered by geography/drive time to minimize driving. (Buyer can reorder as needed.)

2. **Draft Showing Request Messages** — One for each property, in professional MLS/ShowingTime format. Ready to copy/paste to listing agents. Auto-signed with agent name and brokerage from preferences. Includes:
   - Buyer profile (if helpful: first-time buyer, investor, etc.)
   - Requested showing times with flexibility window
   - Preferred contact method
   - Any access notes (gated, requires realtor entry, etc.)

3. **Buyer Confirmation Message** — SMS/email template to send the buyer confirming the full tour schedule. Auto-signed with agent name and brokerage from preferences. Includes:
   - Full itinerary with addresses, times, drive times
   - Parking/access notes
   - A reminder to think about what matters most (price, condition, layout, etc.)

4. **Property Comparison Worksheet** — Template the buyer can print or use on a phone during the tour. Columns for:
   - Address, price, square footage
   - Condition rating (1-5)
   - Key features (kitchen, yard, parking, layout, etc.)
   - Red flags or standout qualities
   - First impression: Would recommend for offer? Yes/No/Maybe

5. **Post-Tour Debrief Prompt** — Questions to ask the buyer after the showing tour:
   - "Which home felt most like home?"
   - "What did you love about each?"
   - "What was the biggest concern or deal-breaker?"
   - "Which property offers the best value for the price?"
   - "Any you'd rule out now?"

### How To Use It

**Input:** Provide the buyer's name, properties (addresses, list prices, key details), and available showing windows (dates/times).

Example:
```
Buyer: Sarah & Mike
Available: Sat 3/1 10am-4pm, Sun 3/2 2pm-6pm

Properties:
- 123 Oak St, $425k (3bed, kitchen reno)
- 456 Elm Ave, $410k (4bed, needs work)
- 789 Pine Rd, $450k (3bed, smart home)
```

**AI Actions:**
1. Analyze addresses for geographic clustering (minimize drive time)
2. Build an optimized itinerary with travel time estimates
3. Draft showing request messages for each listing agent
4. Create buyer confirmation message with full schedule
5. Generate comparison worksheet template
6. Provide post-tour debrief questions

**Output:** All templates ready to send. Copy/paste to MLS, text buyer, print worksheet.

---

## USE CASE 2: LISTING AGENT — Collect & Compile Showing Feedback

### Part A: Generate Feedback Request Messages

**What The Skill Does**

Given a listing address and buyer's agent contact info, generate professional feedback request messages. Tone: collegial, brief, specific. Not desperate — just asking smart questions that move the deal forward. Auto-signed with agent name and brokerage from preferences.

**Message includes:**
- Thank you for showing
- Specific questions: Did the price feel right? Did your buyer see value? What concerns came up? Would your buyer consider writing an offer? What would it take to get them to offer?
- Optional: "Anything we should address to make this more appealing?"
- Professional close: "Happy to discuss over a call if it helps."

**Input:**
```
Listing: 123 Maple Drive, $550k
Showing Agent: Sarah Chen (sarah@realestate.com)
Showing Time: Saturday 2pm
```

**Output:** Customized text/email ready to send. Can send via text, email, or ShowingTime feedback module.

### Part B: Compile Feedback Into Seller Debrief Report

**What The Skill Does**

When the agent pastes in feedback received from multiple showing agents, the skill compiles it into a structured **Seller Debrief Report** that includes:

1. **Showing Activity Summary:**
   - Total showings held
   - Average buyer profile (first-time, investor, downsizer, etc.)
   - Price range of buyers
   - Feedback response rate (% of showing agents who replied)

2. **Sentiment Analysis:**
   - Positive themes (what buyers loved: "beautiful kitchen", "great bones", "quiet cul-de-sac")
   - Negative themes (concerns: "needs updated bathroom", "too close to road", "price feels high")
   - Price feedback ("Buyers think it's priced right" vs. "overpriced for the condition" vs. "underpriced — multiple offers")
   - Offer likelihood: Low / Medium / High (based on agent feedback)

3. **Verbatim Standout Quotes:**
   - Best compliments (exactly what buyers said they loved)
   - Most common objections (word-for-word concerns)
   - Offers or near-offers (if any agent mentioned buyer interest in writing)

4. **Market Context:**
   - Comparable market conditions
   - DOM (days on market) benchmark for the area
   - Whether feedback suggests pricing is aligned with market

5. **Recommended Next Steps:**
   - "Price adjustment recommended to $525k (feedback suggests overpriced)" OR
   - "Price is competitive — expected to get offers"
   - "Fix the [specific thing everyone mentioned] to increase appeal"
   - "Schedule open house to generate buzz" OR
   - "Feedback suggests strong buyer interest — expect multiple offers"
   - "Consider staging [specific room]"

### How To Use It

**Input:** The listing address, then paste feedback from showing agents (can be from ShowingTime, text messages, emails, or just notes).

Example:
```
Listing: 123 Maple Drive, $550k

Feedback:

Sarah Chen (Sat 2pm showing):
"Buyers liked the kitchen and location. Price feels a bit high for the condition. Master bath is dated. They're not going to write an offer but may circle back if price adjusts."

Marcus Rodriguez (Sun 10am):
"Buyers loved the hardwood floors and lot size. Bathroom renovations would help a lot. Interest level 6/10. They want to see more homes before deciding."

Alex Thompson (Sun 2pm):
"Perfect home for first-time buyers but financing fell through. They thought the price was fair."
```

**AI Actions:**
1. Parse each feedback entry
2. Extract sentiment (positive/negative), specific themes, price feedback
3. Identify common patterns (e.g., "3 mentions of dated bathrooms")
4. Flag any offers or strong interest signals
5. Recommend pricing/marketing adjustments based on feedback patterns

**Output:** Professional Seller Debrief Report (formatted for PDF or to share in a listing consultation).

### Part C: Seller Debrief Presentation

**What The Skill Does**

Format the compiled feedback into a clean, professional report the agent can:
- Email to the seller
- Present at a pricing consultation
- Use to justify a price reduction or repositioning strategy
- Share as evidence that the home is receiving showings (builds seller confidence)

**Report includes:**
- Title: "Showing Feedback Report — [Address] — [Date]"
- Summary snapshot (# showings, # feedback responses, sentiment score)
- Buyer feedback themes (positive, concerns, price feedback)
- Standout quotes (anonymized)
- Market analysis (is the price aligned?)
- Actionable recommendations (fix/stage/price adjustment/marketing push)
- Call to action: "Let's discuss next steps on [date/time]" — auto-signed with agent name and brokerage from preferences

---

## Workflow Examples

### Example 1: Buyer's Agent Scheduling a 3-Property Tour

**User:** "I have a buyer visiting Saturday 10am–2pm. Schedule these properties: 123 Oak, $425k (3bed, updated); 456 Elm, $410k (4bed, older); 789 Pine, $450k (3bed, smart home). All in the Riverside area."

**AI Generates:**
1. Optimized itinerary (456 Elm → 123 Oak → 789 Pine, sorted by drive time)
2. Three MLS-format showing request messages
3. Buyer confirmation text: "Hey Sarah! Here's your Saturday tour schedule..."
4. Comparison worksheet with property details
5. Post-tour debrief questions: "Which felt like home?"

**User sends:** Requests to listing agents via MLS, text to buyer, prints worksheet.

---

### Example 2: Listing Agent Collecting Feedback on 5 Showings

**User:** "My listing got 5 showings this weekend. [Pastes feedback from 4 showing agents]."

**AI Generates:**
1. Parses 4 feedback entries
2. Identifies themes: "3 mentions of outdated kitchen, 2 mentions of great location, price feedback mixed (2 say fair, 1 says high)"
3. Compiles into Debrief Report: "Showing Activity: 5 showings, 4 responses. Sentiment: Mixed. Positive: location, hardwood. Negative: kitchen outdated. Price: Feedback suggests slightly high for condition."
4. Recommendations: "Schedule open house, consider kitchen staging or $20k price adjustment"

**User presents** report to seller to manage expectations and justify next steps.

---

## Key Features

- **Time zone aware:** Respects buyer availability across time zones
- **Drive time estimates:** Uses geographic data to optimize itinerary
- **Professional messaging:** MLS-compliant, polite, specific
- **Anonymized feedback:** Seller reports don't name buyers
- **Sentiment scoring:** Quantifies buyer interest (High/Medium/Low offer likelihood)
- **Pattern detection:** Flags repeated concerns across multiple showings
- **Call-to-action ready:** Every output is actionable — copy/paste ready

---

## Why This Matters

**For buyer's agents:**
- Stop text-threading with 5 different listing agents
- Give buyers a professional tour experience (schedule, worksheet, debrief)
- Respond faster to showings (buyers approve tour before 48hr notice required)

**For listing agents:**
- Stop chasing feedback — send one professional message, get faster responses
- Show sellers real data on buyer interest and concerns (not guesses)
- Use feedback to make confident pricing/staging decisions
- Build seller confidence: "We're getting showings and feedback — here's what buyers said"

Feedback collection is the difference between a listing that sits with unknown issues and one where the agent proactively fixes problems or adjusts price based on real market intel.
