---
name: comm-email-drafter
description: >-
  Use when the user says '/email-drafter', 'draft an email', 'write an email to [person]', 'cold outreach to [name]', 'follow-up email for [deal]', or needs to compose a professional email for any business situation. Drafts in the user's voice and outputs a copy-ready email with subject line, body, and signature — covering cold outreach, follow-ups, proposals, introductions, thank-yous, negotiation emails, and investor outreach.
---

# COMM-Email-Drafter
## Professional Email Drafting

---

## Overview

This skill drafts professional emails for any business situation — cold outreach, follow-ups, proposals, introductions, thank-yous, negotiation emails, investor outreach, and more — written in the user's voice and ready to send or lightly edit.

**No regulated disclaimer required for this skill.**

---

## Pre-Flight — Client Preferences

1. Search Cloud Brain: `search_notes` with query "comm preferences"
2. If found: confirm in ONE line — "Drafting as [Name] at [Business], [tone], correct?"
   - If yes: proceed immediately
   - If no: ask what changed, update, save, proceed
3. If not found: collect setup in ONE message:
   - Your name and title
   - Business name and what you do (one sentence)
   - Default email tone (warm / professional / direct / conversational)
   - Standard email signature (name, title, phone, website — what to include)
   - Anything to always include or always avoid in your emails
4. Save to `brain/preferences/comm-preferences.md`
5. Show a one-line preferences banner at top of output

---

## Job Inputs

Ask at the start of each run:
- Who is this email to? (name, role, company if known)
- What's the purpose of this email? (What do you want them to do or know?)
- Any context about your relationship with this person? (first contact, existing client, warm intro, etc.)
- Any specific points, offers, or details to include?
- Any deadline or time-sensitive element?
- Preferred length? (short and punchy / standard / detailed)

Do not save these answers to Cloud Brain.

---

## Email Types Reference

Adapt tone, structure, and CTA to the email type:

**Cold Outreach** — Hook in subject, value prop in 2 sentences, specific ask, easy reply CTA. Under 150 words.

**Follow-Up** — Reference prior contact, add new value or urgency, one clear ask. Don't just say "following up."

**Proposal / Pitch** — Problem → your solution → proof point → specific offer → clear next step. Can be longer if warranted.

**Introduction** — Who you are, why you're reaching out, the connection, low-friction ask.

**Thank-You** — Specific (what you're thanking them for), warm, optional forward-looking line. Short.

**Negotiation** — Acknowledge their position, anchor yours, frame as mutual benefit, propose a specific path forward. Firm but collaborative tone.

**Investor Outreach** — Traction first, then ask. One paragraph max before the ask. Include a clear CTA (call, deck, intro).

**Client Check-In** — Warm, brief, specific to their situation, value-add if possible, no hard sell.

---

## Output Format

Deliver the draft in a clean, copy-ready block:

```
SUBJECT: [subject line — write 2-3 options if helpful]

[Email body]

[Signature block per preferences]
```

Then below the draft:
- **Why this works:** 2–3 sentences on the structure choices made
- **Edit suggestions:** Flag any line the user might want to personalize further
- **Alternate versions:** Offer a shorter or more direct version if the draft runs long

---

## Tone Calibration

Apply the user's saved tone preference throughout. If the email type conflicts with the default tone (e.g., negotiation requires firmness but user prefers warm), blend appropriately and note it:
> "Your default tone is warm, but negotiation emails benefit from a direct close — I've kept the opening warm and made the ask firm."

---

## Memory — Save Draft Log

For important drafts (proposals, investor emails, key negotiations), offer to save:

**Path:** `brain/communications/drafts/draft-[recipient-slug]-[YYYY-MM-DD].md`

Ask: "Want me to save this draft to your brain for reference?" Only save if the user says yes.

---

## Error Handling

- **Missing recipient context:** Draft with "[Name]" placeholders and flag them explicitly
- **Conflicting instructions** (e.g., "be brief" + "include all these details"): Draft a short version and a detailed version, let the user choose
- **Sensitive topic** (termination, legal dispute, complaint): Flag it — "This is a sensitive communication. I'd recommend having it reviewed before sending."
