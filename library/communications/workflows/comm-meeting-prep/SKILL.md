---
name: comm-meeting-prep
description: >-
  Use when the user says '/meeting-prep', 'prep me for my meeting with [name]', 'I have a call with [person]', or needs to walk into any meeting fully prepared. Generates a complete brief: attendee research, structured agenda, talking points, anticipated objections with responses, questions to ask, and a ready-to-send follow-up template.
---

# COMM-Meeting-Prep
## Pre-Meeting Intelligence Brief

---

## Overview

This skill generates a complete pre-meeting brief — attendee research, agenda, talking points, anticipated objections with responses, and a follow-up template — so the user walks into every meeting prepared.

**No regulated disclaimer required for this skill.**

---

## Pre-Flight — Client Preferences

1. Search Cloud Brain: `search_notes` with query "comm preferences"
2. If found: confirm in ONE line — "Prepping as [Name] at [Business], correct?"
   - If yes: proceed immediately
   - If no: ask what changed, update, save, proceed
3. If not found: collect setup in ONE message:
   - Your name, title, and business
   - Your primary role in meetings (seller / buyer / consultant / employee / investor / other)
   - Preferred meeting structure (agenda-first / rapport-first / depends on meeting type)
   - Default follow-up style (formal email / casual check-in / action items list)
4. Save to `brain/preferences/comm-preferences.md`
5. Show a one-line preferences banner at top of output

---

## Job Inputs

Ask at the start of each run:
- Who is this meeting with? (name, title, company)
- What's the purpose of this meeting? (sales call, negotiation, strategy session, interview, intro call, etc.)
- What do you want to walk away with? (a signed deal, an agreement, information, a next step, etc.)
- Date and duration of the meeting
- Anything you already know about this person or situation?

Do not save these answers to Cloud Brain.

---

## Output Structure

### 1. Attendee Intelligence
Research the person and company (use web search if available):
- Name, title, company, LinkedIn summary
- Company size, industry, recent news or announcements
- Known priorities or pain points relevant to this meeting
- Any mutual connections or shared context
- Tone/communication style (if inferable from public presence)

If research is unavailable: provide a structured list of questions the user should ask their own network before the meeting.

### 2. Meeting Agenda
Structured agenda for the time available:
```
[Duration] Meeting Agenda — [Date]
With: [Name], [Title] at [Company]

:00 — Opening / Rapport (X min)
     [Suggested rapport openers based on research]

:XX — [Main Topic 1] (X min)
     [What to cover, what you want to learn or convey]

:XX — [Main Topic 2] (X min)

:XX — Objections / Concerns (X min)

:XX — Close / Next Steps (X min)
     [Your desired outcome and how to ask for it]
```

### 3. Talking Points
3–5 key messages to land in this meeting:
- Each with: the point → why it matters to them → how to phrase it naturally

### 4. Anticipated Objections
For each likely pushback:
```
OBJECTION: "[likely objection]"
RESPONSE: "[how to acknowledge and redirect]"
```

### 5. Questions to Ask
5–8 open-ended questions to understand their situation, uncover needs, or build rapport — in the order they'd naturally arise.

### 6. Follow-Up Template
Ready-to-send follow-up email for after the meeting (fill in actuals after):
```
Subject: [subject]

[Personalized opener referencing the conversation]

[Summary of what was discussed / agreed]

[Next steps — who does what by when]

[Warm close]
```

---

## Memory — Save Meeting Brief

Offer to save the brief to Cloud Brain after generating it:

**Path:** `brain/communications/meetings/prep-[person-slug]-[YYYY-MM-DD].md`

Ask: "Want me to save this prep brief? If you run Meeting-Transcript or Meeting-Actions after the meeting, I'll pull it back up to compare what was planned vs. what actually happened."

---

## Error Handling

- **Unknown attendee:** Skip the research section, expand the "questions to ask your network" section, and note: "I couldn't find public information on [Name] — here's what to learn before the meeting."
- **Multiple attendees:** Profile the most senior or most relevant attendee in depth; list others with basic details
- **Very short meeting (15 min or less):** Compress to: 1 core message, 2 questions, 1 CTA

## See Also

- `/brief-pre-meeting` — full calendar-triggered pre-meeting brief that includes 30-day public activity (from `person-brief`)
- `/brief-pre-call` — sales-call-specific pre-call brief (from `person-brief`)
- `/brief-person-30days` — research a single attendee's last-30-days activity (from `person-brief`)
