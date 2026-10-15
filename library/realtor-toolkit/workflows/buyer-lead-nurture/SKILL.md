---
name: buyer-lead-nurture
description: >-
  Systematic nurture sequences for buyer leads at every stage — active buyers ready now, buyers in 3-6 months, buyers in 6-12 months, and long-term leads. Generates personalized email, text, and call templates for each segment with the right cadence and content type. Saves leads to memory for ongoing tracking. Use to set up drip sequences, add new leads, check lead status, or generate outreach for a specific lead. Triggers on: buyer lead, nurture sequence, follow up buyer, buyer drip, buyer pipeline, lead follow up, buyer not ready, long term buyer, buyer touch base, stay in touch with buyer.
---

# Buyer Lead Nurture

> **Note:** All outreach must comply with CAN-SPAM, TCPA, and applicable state communication laws. Always obtain proper consent before texting buyers and honor opt-out requests immediately. This skill generates templates — you are responsible for compliance with communication laws.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [Market]. Proceed or update?"

**If not found:** Run the setup interview below, then save.

**Setup Interview — ask all questions before proceeding:**

1. Your full name?
2. Brokerage name?
3. Primary market / city?
4. Your phone number (for text templates)?
5. Your email address?
6. Preferred CRM system? (Follow Up Boss / Lofty / GHL / kvCORE / manual — affects format of templates and tagging recommendations)
7. Do you send market updates manually or via a service like Homebot, Cloud CMA, or similar?

Save preferences:

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
    CRM: [system or "manual"]
    Market Update Method: [manual / Homebot / Cloud CMA / other]
    Updated: [date]
```

Show a preferences banner at the top of every output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 1: SELECT ACTION

**Ask the agent what they need:**

1. **Add a new lead** — capture info and generate their first outreach sequence
2. **Generate outreach for an existing lead** — pull from memory and generate next touch
3. **View the full buyer pipeline** — display all leads by segment with follow-up status
4. **Re-engage a quiet lead** — generate a re-engagement sequence
5. **Update a lead's status** — move a lead between segments or mark as closed/inactive

---

## STEP 2: LEAD INTAKE (for new leads)

If the agent is adding a new lead, collect the following. Mark optional items.

**Required:**
1. Lead's full name?
2. Phone number?
3. Email address?
4. How did you meet them / lead source? (open house / referral / online inquiry / past client / cold call / event)

**Qualifying Information:**
5. Pre-approval status? (Yes — approved / In process with lender / No — not started yet)
6. If pre-approved: approximate approval amount or budget range?
7. Target neighborhoods, areas, or school districts?
8. Timeline? (Ready now / 3 months / 6 months / 12+ months / Just exploring — no real timeline)
9. Current housing situation? (Renting month-to-month / Lease ends [date] / Owns — needs to sell first / Relocating / Living with family)
10. First-time buyer or has bought before?
11. Primary motivation — what's driving the move? (Growing family, job change, downsizing, lifestyle, investment, relocation)
12. Any known hesitations or obstacles? (Waiting for rates to drop, needs to sell first, saving for down payment)

**Save new lead to memory:**

```
mcp__cloud-brain__write_note:
  path: brain/leads/buyer-[name-slug].md
  content: |
    # Buyer Lead — [Full Name]
    Added: [date]
    Source: [source]
    
    Contact:
      Phone: [phone]
      Email: [email]
    
    Qualifying Info:
      Pre-Approval Status: [status]
      Budget/Approval Amount: [amount]
      Target Areas: [areas]
      Timeline: [timeline]
      Current Situation: [situation]
      First-Time Buyer: [yes/no]
      Primary Motivation: [motivation]
      Known Hesitations: [hesitations or "none identified"]
    
    Segment: [A / B / C / D — assigned below]
    Status: Active
    
    Touch Log:
      [date]: Added to pipeline — [source]
    
    Next Action: [description + date]
```

---

## STEP 3: SEGMENT ASSIGNMENT

Based on timeline and pre-approval status, assign the lead to a segment. Show the assignment clearly and explain it to the agent.

---

**SEGMENT A — HOT** (Active, 0–90 days)
- Pre-approved or nearly there
- Actively searching; viewing listings; ready to make offers
- Timeline is now or within 3 months

*What they need:* New listings delivered fast, showing support, offer guidance, fast response times. This is an active client relationship, not a nurture sequence.

---

**SEGMENT B — WARM** (3–6 months out)
- Has clear motivation and a real timeline
- May need: time to get pre-approved, to sell current home, school year timing, lease end
- Engaged and responsive but not ready to view homes actively

*What they need:* Regular market education, occasional curated listings, pre-approval encouragement, milestone check-ins. Build trust now so they call you when they're ready to move.

---

**SEGMENT C — COOL** (6–12 months out)
- Clear motivation but timeline is genuinely not urgent
- Watching the market, not making moves yet
- Responsive when contacted but not initiating

*What they need:* Monthly low-pressure value adds, no asks, market context, occasional listings for awareness. Stay top of mind without being a pest.

---

**SEGMENT D — LONG-TERM** (12+ months or "just exploring")
- Early-stage, no real decision made
- May be inspired by curiosity, life event on the horizon, or general interest
- No timeline, may not be pre-qualified

*What they need:* Quarterly gentle touches. Educational content. Zero pressure. The goal is to still be their agent when the timeline crystallizes.

---

## STEP 4: NURTURE SEQUENCES

Generate the complete multi-touch sequence for the lead's segment. Auto-populate with agent name, brokerage, market, phone, email, and lead name from memory.

---

### SEGMENT A — HOT SEQUENCE

**Cadence:** Weekly minimum. Same-day response to any inquiry. Immediate listing alerts.

---

**Day 1 — Text (within hours of meeting):**
"Hey [Name]! [Agent Name] here — great meeting you at [source]. I'm adding your criteria to my search right now. You'll start getting listings as soon as something matches. Any questions in the meantime, just text me here."

**Day 1 — Email (same day):**

Subject: Your personalized home search is live — [Agent Name]

"Hi [Name],

It was great meeting you [today/recently]. I've set up your personalized search based on what you're looking for:

- Area(s): [target areas]
- Budget: up to $[X]
- Key criteria: [bedrooms, bathrooms, must-haves]

You'll get an email the moment something matching your criteria hits the market. I also watch for properties before they're officially listed — I'll reach out directly when I find something worth a look.

One thing that will help us move fast: [if not pre-approved] — getting your pre-approval locked in now means when the right home hits, we can move immediately. I can connect you with [lender name] if you'd like an introduction. [If pre-approved: You're in great shape with your pre-approval — we're ready to move the moment we find the right one.]

Looking forward to finding you something great.

[Agent Name]
[Phone] | [Email]"

---

**Day 3 — Email: First Curated Listings**

Subject: 3 homes I think you should see — [neighborhood]

"Hi [Name],

I've been keeping an eye on the market since we talked, and I've flagged a few that match your criteria:

[Listing 1]: [Address] — [beds/baths/price]. [1 sentence on why it fits their criteria — be specific.]

[Listing 2]: [Address] — [beds/baths/price]. [1 sentence on standout feature.]

[Listing 3]: [Address] — [beds/baths/price]. [1 sentence on value or opportunity.]

I can set up showings for any of these — including back-to-back if you want to see multiple in one trip. What does your schedule look like this weekend?

[Agent Name]"

---

**Day 7 — Call Script:**

"Hi [Name], [Agent Name] here. Just wanted to check in — did any of those listings I sent catch your eye? [Wait for response.]

[If yes]: Great — I can get us in to see it [day/time options]. Is [day] good for you?

[If no]: No problem. Tell me — what's missing? I want to refine the search so we're only looking at homes that actually fit. [Listen and take notes to update memory.]

One thing I want to flag: [any relevant market intel — e.g., 'inventory is really tight right now and homes are moving in under a week at this price point. When you see something you like, we'll want to move same-day if possible.']"

**Weekly from here:**
- New listing alerts as they hit (automated or manual)
- Brief text or email when something notable happens: price reduction on a home they showed interest in, similar home just sold above asking (market context), new listing in target area

---

### SEGMENT B — WARM SEQUENCE

**Cadence:** Every 2 weeks for the first 2 months, monthly thereafter until timeline tightens.

---

**Week 1 — Email: Market Update**

Subject: What's happening in [market] real estate right now

"Hi [Name],

I wanted to keep you in the loop on what the [market] market is doing — especially since you're planning to buy in the next few months.

3 things worth knowing right now:

1. **Inventory:** There are currently [X] homes for sale in [target area]. That's [more/less/about the same] as 3 months ago — which means [implication for buyer: more choices / more competition / etc.]

2. **Prices:** Median sale price in [area] is currently $[X], [up/down/flat] vs. 6 months ago.

3. **Interest Rates:** Rates are currently around [X]% for a 30-year conventional. [1 sentence on what this means for their buying power — e.g., 'On a $400k purchase, that's a payment of approximately $X/month.']

Bottom line for you: [1–2 sentences on strategic recommendation given their timeline and situation.]

Questions? Happy to talk through it. And whenever you're ready to start viewing homes, just say the word.

[Agent Name]"

---

**Week 3 — Text: Curated Listing Mention**

"Hey [Name] — [Agent Name] here. Quick one: [address] just came on in [area they mentioned]. [One specific thing about why it fits: 'Has the big yard you mentioned' / 'Only one on the market under $X with 4 bedrooms in that school district']. I know you're still a few months out, but worth keeping on your radar. Want me to send you the details?"

---

**Week 5 — Email: Educational Content**

Pick the topic most relevant to their specific situation:

*For first-time buyers:*
Subject: What nobody told me about buying my first home (from a buyer's agent)

"Hi [Name], I work with a lot of first-time buyers, and the questions that come up most often are ones nobody thinks to ask until they're in the middle of the process. Here are 5 things I always cover early:

1. **Pre-approval vs. pre-qualification** — they are not the same, and only one carries weight with sellers.
2. **Earnest money** — what it is, where it goes, and when you can lose it.
3. **The inspection process** — what happens, what you can negotiate, and when to walk.
4. **Closing costs** — buyers pay 2–3% of the purchase price in addition to the down payment. Many first-time buyers are surprised by this.
5. **The timeline** — from accepted offer to close is typically 30–45 days. Here's what happens during that time.

Happy to go deeper on any of these — no pressure, just want you to feel prepared."

*For buyers waiting on rates:*
Subject: The rate question — should you wait?

"Hi [Name], I know you mentioned keeping an eye on where rates go before pulling the trigger. Fair. Here's the honest math worth considering:

[1-paragraph analysis of the rate-timing question: price appreciation vs. rate drops, refinancing later vs. buying now, opportunity cost of waiting. Keep it factual, not salesy.]

I'm not trying to push you into a decision. I just want you to have the full picture when you decide."

*For buyers who need to sell first:*
Subject: Your buying timeline and the sell-first question

"Hi [Name], I wanted to touch base on the sell-before-buy piece — it's one of the more complex parts of your situation and I want to make sure you're thinking about it early.

[Overview of bridge financing options, contingent offers and their challenges in the current market, timing considerations for listing their current home relative to buying.]

When you're ready to run through the full plan — sell timeline, what to expect in the process, and how to bridge the gap — I'm here. The earlier we map it out, the smoother it goes."

---

**Week 7 — Call Script:**

"Hi [Name], [Agent Name] here. Quick check-in — has your timeline changed at all? [Listen.]

[If timeline is tightening]: Great. Let's start thinking about getting your search more active. Have you connected with a lender yet? [If no: introduce lender.] And do you want to start doing a few showings just to calibrate — seeing a few homes in person really helps clarify what matters.

[If timeline is same]: No problem. I'll keep sending market updates. When you start feeling the urgency, just reach out — we can move fast."

---

**Monthly from Week 7+:**
One market update email + one listing when relevant. Light, helpful, no pressure.

---

### SEGMENT C — COOL SEQUENCE

**Cadence:** Monthly. No more frequent — these leads will tune out if contacted too often.

---

**Month 1 — Email: Neighborhood Spotlight**

Subject: What's selling in [target neighborhood] — [month, year]

"Hi [Name],

Thought you'd find this useful — here's a quick snapshot of what's happened in [target neighborhood] over the past 30 days:

- Homes sold: [X]
- Median close price: $[X]
- Average days on market: [X]
- Price trend vs. 6 months ago: [up/down/flat X%]

[1–2 sentence interpretation: 'Inventory remains tight, which means the homes that are priced well are still moving quickly. If you're planning to buy in this area, getting a pre-approval locked in before you're ready to view homes is a smart move.']

No action needed — just wanted to keep you informed. Talk soon.

[Agent Name]"

---

**Month 2 — Text: Light Touch**

"Hey [Name] — [Agent Name] here. Quick question: has your timeline shifted at all? I have something I think you'd genuinely love — but only want to send it over if the timing makes sense. No pressure either way."

*(If they respond positively: pivot to a Segment B approach and send the listing + a call.)*
*(If they say 'still a while out': "No worries — I'll keep an eye on things and check back in a month.")*

---

**Month 3 — Email: Buyer's Guide or Educational Content**

Subject: The [market] buyer's guide — what I wish every buyer read before starting

"Hi [Name],

I put together a short guide I share with buyers before they start the process — covers timeline, what to expect at each stage, and the questions most buyers don't think to ask until they're already in escrow.

[Link to buyer guide on website, OR offer to send as PDF, OR include 3–4 key points inline]

No ask — just useful information I want you to have before you need it. When the time gets closer, I'm here.

[Agent Name]"

---

**Quarterly from Month 3+:**
One brief call or email to re-qualify timeline and keep the relationship warm.

---

### SEGMENT D — LONG-TERM SEQUENCE

**Cadence:** Quarterly. The goal is to still be their agent in 12–18 months. Light. Helpful. Zero pressure.

---

**Q1 — Email: Annual Market Report**

Subject: [Market] real estate in [year] — what the data showed

"Hi [Name],

As we head into [new year / new quarter], I wanted to share a brief overview of what the [market] real estate market looked like over the past year — and what the data suggests for buyers planning ahead.

[3–5 bullet market summary: price trends, inventory trends, rate trends, buyer/seller balance, notable neighborhood shifts]

If you're still planning to buy at some point, this is the context worth having. Nothing actionable right now — just keeping you informed.

Happy to answer any questions whenever they come up.

[Agent Name]"

---

**Q2 — Email: Market Update + Personal Note**

Subject: Quick update + thinking of you

"Hi [Name],

I know it's been a few months — just wanted to check in and share a quick market note.

[2–3 sentences on current market conditions — what's happening with inventory, prices, rates]

I remember you mentioned [something from their intake — target area, motivation, timeline]. When that timeline starts to get closer, I'd love to be the first call you make. I'll keep an eye on things between now and then.

[Agent Name]"

---

**Q3 — Email: Neighborhood Spotlight**

Subject: What's happening in [area they mentioned]

"Hi [Name],

[Specific neighborhood] has seen some movement lately that I thought you'd find interesting:

[3 brief data points — recent sales, price per sq ft, DOM, any notable trends]

When you're ready to get serious about this area, we'll have a solid baseline to work from.

[Agent Name]"

---

**Q4 — Email: Year-End Re-Engagement**

Subject: Heading into [year] — still planning to buy?

"Hi [Name],

Hard to believe another year is almost up. I wanted to reach out directly: are you still planning to buy in [area] at some point?

No pressure either way — I just want to make sure I'm giving you the right level of attention. If your plans are still on track, I'd love to get a quick 10-minute call on the calendar to recalibrate. If life has taken things in a different direction, that's completely fine too — just let me know and I'll adjust my outreach accordingly.

Either way, thank you for staying in touch. I'm rooting for you.

[Agent Name]"

---

## STEP 5: RE-ENGAGEMENT (Leads Gone Quiet)

For any lead who has not responded to 2+ consecutive touches. Use before moving to long-term or marking inactive.

---

**Re-Engagement Text:**

"Hey [Name], no pressure at all — just want to make sure I haven't dropped the ball on my end. Still planning to buy in [area]? Happy to back off completely if timing has changed. Just want to make sure I'm being useful, not annoying."

*(If they don't respond to this within 7 days: send the email version.)*

---

**Re-Engagement Email:**

Subject: Quick check-in — [Name]

"Hi [Name],

I've reached out a couple times and want to make sure I'm not missing you or flooding your inbox at the wrong time.

If your buying plans are still on track — even if the timeline has shifted — I'd love to hear where things stand. A quick reply is all I need.

If circumstances have changed and you're no longer planning to buy, no hard feelings at all — just let me know and I'll stop the updates.

Either way, I hope things are going well.

[Agent Name]"

---

**If no response after 2 re-engagement attempts:**
Move to Segment D (Long-Term) quarterly sequence. Update memory to tag as "quiet."

---

## STEP 6: BUYER PIPELINE REPORT

On command ("show me my buyer pipeline" or "pipeline report"), pull all leads from memory and display:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BUYER PIPELINE REPORT — [Date]
[Agent Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SEGMENT A — HOT ([X] leads)
Name | Budget | Area | Last Contact | Next Action
[Name] | $[X] | [area] | [date] | [action + date]

SEGMENT B — WARM ([X] leads)
Name | Budget | Area | Last Contact | Next Action
[Name] | $[X] | [area] | [date] | [action + date]

SEGMENT C — COOL ([X] leads)
Name | Area | Timeline | Last Contact | Next Action
[Name] | [area] | [timeline] | [date] | [action + date]

SEGMENT D — LONG-TERM ([X] leads)
Name | Area | Last Contact
[Name] | [area] | [date]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ATTENTION NEEDED:
🚨 Overdue (Hot — no contact 7+ days): [names]
⚠ Overdue (Warm — no contact 21+ days): [names]

TOTAL PIPELINE: [X] leads
  Hot: [X] | Warm: [X] | Cool: [X] | Long-Term: [X]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## UPDATING LEAD STATUS

When a lead's situation changes, update memory:

```
mcp__cloud-brain__read_note:
  path: brain/leads/buyer-[name-slug].md
```

Then update with new segment, status, and touch log entry:

```
mcp__cloud-brain__write_note:
  path: brain/leads/buyer-[name-slug].md
  content: [updated content — preserve all existing fields, update changed fields, append to touch log]
```

**Status options:**
- Active (in a segment)
- Under Contract — Buyer (actively in a transaction)
- Closed — Buyer (deal done)
- Quiet (no response — on long-term sequence)
- Inactive — Withdrew (no longer buying)
- Lost to Another Agent
