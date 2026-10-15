---
name: 1031-exchange-tracker
description: >-
  Mission-critical deadline tracking and logistics management for tax-deferred real estate exchanges. Capture exchange setup, calculate compliance deadlines (Day 45 identification, Day 180 close), track identified replacement properties, generate status dashboards, create alert messages, and educate clients on 1031 rules. Missing a deadline costs clients massive capital gains taxes — this skill keeps everything on track. Use whenever a client is doing a 1031 exchange, asks about like-kind exchanges, needs to track replacement property deadlines, or you need to generate identification letters or QI communications. Triggers on: 1031, tax deferred exchange, like-kind exchange, replacement property deadline, qualified intermediary, QI, exchange tracker, 1031 deadline, relinquished property.
---

> **⚠️ Disclaimer:** A 1031 exchange is a complex tax-deferral strategy governed by IRC § 1031 and IRS regulations. This skill is a professional workflow and deadline-tracking tool for real estate agents — it does NOT constitute tax advice, legal advice, or accounting advice. The rules, deadlines, and guidelines described here are general and your client's specific situation may vary. Missing deadlines, improper QI selection, or incorrect property identification can result in immediate and substantial tax liability. Clients MUST work with a licensed CPA and a real estate attorney before initiating any 1031 exchange. Always refer clients to their tax and legal professionals for all compliance decisions — do not rely on this skill alone for tax-sensitive guidance.

# 1031 Exchange Tracker

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
    Preferred QI: [qi-name, phone, email]
    Default Identification Rule: [3-Property / 200% / 95%]
    Updated: [date]
```

**Setup interview questions:**
1. Your full name?
2. Brokerage name?
3. Primary market / city?
4. Preferred Qualified Intermediary (QI) contact(s) — name, phone, email — optional, can add later?
5. Default identification rule you typically recommend to clients (3-Property Rule / 200% Rule / 95% Rule)?

Show a preferences banner at the top of every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

This skill helps you manage one of the most stressful and deadline-driven processes in real estate. A 1031 tax-deferred exchange can save clients hundreds of thousands in capital gains taxes — but **missing a single deadline erases that benefit**. You need a reliable system to track every date, every property, and every compliance rule.

The stakes are high: a 45-day miss means the exchange dies, full capital gains taxes are owed immediately, and your client faces a sudden six-figure tax bill. This skill keeps you and your clients compliant, on time, and stress-free.

---

## EXCHANGE SETUP: Capture the Foundation

When a client's relinquished property closes (the sale), immediately capture these details:

**Ask the client for:**
- Full name and contact email
- Relinquished property address (what they're selling)
- Sale close date — this is **Day 0, your anchor date for all deadlines**
- Sale price and net proceeds after costs
- Qualified Intermediary name, phone, and email
- Exchange type: standard forward (most common), reverse (buying before selling), or build/improve (replacement property will be constructed)

**Why this matters:** The close date is the legal trigger. Every deadline in a 1031 flows from this single date. Misidentify it by one day and your client could miss the deadline.

Save each exchange to memory:
```
mcp__cloud-brain__write_note:
  path: brain/transactions/1031-[client-name-slug]-[YYYY-MM-DD].md
  content: [exchange record with all captured details, deadlines, and identified properties]
```

Read at the start of each session:
```
mcp__cloud-brain__search_notes: "brain/transactions 1031"
```

Auto-populate agent name, brokerage, and preferred QI contacts from preferences in all status reports and communication drafts.

---

## CRITICAL DEADLINE TRACKING: The Non-Negotiable Rules

Calculate and clearly display these dates from Day 0 (close date):

### Day 45: Identification Deadline (HARD STOP)
By midnight of Day 45, your client must formally identify which replacement property(ies) they're buying. This deadline is **not negotiable — there is no extension**. The IRS does not grant waivers.

**What happens if you miss it:**
- The exchange terminates automatically
- Your client is treated as having sold without reinvesting
- Full capital gains taxes are owed on the entire sale price within 90 days
- The tax bill arrives before your client can even catch their breath

### Day 180: Exchange Close Deadline (HARD STOP)
By midnight of Day 180, your client must close on the replacement property. Money must transfer, deed must record — no exceptions.

**Critical:** If Day 180 falls on a weekend or holiday, the deadline is the last business day before. Closing on Monday after a Friday deadline costs the exchange.

**What happens if you miss it:**
- Same as above: exchange fails, full capital gains taxes owed
- Your client loses the benefit of reinvesting proceeds at their own pace

### Days 46+: The Failure Zone
If Day 45 passes without identification, the exchange is dead. Your client is liable for full capital gains taxes. A qualified CPA and tax attorney must be notified immediately to minimize damage and set up tax payment planning.

---

## IDENTIFICATION MANAGEMENT: Track the Replacement Properties

Create a log of candidate replacement properties with:
- Property address
- List price
- Your target purchase price
- Property type (similar to relinquished property per IRS rules)
- Status (under contract, pending, identified, expired)

### The Three Rules — Help Your Client Choose One

Your client must pick one identification strategy and stick to it. Default recommendation from preferences is auto-suggested.

**1. Three-Property Rule (Most Flexible)**
Identify any 3 properties, any price. No limit on how much lower than the relinquished sale price. Must close on at least one.

**2. 200% Rule (Medium Flexibility)**
Identify unlimited properties, but the total market value cannot exceed 200% of the relinquished property's sale price.

**3. 95% Rule (Most Conservative)**
Identify unlimited properties, but must reinvest at least 95% of the net proceeds into identified properties. Useful when replacing a high-value property with multiple smaller properties.

### Flag Violations
- If your client is under the 3-property rule but has identified 4+ properties: FLAG IT — they've broken the rule and the entire exchange is at risk
- If 200% rule: sum the list prices of all identified properties; if total exceeds 200% of sale price, FLAG IT
- If 95% rule: monitor contract prices; if total purchase prices fall below 95% of proceeds, FLAG IT

### Generate the Identification Letter
The QI (Qualified Intermediary) must formally document the identification. Create a template letter showing:
- Relinquished property and sale price
- Date of identification
- List of identified replacement properties (addresses, descriptions, market values)
- Rule being used (3-property, 200%, or 95%)
- Client signature and date
- Agent name and brokerage from preferences

The QI will file this with the IRS as proof of compliance.

---

## STATUS DASHBOARD: Your Daily Copilot

Generate a clean, scannable status report whenever requested. Auto-populated with agent name, brokerage, and preferred QI contacts from preferences.

```
═══════════════════════════════════════════════════════════════
                    1031 EXCHANGE STATUS
Client: [Name]                  Exchange Type: [Forward/Reverse/Build]
Relinquished Property: [Address]
Sale Close Date: [Date]         Sale Price: $[Amount]
Agent: [Name] | [Brokerage]
═══════════════════════════════════════════════════════════════

TIMELINE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Days Elapsed:                   XX days since close
Days Until Identification:      XX days remaining (DEADLINE: [Date])
Days Until Close:               XX days remaining (DEADLINE: [Date])

EXCHANGE STATUS: [ON TRACK | AT RISK | CRITICAL | FAILED]

IDENTIFICATION PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Rule Selected:                  3-Property Rule
Properties Identified:          2 of 3 allowed
- [Address 1] - Under contract at $XXX,XXX (closes [Date])
- [Address 2] - Pending offer (asking $XXX,XXX)

QUALIFIED INTERMEDIARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Name: [QI Name from preferences or client-provided]
Funds Held:                     $[Amount]
Contact:                        [Phone / Email]

NEXT ACTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ ] Formal identification letter filed with QI
[ ] Replacement property under contract
[ ] Title company engaged
[ ] Final inspection complete
```

---

## ALERTS & REMINDERS: Stay Ahead of Deadlines

Generate contextual reminder language at these trigger points. All messages auto-signed with agent name and brokerage from preferences.

**Day 20 Alert:** "Your identification deadline is 25 days away (Day 45: [Date]). If you haven't shortlisted replacement properties yet, now is the time. Contact your agent with at least 3-5 candidate addresses so we can move forward with offers."

**Day 30 Alert:** "30 days to identification deadline. You need to decide which property(ies) you're buying. We should have preliminary offers in and be in active negotiations by now."

**Day 35 Alert:** "IMPORTANT: Only 10 days until identification deadline (Day 45: [Date]). You must formally identify your replacement property(ies) by this date. Have you selected which property? Contact [QI name] immediately if not."

**Day 44 Alert:** "⚠️ URGENT: 24 HOURS to identification deadline. This is not a suggestion — missing midnight on [Date] terminates the entire exchange and triggers capital gains taxes. If you haven't identified yet, contact your QI immediately."

**Day 150 Alert:** "30 days to exchange close deadline (Day 180: [Date]). Confirm your replacement property is under contract and moving toward closing. Any delays now put the entire exchange at risk."

**Day 170 Alert:** "⚠️ CRITICAL: 10 days to exchange close deadline (Day 180: [Date]). Title must be clear, inspection must be complete, and final walkthrough must be done. Coordinate with your title company and lender daily. Any delays forfeit the exchange."

---

## EDUCATION MODULE: Explain 1031s in Plain English

When a client says "What is a 1031?" or asks a rule question, provide this explanation:

**What is a 1031 Exchange?**
A 1031 exchange is a legal structure that lets you sell an investment property and reinvest the proceeds into another investment property **without paying capital gains taxes on the sale**. Instead of writing a check to the IRS for 15-20% of your gain (plus state taxes), you keep all the proceeds working for you in real estate.

Example: You sell a rental property for $500K. Your gain is $200K. Without a 1031, you'd owe ~$40K in federal taxes. With a 1031, you can reinvest the full $500K into new properties and defer all those taxes.

**Why it matters:**
Taxes are your client's biggest expense when selling real estate. A 1031 can preserve $50K–$200K+ depending on the property value and your state's tax rate. That's money that stays in your client's portfolio and compounds over time.

**The key rules (memorize these):**
1. You have 45 days from the close of sale to identify which property(ies) you're buying
2. You have 180 days from the close of sale to close on the replacement property
3. The replacement property must be "like-kind" (IRS term) — meaning real property for real property (rental for rental, commercial for commercial, land for land, etc.). A property for personal use does not qualify
4. A Qualified Intermediary (QI) must hold the proceeds from sale; you cannot touch the money or the exchange dies
5. You must reinvest **all** net proceeds (or satisfy one of the three identification rules to reinvest less)

**Common mistakes that kill exchanges:**
- Missing the Day 45 identification deadline
- Missing the Day 180 close deadline
- Touching the sale proceeds before closing on replacement property (even to pay closing costs)
- Not using a QI (self-dealing means the IRS rejects the entire exchange)
- Identifying more than 3 properties and trying to close on one outside those 3
- Buying a property that's not "like-kind" (e.g., raw land to buy a rental house works, but raw land to buy a gas station may not)

**When to bring in your team:**
- Have a CPA review the exchange structure **before** you sell; they spot issues a real estate agent can't
- Have a real estate attorney draft your purchase agreement with 1031 language (the agreement must allow time for 1031 compliance)
- Keep your QI in the loop at every step; they're your compliance guardian

**The one thing most agents get wrong:**
They think "like-kind" is narrower than it is. The IRS allows a LOT of flexibility: rental house to commercial building, apartment to office space, one property to multiple properties. Your CPA or attorney will advise what works for your client's goal.

---

## WORKFLOW: How to Use This Skill

1. **At Close of Sale:** Capture all exchange setup details (client, property, QI, dates) and save to memory
2. **Every Day:** Monitor days remaining to both deadlines
3. **Weekly:** Ask for identification updates and flag rule violations
4. **At Day 35:** Generate reminder alerts to client and your team
5. **At Day 45 Close:** Confirm identification letter filed with QI
6. **Daily After Day 45:** Monitor replacement property contract and closing progress
7. **At Day 180 Close:** Confirm exchange is complete; send congratulations and tax planning reminder

---

## Pro Tips from High-Volume Agents

- **Set phone reminders** for Day 35 and Day 44. Missing these is the most common failure point
- **Get the QI's direct number** and call weekly. They track compliance tighter than you do
- **Never let your client touch the sale proceeds.** It kills the exchange instantly
- **Have a backup property identified by Day 40.** First-choice deals fall through; you need a Plan B
- **Close the replacement property 2–3 days before Day 180 if possible.** Recording delays can push you past the deadline
- **Communicate early and often.** A client who knows the rules won't panic and miss a deadline

---

## Questions for Your Client

Use these to confirm they understand what's ahead:

- "Do you understand that Day 45 is a hard stop with no extensions? If we miss it, the exchange dies and you owe full capital gains taxes."
- "Are you ready to move fast on identifying properties? Day 45 comes quickly."
- "Have you worked with a 1031 before? Should we schedule a call with a CPA to walk through the rules?"
- "Are you clear that the Qualified Intermediary holds the proceeds, and you cannot touch them? This is non-negotiable."
- "Which identification rule makes sense for you: 3-property, 200%, or 95%?"

---

**Bottom line:** A 1031 exchange is a powerful tax tool that most agents under-utilize. But it demands obsessive attention to deadlines. Use this skill as your daily checklist, your client's peace of mind, and your compliance safety net. The stakes are too high for guesswork.

## See Also

- `/tax-cost-segregation` — cost seg on the replacement property — interacts with 1031 basis carryover (from `tax-and-audit`)
- `/tax-quarterly-estimate` — fold 1031 gain deferral into the quarterly estimate (from `tax-and-audit`)
- `/disclosure-compliance-tracker` — disclosure for the relinquished property sale (same plugin)
