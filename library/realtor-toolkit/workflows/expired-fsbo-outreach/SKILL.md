---
name: expired-fsbo-outreach
description: >-
  Multi-touch outreach sequences for expired listings and FSBOs — the two warmest lead types in real estate. Generate personalized letters, text scripts, call scripts, email sequences, and door-knock approaches tailored to each prospect's specific situation. Handles both expired listing owners (frustrated after a failed listing) and FSBO sellers (trying to sell on their own). Triggers on: expired listing, expired lead, FSBO, for sale by owner, cold outreach, prospect outreach, listing lead, expired script, FSBO script, door knock, prospecting.
---

# Expired Listing & FSBO Outreach

> **Note:** All outreach must comply with Do Not Call (DNC) registry rules, your state's solicitation laws, and your MLS/brokerage policies. Always verify you are permitted to contact a prospect before reaching out. This skill generates outreach templates — you are responsible for compliance with applicable laws and regulations.

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
4. Your phone number (for call scripts and contact info)?
5. Your email address (for email templates)?
6. Typical days before first contact on an expired listing — day of expiration, or the next morning?

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
    Expired First Contact: [day of / day after]
    Updated: [date]
```

Show a preferences banner at the top of every output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 1: IDENTIFY OUTREACH TYPE

**Ask the agent:**

1. Are you reaching out to an **expired listing** or a **FSBO**?
2. Seller's name? (first name at minimum)
3. Property address?
4. Any relevant context? (how long it was listed, price, any obvious issues, days on market)

Proceed to the appropriate part below.

---

# PART 1: EXPIRED LISTING OUTREACH

## Understanding Expired Sellers

Before writing a single word, understand who you're talking to. Expired sellers are not cold leads. They:

- Already tried to sell and failed — there is an emotional wound here
- Were let down by the process, the agent, or the market
- Have likely already received 5–10 calls and letters from other agents — they are skeptical
- Are still motivated — they wanted to sell then, and they probably still do
- Respond to empathy and specific diagnosis, not generic pitches

**Your competition is doing this:** Calling with enthusiasm, pitching themselves, sending glossy postcards with their headshot. Do the opposite. Be a diagnostician, not a salesperson.

---

## What to Know Before You Call

Pull from MLS and review:

- How long was the listing active? Longer = likely overpriced. Shorter = possibly condition or marketing issue.
- How many price reductions? (Multiple reductions = seller was chasing the market)
- What was list price vs. comparable recent solds? (Calculate price per sq ft gap)
- How many showings estimated? (If MLS shows showing data: lots of showings + no offers = price problem. Few showings = marketing or price problem. No activity = severely overpriced or major condition issue.)
- Were there any back-on-market or status changes suggesting a deal fell apart?

**Use this intelligence throughout the sequence.** Generic outreach fails. Specific, informed outreach earns the conversation.

---

## EXPIRED SEQUENCE — 5-Touch Campaign

*(All templates auto-populated with agent name, brokerage, phone, email, and market from preferences.)*

---

### Touch 1 — Day 0: Direct Mail Letter

Send the same day the listing expires. Arrive in the mailbox before competing agents' postcards — and sound nothing like them.

**Purpose:** Acknowledge their frustration, offer a specific diagnosis, earn a callback. Do not pitch yourself.

---

[Agent Name]
[Brokerage] | [Phone] | [Email]
[Date]

Dear [Seller Name],

I noticed that your home at [address] recently came off the market, and I wanted to reach out personally.

I know this isn't easy. You prepared your home, made it available for showings, and went through the emotional process of waiting for an offer that didn't come. That's exhausting — and frustrating in a way that's hard to explain to people who haven't been through it.

I'm not writing to pitch you on hiring me. I'm writing because I've been analyzing the recent listing activity in your neighborhood and I think I understand specifically what happened — and more importantly, what would need to be different for a second listing to succeed.

Before your home came off the market, I was already watching it. I pulled the comparable sales. I looked at the showing activity pattern and the pricing data. And I have a clear theory about why it didn't sell — one I'd rather share with you directly than put in a mailer.

If you're open to a 20-minute conversation — no pitch, no pressure — I'd welcome the chance to share what I found. You deserve an honest answer about what went wrong, whether you hire me or not.

I'll follow up with a quick call in the next day or two. If you'd prefer to reach me first: [phone] or [email].

Sincerely,

[Agent Name]
[Brokerage]
[Phone] | [Email]

---

### Touch 2 — Day 1: Phone Call (morning)

Call between 8:30–10:00 AM when people are available and not yet in the middle of the day.

**Goal:** Earn 20 minutes — not a listing. Do not rush past "no" into a pitch.

---

**Opening:**
"Hi, is this [Seller Name]? This is [Agent Name] with [Brokerage] in [market]. I sent you a note yesterday about your home at [address]. Did you happen to get it?"

*(Pause — let them respond)*

"I'm not calling to give you a pitch or tell you I'm the best agent in [market]. I'm calling because I've done a quick analysis of your neighborhood and I think I have a specific theory about what happened with your last listing — and I'd rather share it with you directly than send another generic flyer. Do you have 20 minutes this week to hear it?"

---

**If they say "We're not ready to re-list yet":**
"That makes complete sense — the timing is entirely yours. I'm not asking you to list today. I'm just asking for 20 minutes to share what I found. Would it be okay if I followed up in a week or two? I promise I won't make it awkward."

**If they say "We already have an agent / we're re-listing with our old agent":**
"No problem at all — that's their listing to lose, not mine to take. Out of curiosity — did you re-list with the same agent or someone new?" *(Pause.)*

- *If same agent:* "Interesting. I'd still love to send you the analysis I put together — just as a second opinion on the pricing and marketing approach. No strings attached. You should have that data regardless of who you list with."
- *If new agent:* "Great. Good luck with it — I hope it goes smoothly."

**If they're hostile or dismissive:**
"I completely understand — you've probably gotten a dozen calls like this in the last 24 hours. That's fair. I won't take up your time. If you ever want a second opinion on what the data shows about your home's market position, my number is [phone]. I wish you the best."

*(Hang up gracefully. Mark them for a 2-week follow-up.)*

---

### Touch 3 — Day 3: Email with Market Data

**Subject:** What I found after analyzing [address] — [Agent Name]

---

[Seller Name],

I wanted to follow through on my offer to share what I found when I analyzed your home's listing.

I pulled the three most recent closed sales closest to your property:

| Address | Beds/Baths | Sq Ft | Price/Sq Ft | DOM | Close Price |
|---|---|---|---|---|---|
| [Comp 1] | [x/x] | [X] | $[X] | [X] | $[X] |
| [Comp 2] | [x/x] | [X] | $[X] | [X] | $[X] |
| [Comp 3] | [x/x] | [X] | $[X] | [X] | $[X] |

Based on this data, my read on your situation is:

**[Specific diagnosis — choose the one that fits:]**

*(Overpriced:)* "Your listing price was approximately $[X] above where comparable homes have been closing. At $[X]/sq ft vs. the market's current $[X]/sq ft, buyers looking at homes in your range were comparing your home directly to options that were priced $[X] less. That's a difficult position to overcome regardless of how well-presented the home is."

*(Marketing/exposure:)* "Your home was priced reasonably close to market — which suggests the issue was reach, not price. Looking at the listing photos and syndication pattern, I believe the home didn't get the buyer traffic it deserved. With the right photography and targeted digital advertising, the pool of buyers seeing it could have been 3–4x larger."

*(Market timing + pricing:)* "The market shifted meaningfully while your home was listed. What was a competitive price in [month] is no longer supported by the comps that closed in [recent months]. This isn't unusual — and it's fixable with a recalibrated pricing strategy."

I believe that at $[recommended price], based on current market data, you'd see a meaningful change in buyer activity — with an expected timeline to offer of approximately [X days] if the home is marketed correctly.

If you'd like to see my full analysis — including what I'd do differently on the marketing side — I'd be glad to walk through it in a 20-minute call. No obligation.

[Agent Name]
[Phone] | [Email]

---

### Touch 4 — Day 7: Second Phone Call

Lighter, referencing the email. Keep it brief.

---

"Hi [Seller Name], this is [Agent Name] again — I sent over a quick market analysis on [address] a few days ago. Did you get a chance to look at it?

[If yes]: Great. What did you think? Did anything in the data surprise you?

[If no / lost it]: No problem — let me resend it tonight. It's just two pages — shows specifically how your home compared to the three most recent closed sales in your area. Worth a look before you make any decisions about next steps.

Either way, I just wanted to make sure you had it."

---

### Touch 5 — Day 14: Final Outreach

One last reach. Graceful, no pressure, leaves the door open permanently.

---

**Email subject:** Last check-in — [address]

"[Seller Name],

I don't want to be a bother — and I'll keep this brief.

I've genuinely enjoyed thinking about this property and what the right approach would be. I put together a full pricing and marketing analysis that I'd be glad to share if you ever want it.

Whether you re-list with me, with someone else, or decide to wait — I hope you get the outcome you were looking for. If circumstances change and you'd like to talk, my door is always open.

[Agent Name]
[Phone] | [Email]"

---

# PART 2: FSBO OUTREACH

## Understanding FSBO Sellers

FSBOs are not anti-agent — they are anti-commission. Their belief system:
- Agents are not worth the cost
- Selling a home is something they can manage themselves
- Buyers will come to them directly and they'll keep the commission

**Your approach:** Do not argue with this directly. Instead, demonstrate value they cannot replicate, educate on real risks they haven't considered, and earn trust through genuine helpfulness before ever asking for the listing.

**Your first goal with a FSBO is not a listing — it's a relationship.** Most FSBOs who list with an agent do so after 3–6 weeks on market.

---

## What to Know Before You Call

- How long has the FSBO been listed? (Under 2 weeks = still optimistic. Over 4 weeks = starting to doubt. Over 60 days = very ready for a change.)
- How are they marketing? (Zillow FSBO listing, Craigslist, Facebook Marketplace, yard sign only, personal website?)
- How is their price compared to recent comps? (Overpriced FSBOs are your best opportunity.)
- Do they have an agent for the buy side of the move? (Often creates pressure to close the sale faster.)

---

## FSBO SEQUENCE — 4-Touch Campaign

*(All templates auto-populated from preferences.)*

---

### Touch 1 — Day 1: Initial Phone Call

**Goal:** Get showing access + plant the seed that buyer agents = more buyers. Do NOT ask for the listing.

---

"Hi, I'm calling about the home for sale at [address]. This is [Agent Name] with [Brokerage].

I'm not calling to list your home — I represent buyers and I'm always looking for properties to show in [neighborhood]. I have a couple of clients searching in your price range right now.

Quick question: are you open to working with a buyer's agent if they brought you a qualified buyer?"

*(Pause — let them answer.)*

*[If yes]:* "Great. What I'll do is add your home to the properties I'm showing my buyers this week. Can you walk me through the property briefly? Beds, baths, square footage, and any upgrades worth highlighting? And what's your availability for showings?"

*[If hesitant about buyer agent commission]:* "That's fair — and worth thinking through. What most sellers find is that offering a buyer's agent commission actually increases the pool of buyers significantly, because 89% of buyers use an agent. But let's not get into that right now — tell me about the home and I'll see if it fits my clients' criteria."

---

**At the end of the call:**
"One more thing — I run a weekly market report for [neighborhood]. Would it be helpful if I sent you the most recent comp data for your area? I put it together anyway and you should have it regardless of how you sell. Just an email address is all I need."

---

### Touch 2 — Day 5: Follow-Up Call with Value

---

"Hi [Name], this is [Agent Name] — I called about [address] earlier this week. How's it going so far?"

*(Listen genuinely. Ask follow-up questions.)*

"I mentioned I'd pull some comp data for you — I've put together a quick analysis of the three most recent closed sales within a half mile of your home. I'd like to drop it by or email it to you — no charge, no strings. You should have this data whether you sell with an agent or not.

Would it be easier if I dropped a copy by the house, or would you rather I email it?"

---

### Touch 3 — Day 14: The Honest Conversation

For sellers who are still actively trying to sell on their own — have an honest, adult conversation about what's ahead.

---

**Phone script:**

"Hi [Name], [Agent Name] here. I noticed you've been on market about two weeks — how's it going? Are you getting much traffic?"

*(Listen. They'll often reveal exactly where they're struggling.)*

"I want to be honest with you — and I say this respectfully, not to scare you into hiring me.

Most FSBOs I've talked to say the hardest part isn't finding an interested buyer. It's what comes after. Once you get an offer, you're negotiating against a buyer's agent who does this every day. You're managing the inspection response — which is where a lot of deals fall apart. You're coordinating with title and making sure every disclosure is completed correctly. I've seen sellers lose more in a bad inspection negotiation than they would have paid in commission.

I'm not trying to talk you out of selling yourself if that's what you want to do. I just want you to go in with clear eyes.

Would it be worth 30 minutes to walk through what you'd be facing if you got an offer tomorrow? I can show you the typical contract, the inspection process, and what the closing timeline looks like. No cost, no pitch — just information."

---

### Touch 4 — Day 30+: The Market Reality Conversation

For FSBOs who have been on market 30+ days without a sale. They are now genuinely open to a different approach.

---

**Phone script:**

"Hi [Name], [Agent Name] here. I've been keeping an eye on your listing on Zillow — it's been about [X] weeks now. I want to ask you a direct question: are you still confident in your price?

I've been tracking the sales in your neighborhood, and three similar homes have closed in the past few weeks. The data is telling a story — and I think you deserve to see it before you decide what to do next.

I'm not calling to push anything. But if you're at the point where you're questioning whether your strategy is working, I'd like to come by for 15 minutes and show you what I'm seeing. No pressure, no hard sell. Just the numbers.

Would [Tuesday or Wednesday] morning work?"

---

## PROSPECT MEMORY — SAVE OUTREACH STATUS

When tracking active expired or FSBO prospects, save to memory:

```
mcp__cloud-brain__write_note:
  path: brain/projects/outreach-[address-slug]-[YYYY-MM-DD].md
  content: |
    # Outreach Log — [Address]
    Type: [Expired / FSBO]
    Seller Name: [name]
    Address: [address]
    Contact Info: [phone/email if obtained]
    
    Status: [Active / Quiet / Listed with agent / Listed with me / Withdrawn]
    
    MLS Intelligence:
    - List price: $[X]
    - DOM: [X] days
    - Price reductions: [X]
    - Estimated showing activity: [high/medium/low/unknown]
    - Diagnosis: [pricing / marketing / condition / market timing]
    
    Touch Log:
    - Touch 1 (Day 0): [sent / not sent]
    - Touch 2 (Day 1): [called / no answer / conversation notes]
    - Touch 3 (Day 3): [email sent / no response]
    - Touch 4 (Day 7): [called / notes]
    - Touch 5 (Day 14): [final outreach sent]
    
    Next Action: [description + date]
    Notes: [any relevant context from conversations]
```
