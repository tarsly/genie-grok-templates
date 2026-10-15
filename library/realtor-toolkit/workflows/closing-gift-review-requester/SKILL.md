---
name: closing-gift-review-requester
description: >-
  Two-part post-close skill: generate a personalized closing gift recommendation tailored to the client's profile and budget, and draft a warm, effective review request message that actually gets responses. Most agents skip both — this skill makes it effortless. Triggers on: closing gift, gift for buyer, gift for seller, review request, ask for review, Google review, Zillow review, client review, testimonial request, post-close, after closing.
---

# Closing Gift + Review Requester

> **Note:** Some brokerages have policies governing closing gifts — check with your broker regarding allowable gift types, maximum values, and any required disclosures. This skill provides recommendations and message templates only. You are responsible for compliance with your brokerage's advertising and gift policies.

You're running the **post-close sequence** — two high-leverage actions that most agents skip: a thoughtful closing gift and a well-timed review request. Both take 10 minutes and pay dividends for years.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Display and confirm — "Using: [Agent Name], [Brokerage], [Market]. Gift budget: $[X]. Review platforms: [platforms]. Proceed or update?"

**If not found:** Run the setup interview below, then save.

> Let me save your post-close preferences so every gift and review request is pre-loaded.
> 1. Your full name?
> 2. Brokerage name?
> 3. Typical closing gift budget per transaction? (e.g., $50 / $100 / $150 / $200+)
> 4. Gift style preference — local/personalized, practical/useful, or experiences?
> 5. Which review platforms do you use? (Google, Zillow, Realtor.com, other)
> 6. Your Google review link — optional but recommended (paste here for auto-population)?
> 7. Your Zillow review link — optional?

```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
  content: |
    # Real Estate Agent Pro — Preferences
    Agent: [name]
    Brokerage: [brokerage]
    Market: [market]
    Closing Gift Budget: $[X]
    Gift Style: [local-personalized / practical / experiences]
    Review Platforms: [list]
    Google Review Link: [url or blank]
    Zillow Review Link: [url or blank]
    Updated: [date]
```

Show preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## PART 1: CLOSING GIFT GENERATOR

### Step 1A: Client Profile

Pull from transaction memory if available, otherwise collect:
```
mcp__cloud-brain__search_notes: "[client name] client"
```

Gather or confirm:
- Client name(s)
- Buyer or seller?
- First-time buyer? (milestone moment — warrants a more meaningful gift)
- Family composition: single / couple / family with kids (ages?) / empty nester
- Pets mentioned?
- Hobbies or interests noted during the transaction (cooking, wine, gardening, outdoor activities, sports teams, travel, coffee, etc.)
- New construction (brand new home) or resale?
- Budget from preferences — confirm or override for this transaction

---

### Step 1B: Gift Recommendations

Generate three personalized options at the agent's budget. Each option includes what to buy and why it fits this client.

**For Buyers — First-Time Homeowner:**
- Milestone keepsakes: custom house portrait (watercolor or illustrated), framed "first keys" shadow box, personalized address sign or doormat, custom map print of their neighborhood
- Home starter kit: quality tool set, cleaning supply basket (Method, Grove, etc.), smart home starter device (smart plug, video doorbell)
- Local welcome box: curated gift box from local businesses in their new neighborhood (coffee, bakery gift cards, wine from a local spot)
- ⚠️ Avoid: anything too interior-style-specific (you don't know their taste), anything that implies their current setup is inadequate

**For Buyers — Family with Kids:**
- Neighborhood experience: gift cards to local family-friendly restaurants, bowling alley, or family activity center nearby
- Family game night set (quality board games, snack basket)
- Age-appropriate activity kits (Lego sets, art supplies, backyard games)

**For Buyers — Investment Property:**
- Practical for landlord life: landlord/property management logbook, smart lock for tenant turnover, a nice coffee setup for the office
- Skip sentimental gifts — focus on useful

**For Buyers — Luxury ($200+ budget):**
- Wine or spirits club subscription (first month covered)
- Spa or restaurant experience gift card (high-end local spot)
- Custom home engraving or monogram piece
- Premium smart home device (smart thermostat, premium speaker)
- Curated local artisan basket (premium cheeses, meats, wine — sourced locally)

**For Sellers — "What's Next" Theme:**
- Travel gift card (Chase, Airbnb, airline) — especially if they mentioned a trip or move
- Dining experience at a nice local restaurant
- Moving essentials box: quality packing tape dispenser, markers, address labels, a bottle of wine for moving day, congratulations card

**For Sellers — Long-Term Homeowner (sentimental sale):**
- Framed photo of the home they sold — especially powerful if they lived there 10+ years or raised kids there. Commission from a local photographer or offer to arrange a print of a listing photo
- Custom illustration of the home (etsy artists do this for $50–150)
- "What's next" journal or travel planner if they're starting a new chapter

**Always include regardless of gift type:**
- A handwritten personal note (see Step 1C)
- Your business card attached naturally (not as the centerpiece)

---

### Step 1C: Handwritten Note Template

Generate a personalized note. The note must contain at least one specific reference to something real from the transaction — not a generic template. Pull from transaction notes in client memory.

Template structure:

"Dear [First Name(s)],

It was such a pleasure helping you [buy / sell] [address / 'your home on [street]'].

[Specific personal moment from the transaction — e.g., 'When you walked into that backyard for the first time and I saw your face light up, I knew this was the right house.' / 'Watching you navigate that bumpy inspection process with such grace — you handled it better than most experienced buyers I've worked with.' / 'The day your offer was accepted after we lost out on those other two homes — that was one of my favorite phone calls I've made this year.']

I hope every day in [your new home / your next chapter] brings you exactly what you hoped for.

Please don't hesitate to reach out anytime — whether it's a market question, a contractor recommendation, or just to say hello. That part of my job doesn't end at closing.

Warmly,
[Agent Name]
[Brokerage]
[Phone]"

**Prompt for personalization:** If no specific transaction memory is available, ask: "What's one specific moment from this transaction that stood out — something the client said, a challenge you overcame together, or a moment they seemed genuinely happy?"

---

## PART 2: REVIEW REQUEST SYSTEM

### Step 2A: Timing Strategy

Reviews are most likely to happen when emotion is high and the ask is easy. The sequence:

1. **Primary ask — 3 to 7 days after closing** (best window: client is in the home honeymoon phase, gratitude is fresh, emotion is high)
2. **Second chance — 1-month post-close check-in** (natural touchpoint, can weave review ask back in if no review yet)
3. **Final ask — 6-month check-in** (if still no review, one last gentle ask — after this, let it go)

Confirm which window applies: "Has this client already left a review? Which ask number is this — primary, 1-month, or 6-month?"

---

### Step 2B: Review Request Templates

**Text Message (highest response rate — use this first):**

*Primary ask (3–7 days):*
"Hey [First Name]! Hope you're settling into [address / the new place]. Congrats again — it was such a pleasure working with you! If you have 2 minutes, a quick review on Google would genuinely help me connect with more great clients like you. Here's the link: [Google link from preferences]. No pressure at all — just means a lot. — [Agent Name]"

*1-month follow-up (if no review yet):*
"Hey [First Name]! Just thinking about you — hope month one at [address] has been wonderful. I know I mentioned this before, but if you ever have a couple minutes, a review on [Google / Zillow] still means the world. [link]. Thanks so much — and reach out anytime! — [Agent Name]"

**Email (for clients who prefer email or didn't respond to text):**

Subject: "A small favor — [First Name]?"

"Hi [First Name],

I'm so glad we got to work together on [address]. [One specific sentence about something from the transaction.] Working with you was genuinely one of my favorite parts of this year.

If you have a few minutes, a review on [platform] would help me connect with more clients who are a great fit — the kind of clients I love working with.

Here's the direct link: [link]

It takes about 2 minutes and means more than I can say. No pressure if now isn't a good time — and of course I'm always here for anything you need.

Warmly,
[Agent Name]
[Brokerage] | [Phone]"

**What to say if client asks what to write (coaching them):**
"If you're not sure what to write, just think about: what made this experience different from what you expected? What would you tell a friend who was thinking about working with me? Even one or two genuine sentences is incredibly helpful — you don't need to write an essay."

---

### Step 2C: Thank-You When a Review Is Received

When the agent confirms a client left a review:

**Personal thank-you text (send immediately):**
"[First Name]! I just saw your review — thank you so much. That genuinely made my day (maybe my week). I'll be cheering you on in your new home and if there's ever anything I can do for you, please don't hesitate. — [Agent Name]"

**Also:** Remind agent to respond publicly to the review within 24 hours. See Step 2D.

---

### Step 2D: Public Response Templates

Agent posts these responses on the review platform to demonstrate professionalism to future readers.

**Standard positive review response:**
"[Client First Name], thank you so much — this genuinely made my day! It was such a pleasure working with you on [address / your home search / your sale]. Helping clients like you is exactly why I love what I do. Please don't hesitate to reach out anytime, and I hope you love every moment ahead!"

**For a review that mentions a specific challenge overcome:**
"[First Name], I so appreciate you taking the time to share this — and especially for mentioning [challenge they referenced]. Those are the moments that test everyone, and you handled it with so much grace. It was truly a pleasure working with you and I'm so glad we got you to the finish line. Congratulations!"

**For a short, simple review ("5 stars — great agent!"):**
"Thank you so much, [First Name]! It was a pleasure working with you and I'm so glad everything came together. Reach out anytime — I'm always here!"

---

### Step 2E: Review Platform Tracking

After the review sequence runs, update client memory:

```
mcp__cloud-brain__edit_note:
  path: brain/clients/[client-slug].md
  [update: Review Requested: [date], Review Received: [yes/no/date], Platform: [platform]]
```
