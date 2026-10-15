---
name: past-client-nurture
description: >-
  Systematic touchpoints for past clients — anniversary messages, home value check-ins, referral asks, and milestone follow-ups. Past clients are your highest-ROI source of repeat and referral business. This skill tracks clients in memory, generates personalized annual and quarterly messages, and reminds you who needs a touch. Ties into referral-partner-manager for tracking referrals received from past clients. Triggers on: past client, client anniversary, closing anniversary, client follow up, past client nurture, stay in touch client, client check in, referral ask, home value update, past buyer, past seller.
---

# Past Client Nurture

> **Note:** All outreach must comply with applicable communication laws including CAN-SPAM and TCPA. Always honor opt-out requests immediately. This skill generates message templates — you are responsible for sending from appropriate platforms with proper consent records in place.

You're running a **systematic past client nurture program** — the highest-ROI relationship activity in real estate. Most agents close a transaction and disappear. This skill makes staying connected effortless and consistent.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [Market]. Proceed or update?"

**If not found:** Run the setup interview below, then save.

> Let me save your details so every client message is pre-personalized with your info.
> 1. Your full name?
> 2. Brokerage name?
> 3. Primary market / city?
> 4. Your phone number (for message templates)?
> 5. Your email?
> 6. Do you use a service like Homebot for automated home value reports? (yes/no — affects template recommendations)
> 7. How often do you want to touch past clients beyond anniversaries — quarterly, bi-annually, or anniversaries only?

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
    Uses Homebot: [yes / no]
    Non-Anniversary Touch Frequency: [quarterly / bi-annually / annually only]
    Updated: [date]
```

Show preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 1: ADD A PAST CLIENT

Use this when a deal closes. Capture the client now while the transaction details are fresh — this data powers every future touchpoint.

**Collect the following:**
- Client name(s) — first and last
- Phone number and email
- Property address (purchased or sold)
- Close date — exact (used for all anniversary calculations)
- Client type: Buyer / Seller / Both
- Family details: spouse/partner name, kids (ages if mentioned), pets
- Hobbies or interests mentioned during the transaction (cooking, wine, gardening, sports, dogs, travel, etc.)
- How did they find the agent? (referral from [name] / Zillow lead / open house / sphere / etc.)
- Any notable transaction moments (smooth deal / bumpy inspection / multiple-offer win / sentimental sale)?
- Post-close satisfaction: any issues raised? Any specific compliments?

Save to memory:
```
mcp__cloud-brain__write_note:
  path: brain/clients/[last-name-first-name-slug].md
  content: |
    # Client: [Full Name(s)]
    Address: [property address]
    Close Date: [YYYY-MM-DD]
    Type: [Buyer / Seller / Both]
    Phone: [phone]
    Email: [email]
    Family: [partner name, kids, pets]
    Interests: [hobbies and personal details]
    Referral Source: [how they found agent]
    Transaction Notes: [notable moments, smooth/bumpy, specific details for personalization]
    Post-Close Satisfaction: [notes]
    Referral Status: [Has referred / Has not referred yet / Received referral fee]
    Last Contact: [YYYY-MM-DD]
    Next Scheduled Touch: [YYYY-MM-DD — calculate 1 week from close date]
```

Confirm: "Client saved. I've queued the 1-week check-in for [date], 1-month follow-up for [date], and 1-year anniversary for [anniversary date]."

---

## STEP 2: ANNIVERSARY TOUCHPOINTS

Generate the appropriate message when the agent says "Give me the [X-week / X-month / anniversary] message for [client name]" or when a scheduled date arrives.

Always pull client record first:
```
mcp__cloud-brain__search_notes: "[client name] client"
```

Use personal details from the client record to make every message specific — not generic. The goal is that the client feels remembered, not marketed to.

---

### 1-Week Post-Close Check-In (Text — highest open rate)

"Hey [First Name]! It's been about a week — how are you [settling into the new place / how's the move going]? Everything going smoothly? If anything came up with the property or you need a contractor recommendation, don't hesitate to reach out. — [Agent Name]"

**Personalization tip:** If they mentioned specific excitement (e.g., "can't wait to set up the home office"), reference it: "...how are you settling in? Is the home office coming together?"

---

### 1-Month Post-Close (Email or Text)

**Text version:**
"Hey [First Name]! One month in [address / the new place] — hope it's everything you hoped for! If you haven't already, I'd love it if you could take 2 minutes to leave a quick review — it genuinely helps me connect with more great clients. [Review link]. No pressure at all. — [Agent Name]"

**Email version:**
Subject: "One month in [address] — how are you doing?"

"Hi [First Name],

Hard to believe it's already been a month since you got the keys to [address]! I hope you're loving it.

If you have two minutes, a review on [Google / Zillow] would mean a lot — it genuinely helps me find clients who are a great fit, the way you were.

[Review link]

In the meantime, if anything has come up with the property — or if you need a contractor, a handyman, or any local recommendation — I'm your first call. That part of my job doesn't end at closing.

Talk soon,
[Agent Name]
[Brokerage] | [Phone]"

---

### 6-Month Check-In (Email or Call)

**Email:**
Subject: "Six months in [address] — checking in"

"Hi [First Name],

Six months already — time flies! How are you loving [the neighborhood / the new place]?

I wanted to check in and let you know I'm always here for anything you need — whether it's a market question, a contractor referral, or just to say hello.

One more thing: I'm always grateful for referrals from clients like you. If anyone in your circle is thinking about buying or selling, I would genuinely love to help them the way I helped you. You know better than most how I work.

Hope things are going wonderfully,
[Agent Name]
[Brokerage] | [Phone] | [Email]"

**If agent prefers to call:**
Talk track: "Hey [Name], it's [Agent Name]. I was just thinking about you — six months ago today I handed you the keys to [address]. How are you doing? How's the neighborhood treating you? ... [listen and engage genuinely] ... Listen, I just wanted to call and say — if you ever know anyone thinking about buying or selling, I would love the referral. You know how I work, and I'd take great care of anyone you send my way."

---

### 1-Year Closing Anniversary (Most Important Touchpoint)

The closing anniversary is the highest-impact moment in the past client relationship. Use the option that best fits the client's personality and your relationship style.

**Option A — Personal Text (warm, fast, works for most clients):**
"[First Name]! One year ago today you got the keys to [address]. I genuinely hope this past year has been everything you hoped for in your new home. It was one of my favorite transactions — [specific personal detail from transaction, e.g., 'watching you walk through that backyard the first time' / 'getting that call when your offer was accepted after the third try']. How are you doing? — [Agent Name]"

**Option B — Handwritten Note + Market Update (high-touch, memorable):**
Personal note on branded notecard:

*[First Name],*

*One year ago today, you got the keys to [address]. I hope every day in that home has brought you joy.*

*I wanted to share a quick market update: homes in [neighborhood] are now selling for approximately $[price range estimate based on recent activity] — your home has likely appreciated an estimated $[X] since you purchased. If you'd ever like the full picture, I'm happy to run a current analysis — no obligation, just data.*

*It was genuinely a pleasure working with you.*

*Warmly,*
*[Agent Name]*
*[Brokerage] | [Phone]*

**Option C — Home Value Report (for clients who respond to data):**
"Happy 1-year home anniversary, [First Name]! I ran a quick market snapshot on your neighborhood — homes like yours are selling for [price range] today. Your home may have appreciated approximately $[X] since you purchased at $[purchase price].*

Want the full breakdown? I can run a complete CMA anytime — no obligation, takes about 15 minutes, and you'll know exactly where you stand. — [Agent Name], [Brokerage], [Phone]"

*Note: Frame appreciation figures as estimates. Flag with ⚠️ if market data is insufficient to make a confident estimate.*

---

### Annual Recurring Touchpoints (Year 2 and Beyond)

Rotate through these each year to stay fresh and relevant. Avoid sending the same type of message two years in a row.

- **Anniversary message** (personal, emotional, referencing a memory from the transaction)
- **Market update** (what the neighborhood is doing — tie to their home value)
- **Holiday card** (handwritten preferred; simple and personal)
- **Home value check-in** (formal CMA offer or Homebot enrollment if agent uses it)
- **Seasonal home tip** (useful, low-pressure: "Fall reminder to service your furnace before the cold hits — I use [contractor name] and can refer you if helpful")

---

## STEP 3: REFERRAL ASK SEQUENCE

Strategic referral asks are woven into the touchpoint cadence — not bolted on awkwardly. The best referral asks come right after a positive interaction.

**At 1-month post-close (natural integration):**
Include in the message: "The best compliment you can give me is a referral. If you know anyone thinking about buying or selling, I'd be honored to help them the way I helped you."

**At 1-year anniversary (highest-conversion moment):**
"Now that you've been in your home a full year — if any friends, family members, or coworkers are thinking about making a move, I would love to help them. You know better than anyone how I work, and I'd take great care of anyone you send my way."

**Quarterly soft ask for high-value clients (top referrers or significant transactions):**
Lead with value — a market update, a home maintenance reminder, or a local event tip — then close softly:
"As always, if you cross paths with anyone in the market, I hope you'd think of me. A referral from someone like you carries a lot of weight."

**When a referral is received:**
- Send a personal thank-you immediately (text or call, not email — this deserves real acknowledgment)
- Update the client record in memory with referral details
- Follow up after the referral closes: "Just wanted to let you know — [referred name] closed last week. It was a pleasure working with them, and it's thanks to you. I can't tell you how much that means."

---

## STEP 4: CLIENT STATUS REPORT

On command: "Show me my past client list" or "Who needs a touch?"

Pull all client records from memory:
```
mcp__cloud-brain__search_notes: "brain/clients"
```

Generate a status table:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PAST CLIENT STATUS REPORT — [Agent Name] — [Date]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Client Name    | Close Date  | Anniversary  | Last Contact | Referred? | Status
[Name]         | [date]      | [date]       | [date]       | Yes/No    | [On track / Needs touch / Overdue]
...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ALERTS:
🚨 Anniversary within 30 days: [Name] — [close date anniversary]
⚠️ No contact in 12+ months: [Name], [Name]
```

After displaying the report, ask: "Would you like me to generate messages for anyone on this list?"

Update memory after each outreach:
```
mcp__cloud-brain__edit_note:
  path: brain/clients/[client-slug].md
  [update Last Contact date and Next Scheduled Touch date]
```
