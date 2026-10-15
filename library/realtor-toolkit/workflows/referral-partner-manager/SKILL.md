---
name: referral-partner-manager
description: >-
  Manage your referral network like a top producer. Track who sent what leads, automate relationship maintenance, calculate referral fees owed, flag cold relationships, and generate personalized thank-you messages. 30-50 percent of top agent business comes from referrals — this skill ensures none fall through the cracks. Use for logging new referrals, generating weekly relationship maintenance lists, calculating referral fees at close, drafting thank-you messages to referral partners, and running referral performance reports. Triggers on: referral, referral partner, referral fee, who sent me leads, thank you for the referral, referral tracking, referral network, touch base with partners, relationship maintenance.
---

# Referral Partner Manager

> **Disclaimer:** Referral fee arrangements are subject to state real estate law and brokerage policy. Some states restrict or prohibit referral fees paid to unlicensed parties. Always verify your referral compensation structure with your broker and ensure all arrangements are disclosed and documented in writing. This skill is a professional tracking tool — not legal or financial advice.

You manage a referral network that generates 30–50% of your business. This skill tracks everything: who sent you leads, conversion rates, fees owed, and relationship warmth.

---

## STEP 0: PREFERENCES

Check memory for saved agent preferences first.

```
mcp__cloud-brain__search_notes: "realtor-toolkit preferences"
```

**If preferences found:** Confirm and continue — "Using: [Agent Name], [Brokerage]. Proceed or update?"

**If not found:** Run setup interview:

> Quick setup — I'll save this so you never have to re-enter it.
> 1. Your full name?
> 2. Brokerage name?
> 3. Your market / primary city?
> 4. Default referral fee % (what you typically pay partners)? (Common: 25% of your commission)
> 5. Preferred payment method (ACH, check, wire)?
> 6. Your timezone?

Save to memory:
```
mcp__cloud-brain__write_note:
  path: brain/preferences/realtor-toolkit-preferences.md
```

Show preferences banner on every output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent: [Name] | [Brokerage] | [Market]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CORE OPERATIONS

Read the referral partners file at the start of every session:
```
mcp__cloud-brain__read_note: brain/referrals/referral-partners.md
```

Initialize if it doesn't exist. Save all updates back to `brain/referrals/referral-partners.md`.

### 1. LOG A NEW REFERRAL
When you say "I got a referral from [partner name]":
- Extract: partner name, lead name/contact, date received, current lead status
- Update partner's `last_contact` to today
- Increment partner's `leads_referred` count
- Flag new partners for immediate thank-you after first referral

**Output:** "Logged referral from [name] — [lead name]. Updated last contact. Status: [status]."

### 2. LOG REFERRAL CLOSE & CALCULATE FEE
When a referred deal closes:
- Mark lead as "Closed"
- Calculate fee using partner's arrangement (% of sale price or flat)
- Mark fee as "Unpaid" — generate payment reminder if unpaid after 30 days
- Default to preferences fee % if no specific arrangement exists

**Output:** "Deal closed. Referral fee owed to [partner]: $[amount] ([%] of $[sale price]). Marked unpaid."

### 3. GENERATE THANK-YOU MESSAGES
Three templates by deal stage — all auto-populated with agent name from preferences:

**New referral received:**
> Thanks for thinking of me with [Lead Name] — exactly the type of property we love. I'll keep you posted. — [Agent Name], [Brokerage]

**Under contract:**
> Quick update on [Lead Name]'s property — we're in contract, close expected [Month]. Thanks for the connection. — [Agent Name]

**Closed:**
> Deal closed on [Address] — $[price] on [date]. Referral fee of $[amount] being processed via [method]. Your referrals are invaluable. — [Agent Name], [Brokerage]

### 4. RELATIONSHIP MAINTENANCE
Categorize partners by contact recency:
- **Hot** = contacted within 30 days
- **Warm** = 31–60 days
- **Cold** = 61+ days

Weekly maintenance list: Cold partners first, then Warm, then recent referrers needing thanks.

### 5. QUARTERLY TOUCH-BASE MESSAGES
For Cold/Warm partners — value-add check-ins with no referral ask:
- Share a market insight relevant to their industry
- Ask about their business (genuine interest)
- Reference something specific about them or their work

### 6. REFERRAL PERFORMANCE REPORT
- Total referrals received (by partner)
- Conversion rate (closed / total)
- Total GCI from referrals
- Top 5 partners by volume
- Dormant partners (no referrals in 6+ months)
- Fee summary: owed / paid / outstanding

### 7. REFERRAL FEE TRACKER
- Fees owed per partner
- Payment status (paid / unpaid)
- Days outstanding (auto-flag at 30+ days)
- Payment method used

---

## MEMORY FORMAT

All data at `brain/referrals/referral-partners.md`:

```markdown
# Referral Partners
Agent: [Name] | [Brokerage]
Last Updated: [YYYY-MM-DD]
Default Fee: [X]% | Payment Method: [method] | Timezone: [tz]

## Partners

### [Partner Name]
Company: [company]
Role: [mortgage_lender / attorney / past_client / etc.]
Email: [email] | Phone: [phone]
Last Contact: [YYYY-MM-DD]
Warmth: [Hot / Warm / Cold]
Fee Arrangement: [X]% of sale price
Notes: [any notes]

**Referrals:**
- [Lead Name] | Received: [date] | Status: [Active/Closed/Lost]
  - Sale Price: $[X] | Close Date: [date]
  - Fee: $[amount] | Paid: [Yes/No] | Payment Date: [date]
```

---

## USAGE EXAMPLES

- "I got a referral from Sarah Chen for a buyer named the Patels." → Log referral, update Sarah's last contact.
- "The Johnson deal just closed for $480k." → Mark closed, calculate fee, log payment reminder.
- "Who should I call this week?" → Weekly maintenance list, Cold partners first.
- "What's my referral performance this quarter?" → Full report with GCI, conversion rate, top partners.
- "Send thank-yous to Q1 referrers." → Personalized messages ready to copy/send.
