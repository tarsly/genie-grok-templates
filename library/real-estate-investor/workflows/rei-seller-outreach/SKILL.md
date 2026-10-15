---
name: rei-seller-outreach
description: >-
  Draft seller outreach — letters, text messages, voicemail scripts, emails, and cold call scripts for motivated sellers. Handles pre-foreclosure, probate, absentee owner, tired landlord, tax delinquent, code violations, vacant, divorce, estate, and batch outreach, or wants to draft a letter, text, or call script for a motivated seller situation.
---

# REI Seller Outreach Drafter

## Overview

Drafts personalized outreach to property sellers across every channel: physical letter, text message, voicemail script, email, and cold call framework. Each piece is tailored to the seller's situation (pre-foreclosure, probate, absentee, tired landlord, divorce, tax delinquent, code violations, vacant, estate) with empathy, a clear value proposition, and a soft call to action. Supports single-property and batch generation. Reads investor contact info and brand voice from saved preferences so you never re-enter your own details.

## When This Skill Applies

- User wants to reach out to a property owner about buying their property
- User has a list of seller leads and needs outreach drafted
- User says "write a letter to this seller" or "draft outreach"
- User needs a cold call script for motivated sellers
- User wants a voicemail script for property owners
- User needs text / SMS messages for seller leads
- User has pre-foreclosure, probate, or tax-delinquent leads
- User asks for a "yellow letter" or direct mail piece
- User wants batch outreach for a list of addresses

---

## Pre-Flight — Investor Preferences

1. Use `mcp__cloud-brain__search_notes` with query `REI preferences`
2. **If found:** Load saved contact info (investor name, phone, email, website, company name, credibility statement) and brand voice preference. Display ⚙️ banner. Confirm: "Using your saved contact info for outreach materials — [Name], [Phone]. Is that still correct?"
3. **If not found:** Ask in ONE message:
   > "Let me save your investor profile so you never have to re-enter contact info. Tell me:
   > - Your name and company name
   > - Phone number (for outreach materials)
   > - Email and website
   > - One-line credibility statement (e.g., 'I've worked with 50+ sellers in the Dallas area' or 'Local investor, 5 years in this market')
   > - Brand voice preference: Professional/formal, Friendly/conversational, or Direct/to-the-point"

   Save to `brain/preferences/rei-preferences.md` via `mcp__cloud-brain__write_note`. Proceed.

---

## How It Works

### Step 0: Gather Seller Information

| Input | Required | Example |
|-------|----------|---------|
| Property address | Yes | 4521 Elm St, Dallas TX |
| Owner name | Helpful | "Jane Smith" or "unknown" |
| Seller situation | Yes | Pre-foreclosure, probate, absentee, tired landlord, tax delinquent, code violation, vacant, divorce, estate, inherited, downsizing, relocating |
| Communication channel | Helpful | Letter, text, voicemail, email, cold call, or "all" |
| Tone preference | Optional | Professional, friendly, direct, compassionate |
| Batch mode? | Optional | Single or list of multiple properties |

If seller situation is unknown, ask — it drives the entire tone. If truly unknown, default to a general "exploring options" approach.

### Step 1: Research the Situation (if address provided)

**Search queries:**
- `"{full address}" property records owner`
- `"{full address}" foreclosure` (if pre-foreclosure)
- `"{full address}" tax delinquent`
- `"{county} {state} probate records` (if probate)
- `"{full address}" code violation {city}`

**Context to gather:** owner name, ownership length, liens/judgments/lis pendens, estimated equity, last sale, motivation indicators.

### Step 2: Determine Outreach Angle

| Situation | Tone | Core Angle | Avoid |
|-----------|------|-----------|-------|
| Pre-foreclosure | Urgent but compassionate | "I may be able to help you avoid foreclosure and protect your credit" | Using the word "foreclosure" in letters/texts — use "situation with your property" |
| Probate / Inherited | Respectful, patient | "I understand you inherited a property and may not want the burden of managing it" | Appearing opportunistic; never mention "profiting from their loss" |
| Absentee Owner | Casual, opportunity-focused | "I'm interested in properties in this neighborhood. Would you consider selling?" | Assuming they want to sell |
| Tired Landlord | Empathetic, relatable | "Being a landlord isn't always what people expect. If you're ready for a break, I buy as-is" | Belittling their situation |
| Tax Delinquent | Helpful, informational | "I help property owners resolve tax situations. There may be options you haven't considered" | Threatening language or pressure |
| Code Violations | Neighborly, solution-oriented | "I work with properties in this area and I'm happy to take it off your hands as-is" | Mentioning specific violations (feels like surveillance) |
| Vacant | Curious, low-pressure | "I drive through this neighborhood regularly and noticed your property appears unoccupied" | |
| Divorce | Sensitive, practical | "I work with families going through transitions who need to sell quickly and fairly" | Mentioning "divorce" unless they do first |
| General / Unknown | Friendly, exploratory | "I'm an active buyer in your area — would you ever consider selling?" | |

### Step 3: Draft Physical Letter

**Professional Letter:**

```
[Investor Name — from saved preferences]
[Company Name]
[Phone] | [Email]

[Date]

[Owner Name / "Property Owner"]
[Owner Mailing Address]

RE: [Property Address]

Dear [Owner Name / "Property Owner"],

[OPENING — 1–2 sentences acknowledging their situation without presuming. Create connection.]

[BODY — Who you are and how you help. Include credibility marker from saved preferences.]

[VALUE PROPOSITION:]
• Buy as-is — no repairs or cleaning needed
• Flexible closing timeline — we close when it works for you
• No commissions or fees
• Handle all paperwork
• Cash or creative terms available

[SOFT CTA — "If you'd ever like to explore your options, I'm a quick phone call away."]

Sincerely,
[Investor Name — from saved preferences]
[Phone]
[Email]

P.S. [Strongest hook — e.g., "Even if you're not ready now, I'm happy to give you a no-obligation estimate of what your property is worth today."]
```

**Handwritten-Style / Yellow Letter:**

```
[Date]

Hi [Owner Name / "Neighbor"],

My name is [Name from preferences] and I've been looking at properties in [neighborhood/area].

I came across your property at [address] and wanted to reach out personally to see if you'd ever consider selling.

I buy properties in any condition and work on YOUR timeline. No pressure — if you're curious, give me a call or text at [phone from preferences].

Thanks,
[Name]
[Phone]
```

### Step 4: Draft Text Message / SMS

**First touch:**
```
Hi [Name], this is [Investor Name — preferences]. I was looking at properties in [neighborhood] and came across [address]. Would you ever consider an offer on it? No pressure — just thought I'd ask.
```

**Follow-up #1 (3–5 days, no response):**
```
Hey [Name], circling back on [address]. I know you're busy — just wanted to see if selling is something you'd ever consider. I buy in any condition and close on your timeline.
```

**Follow-up #2 (7–10 days, still no response):**
```
Hi [Name], last follow-up on [address]. If timing isn't right, totally understand. If anything changes, feel free to reach out — my number's always open. Have a great week!
```

**Response templates:**
- If interested: "Great to hear from you! Would it be easier to chat on the phone for a few minutes, or should I send over some info first?"
- If not interested: "Totally understand. Appreciate you letting me know. If anything changes, don't hesitate to reach out."

### Step 5: Draft Voicemail Script (30–45 seconds)

```
"Hi [Name], this is [Investor Name — preferences], a local real estate buyer in [city/area].
I'm reaching out because I was looking at properties in your neighborhood and came across [address].
I wanted to see if you'd ever consider selling. I buy in any condition and work on whatever timeline works for you.
No pressure — just thought it was worth a conversation.
If you're open to chatting, you can reach me at [phone from preferences]. That's [repeat phone].
Thanks, [Name]. Have a great [day/evening]."
```

### Step 6: Draft Email

```
Subject: Quick question about [Property Address]

Hi [Name],

My name is [Investor Name — preferences] and I'm a real estate buyer active in [city/area].

I came across your property at [address] and wanted to reach out to see if you'd ever consider selling — now or in the future.

How I work:
• I buy properties as-is (no repairs or cleaning needed)
• I cover all closing costs
• Flexible timeline — we close when it works for YOU
• No realtor commissions or hidden fees
• Cash or creative financing terms available

[Situation-specific paragraph — tailor to their circumstance]

If you're even a little curious about your options, I'm happy to have a no-obligation conversation.

Reply here, call or text me at [phone from preferences], or let me know a convenient time.

Best,
[Investor Name]
[Phone]
[Email]
[Website — from preferences]
```

### Step 7: Draft Cold Call Script

```
OPENING (first 10 seconds):
"Hi, is this [Name]? Hey [Name], this is [Investor Name — preferences].
I'm calling about your property at [address] — do you have just a minute?"

RAPPORT (15–20 seconds):
"I appreciate you taking my call. I'm a [local investor / real estate buyer] in [city]
and I came across [address]. I just wanted to see if selling is something you've ever thought about."

[LISTEN. Their response tells you everything.]

QUALIFY (ask — don't pitch):
"How long have you owned the property?"
"Are you living there or is it [rented/vacant]?"
"Is there anything about the property that's been a headache?"
"Have you thought about what you'd want for it?"
"Is there a timeline you're working with?"

TRANSITION (if interest shown):
"Based on what you're telling me, I think there might be a way I can help.
Here's what I typically do — I'd take a look at the property, run the numbers,
and make you a fair offer. No obligation, no pressure."

CLOSE (always end with a specific next step):
"Would it make sense for me to come take a look this [day]?
Or I can put together a preliminary offer based on what I know and send it over."

OBJECTION HANDLING:
- "Need to think about it" → "Of course. Can I check back with you [specific day]?"
- "Working with a realtor" → "No problem! If that doesn't work out, keep my number. I can close faster."
- "Offer too low" → "I understand. What number would work for you? Let me see if I can make it work."
- "Not interested" → "Got it. Is it the timing, or you just don't want to sell? [If timing:] May I check back in a few months?"
```

### Step 8: Save to Cloud Brain

Save an outreach record per property for follow-up tracking:

```
Path: brain/seller-outreach/{address-slug}-{YYYY-MM-DD}.md
Tool: mcp__cloud-brain__write_note
Content includes: seller name, situation, channels drafted, date, follow-up schedule
```

---

## Batch Mode

For multiple properties, generate a subfolder structure:

```
Output — seller outreach batch {date}:
  brain/seller-outreach/batch-{date}/{address-1-slug}/
  brain/seller-outreach/batch-{date}/{address-2-slug}/
  ...
```

For batches over 20 properties, process in groups of 10 and confirm between groups.

---

## Error Handling

- **No property address:** Ask for at least street address and city/state.
- **No seller situation:** Ask before proceeding — drives the entire tone.
- **Owner name unknown:** Use "Property Owner" throughout; note that including the name significantly increases response rate.
- **No investor contact info:** Load from preferences or ask; required for all materials.
- **WebSearch unavailable:** Proceed with user-provided data; note research was skipped.
- **Outreach already exists for address:** Ask whether to overwrite or create a follow-up sequence.
- **Sensitive situation + request for aggressive language:** Decline and explain — empathy consistently outperforms pressure for distressed sellers.
- **User asks to actually send messages:** Clarify this skill drafts only. For batch texting, suggest platforms like BatchLeads or Launch Control.
