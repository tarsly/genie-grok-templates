---
name: comm-zoom-prep
description: >-
  Zoom-powered meeting prep — pulls your full history with a person or company from Zoom (prior meeting summaries, open action items, unresolved follow-ups), combines it with web research, and builds a complete pre-meeting intelligence brief: context on prior conversations, talking points, an agenda, anticipated objections, and a follow-up template ready to send. Knows what happened last time — without you having to remember. Triggers on: 'prep for my meeting with [name]', 'meeting prep', 'brief me before my call with [name]', 'I have a call with [company]', 'what should I know before I meet with [name]', 'build an agenda for [topic]', 'call prep', 'prepare talking points', 'research [person] before our meeting'.
---

# COMM-ZOOM-PREP — Zoom-Powered Pre-Meeting Intelligence Brief

> **Requires:** Zoom MCP connector for meeting history lookup. Web search for external research.

---

## Overview

Meeting prep that actually knows what happened before. This skill searches the user's Zoom meeting history for all prior interactions with a person or company, pulls the summaries and open items from those calls, and combines that institutional memory with web research to build a complete pre-meeting brief. The result: the user walks in knowing what was discussed last time, what was promised, what's still unresolved — plus fresh intelligence on the person or company.

---

## When This Skill Applies

- User says "prep me for my meeting with [name]"
- User says "I have a call with [person/company] — help me prepare"
- User says "brief me before my meeting"
- User says "what should I know before I meet with [name]?"
- User says "build an agenda for [topic/person]"
- User says "research [person] before our call"
- User says "call prep" or "meeting prep" with any person or company context
- User mentions an upcoming meeting and asks for preparation help

---

## Pre-Flight — User Preferences

Search Cloud Brain using `mcp__cloud-brain__search_notes` with query: `"meeting prep preferences"`.

**If preferences ARE found:** Confirm in one line: *"Using your saved preferences — [name], [role/business context]."*

**If NOT found:** Collect before proceeding:
1. Your name and role/title
2. Your company/business
3. Your primary goal in most meetings (e.g., close deals, build partnerships, onboard clients, negotiate contracts)
4. Your preferred meeting length and format (30 min / 60 min, Zoom / phone / in-person)

Save to `brain/preferences/meeting-prep-preferences.md`.

---

## Step 1: Gather Meeting Context

Extract from the user's request:

1. **Who:** Person's name, company name, or both
2. **Topic:** What the meeting is about
3. **Type:** First meeting or follow-up? Sales call, onboarding, negotiation, partnership, internal?
4. **When:** Date/time if provided
5. **Desired outcome:** What does the user want to walk away with?
6. **Any concerns:** Anything the user is nervous about or wants to handle carefully?

If any of these are unclear, ask before proceeding. A one-question clarification is fine — don't interrogate.

Also ask: *"What timezone are you in?"* (required for Zoom meeting search).

---

## Step 2: Search Zoom Meeting History

Look up all prior Zoom meetings with this person or company.

**Search 1 — by person's name:**
Call `mcp__zoom__search_meetings` with:
- `q`: the person's name or company name
- `from`: 1 year ago (to capture substantial history)
- Sort to get the most relevant results

**Search 2 — by topic keywords (if applicable):**
If the meeting topic is distinctive, run a second search with those keywords to catch related meetings the person wasn't in the topic field for.

**For each matching meeting found** (up to 5 most recent):
Call `mcp__zoom__get_meeting_assets` with the `meeting_uuid` to get:
- AI summary (quick recap + key points)
- Zoom AI next steps (what was supposed to happen after that meeting)
- Participants list

Extract from each past meeting:
- **What was discussed** (1–3 sentences)
- **What was decided or committed to**
- **Open items** (things that were supposed to happen but status unknown)
- **Relationship notes** (what does this person care about? Any concerns raised? Tone of the interaction?)

---

## Step 3: Check Cloud Brain for Existing Notes

Search Cloud Brain for any saved notes about this person or company:
- `mcp__cloud-brain__search_notes` with the person's name
- `mcp__cloud-brain__search_notes` with the company name

If notes exist (`mcp__cloud-brain__read_note`), extract:
- Relationship context (how do we know them, what's the history)
- Any deal or project status
- Personal preferences or communication notes
- Prior commitments made to them that aren't in Zoom

---

## Step 4: Research the Person and Company

**Web research** — use `WebSearch` and `WebFetch`:

For the person:
- LinkedIn profile (title, background, recent activity, mutual connections)
- Recent public content (articles, posts, talks — what are they thinking about right now?)
- Any news about them personally

For the company:
- Recent news (funding, acquisitions, product launches, leadership changes, layoffs)
- Revenue / company size / stage (if relevant to the meeting type)
- Competitive positioning
- Any publicly known pain points or priorities

**Research depth should match meeting type:**
- First meeting / intro → deeper research (5–7 web lookups)
- Regular client or partner → lighter (2–3 lookups, focus on recent news)
- Internal meeting → skip external research unless the topic warrants it

---

## Step 5: Build the Pre-Meeting Brief

Compile everything into a single document the user can read in 5 minutes before the meeting.

```markdown
# Meeting Brief: [Meeting Topic]
**With:** [Person Name] — [Title], [Company]  
**Date:** [Date/Time]  
**Type:** [Meeting type]  
**Goal:** [What the user wants to walk away with]

---

## Prior Meeting History (from Zoom)
[If no prior meetings: "This is your first recorded meeting with [Name]."]

### [Meeting Topic] — [Date]
- **What happened:** [1–2 sentence summary]
- **What was decided/committed:** [Key outcomes]
- **Open items from that meeting:** [Anything that was supposed to happen but outcome unknown]

### [Meeting Topic 2] — [Date]
- ...

**⚠️ Open items to address this meeting:**
- [Item 1 that was unresolved from a prior meeting]
- [Item 2]

---

## About [Person Name]
- **Role:** [Title, Company, tenure if known]
- **Background:** [1–2 sentences on who they are professionally]
- **Recent activity:** [Latest news, posts, or public moves relevant to this meeting]

## About [Company]
- **What they do:** [1–2 sentence description]
- **Recent news:** [Most relevant news item and why it matters for this meeting]
- **Size / stage:** [Relevant context]

---

## Your Agenda (Suggested)
| Time | Topic | Purpose |
|------|-------|---------|
| 0–2 min | Opening / small talk | Build rapport |
| 2–5 min | [Agenda Item 1] | [Goal] |
| 5–15 min | [Agenda Item 2] | [Goal] |
| 15–25 min | [Agenda Item 3] | [Goal] |
| 25–28 min | Next steps | Confirm commitments |
| 28–30 min | Close | |

---

## Talking Points

### Opening
- [Relevant opener based on recent news or last meeting context]
- [Reference to something from prior meeting to show you remember]

### [Topic 1]
- [Key point to make, with supporting evidence or data if available]
- [Anticipated response from them]

### [Topic 2]
- [Key point]

---

## Anticipated Objections / Difficult Questions

| They might say... | Your response |
|---|---|
| "[Likely objection 1]" | [Prepared response] |
| "[Likely objection 2]" | [Prepared response] |

---

## Open Loops to Close
[Items from prior meetings that need to be addressed or confirmed in this meeting]
- [ ] [Open item 1]
- [ ] [Open item 2]

---

## Follow-Up Template (Ready to Send After the Meeting)

> Subject: Great connecting — [Topic/Next Steps]
>
> Hi [Name],
>
> Thanks for [taking the time / the conversation] today. As discussed:
>
> - [Key decision or commitment 1]
> - [Key decision or commitment 2]
>
> My next step is [user action]. I'll [follow up / send over / have that to you] by [date].
>
> [Call to action if applicable]
>
> Best,
> [User Name]

---

*Prepared by COMM-zoom-prep. Zoom history: [N] prior meetings found.*
```

---

## Step 6: Save Brief to Cloud Brain

Save the completed brief:
- Tool: `mcp__cloud-brain__write_note`
- Path: `brain/meeting-prep/[slug]-[YYYY-MM-DD].md`

Also update (or create) the person's contact note in the brain:
- Path: `brain/contacts/[person-name].md`
- Append: most recent meeting date, topic, and any new context from the research

---

## Step 7: Present to User

Deliver the brief and offer a quick coaching check:

*"Your meeting brief is ready. Anything specific you want to workshop before the call — an objection script, a specific talking point, or how to open?"*

---

## Error Handling

- **No Zoom history found for this person:** Proceed with web research only. Note: "No prior Zoom meetings found with [Name]. This brief is based on research only."
- **Zoom search returns unrelated meetings:** Use judgment to filter. If uncertain, show the user the meeting titles found and ask which (if any) are relevant.
- **Person not found online:** Note in the brief and proceed with what's available. "Limited public information found for [Name] — the brief focuses on Zoom history and company context."
- **Meeting is internal (same company, no external research needed):** Skip external research. Focus on Zoom history and Cloud Brain context.
- **User gives very little context:** Proceed with what's available. Flag at the top of the brief: "This brief was built with limited context — [what was known]. Let me know if you want to add more."
