---
name: comm-zoom-debrief
description: >-
  Live Zoom meeting debrief — automatically pulls any meeting from Zoom by name, topic, or recency, retrieves the AI summary, transcript, and next steps, then delivers a structured debrief: decisions made, action items with owners and deadlines, follow-ups, and key insights. Saves to Cloud Brain and creates a Zoom Doc with the structured output. No pasting required. Triggers on: 'debrief my last meeting', 'process the call with [name]', 'what happened in my meeting about [topic]', 'pull up the recording from [date]', 'meeting debrief', 'summarize my Zoom call', 'extract action items from my meeting', 'post-meeting summary', 'what did we decide in the [topic] meeting'.
---

# COMM-ZOOM-DEBRIEF — Live Zoom Meeting Debrief

> **Requires:** Zoom MCP connector. All data is pulled live from Zoom — no transcript pasting needed.

---

## Overview

This skill finds any meeting in the user's Zoom account, pulls the full AI summary, transcript, and auto-generated next steps, then structures everything into a professional debrief: decisions made, action items with owners and deadlines, follow-ups needed, and key insights. The structured debrief is saved to Cloud Brain and optionally pushed back into Zoom as a Zoom Doc the whole team can reference.

This replaces the manual workflow of copying transcripts out of Zoom and pasting them into a separate tool.

---

## When This Skill Applies

- User says "debrief my last meeting"
- User says "process the call with [person/company name]"
- User says "what happened in the [topic] meeting?"
- User says "extract action items from my meeting"
- User says "pull up the recording from [date]"
- User says "post-meeting summary" or "meeting recap"
- User says "what did we decide in [meeting topic]?"
- User wants structured output from any Zoom call, whether it happened today or weeks ago

---

## Pre-Flight — User Preferences

Before locating the meeting, load any saved communication preferences from Cloud Brain.

Search Cloud Brain using `mcp__cloud-brain__search_notes` with query: `"zoom debrief preferences"`.

**If preferences ARE found:** Confirm in one line — *"Using your saved preferences — [name], output style: [style]."*

**If NO preferences found:** Ask these two quick questions before proceeding:

1. What is your name (so I can identify your action items vs. others')?
2. For action items assigned to you, would you like a default deadline added if none was stated? (e.g., "3 business days") — Yes/No

Save to Cloud Brain:
- Tool: `mcp__cloud-brain__write_note`
- Path: `brain/preferences/zoom-debrief-preferences.md`

---

## Step 1: Identify the Meeting

Determine which meeting to process from the user's request.

**Ask for user's current timezone** before searching — this is required by the Zoom search API. If the user has preferences saved that include timezone, use that. Otherwise ask: *"What timezone are you in? (e.g., America/Phoenix, America/New_York)"*

**Parse the user's request for meeting identifiers:**
- Meeting topic or keywords ("the onboarding call", "the pricing discussion")
- Person's name ("my call with Sarah")
- Date or relative time ("yesterday", "last Tuesday", "the meeting on June 10")
- Meeting ID (if provided directly)

**Search for the meeting using `mcp__zoom__search_meetings`:**
- Set `q` to the topic/keywords or person's name
- Set `from`/`to` based on the date context (if "last week," set the range accordingly)
- If "most recent" or "last meeting," set `from` to 7 days ago

**If multiple meetings match:** Display a brief list and ask the user to confirm which one:

```
I found [N] meetings that might match:
1. [Topic] — [Date] — [Duration]
2. [Topic] — [Date] — [Duration]
3. [Topic] — [Date] — [Duration]

Which meeting would you like to debrief?
```

**If no meeting matches:** Ask the user to refine — "I couldn't find a match. Can you give me more detail about the meeting topic or date?"

---

## Step 2: Pull Meeting Assets

Using the `meeting_uuid` from the search result, call `mcp__zoom__get_meeting_assets`.

This returns:
- `meeting_summary` — Zoom AI Companion's auto-generated summary (quick recap, full text, next steps)
- `my_notes` — Host/participant notes taken during the meeting
- `recording` — Cloud recording URL and processing status
- `participants` — Attendee list
- `agenda_doc` — Agenda document if one was attached

If recording is still processing (`processing: true`), note: *"The recording is still being processed by Zoom. The AI summary and notes are available — I'll use those. If you want the full transcript, try again in a few minutes."*

---

## Step 3: Pull Recording Transcript and AI Next Steps

If recording is available, call `mcp__zoom__get_recording_resource` with `types: "transcript,summary,nextStep"` to get:
- Full timestamped transcript
- Zoom's auto-generated topic summaries
- Zoom's auto-generated next steps

Use both the AI summary AND the transcript. The summary is fast; the transcript catches specifics that the summary may gloss over.

**If transcript is unavailable** (recording not enabled, still processing, or no recording for this meeting): Proceed with AI summary and notes only. Flag: *"No transcript was available — analysis is based on Zoom's AI summary and meeting notes."*

---

## Step 4: Extract Structured Output

Analyze all available data (AI summary, Zoom next steps, transcript, notes) and extract:

### Decisions Made
- What was decided (the outcome, not the discussion)
- Who made or agreed to the decision
- Context or rationale if stated

### Action Items
For each action item, capture:

| Field | What to Extract |
|---|---|
| **Action** | What needs to be done (specific, clear) |
| **Owner** | Who is responsible (name or role) |
| **Deadline** | Extract if mentioned; apply default if user has one saved; otherwise "TBD" |
| **Priority** | High / Medium / Low based on urgency signals in conversation |
| **Source** | Brief quote or context from transcript where this was stated |

**Highlight action items assigned to the user** (by name match against saved preferences). These appear first in the output under "Your Action Items."

### Follow-Ups
- Items that need further discussion
- Questions raised but not resolved
- Topics deferred to a future meeting

### Key Insights
- Strategic points worth remembering
- Notable commitments or concerns raised
- Any relationship-relevant context (what the client cares about, objections raised)

**Content to EXCLUDE:**
- Technical meeting issues ("you're on mute", "can you share your screen")
- Side conversations unrelated to the meeting topic
- Incomplete or inaudible/garbled transcript fragments

---

## Step 5: Generate the Debrief Document

Format the structured debrief:

```markdown
# Meeting Debrief: [Meeting Topic]
**Date:** YYYY-MM-DD  
**Attendees:** [Name 1], [Name 2], [Name 3]  
**Duration:** [X minutes]  
**Source:** Zoom recording — [meeting_uuid]

---

## Summary
[2–4 sentence executive summary of what the meeting was about and what was accomplished]

---

## Your Action Items
| # | Action | Deadline | Priority |
|---|--------|----------|----------|
| 1 | [Task — for the user specifically] | [Date/TBD] | High |

## All Action Items
| # | Action | Owner | Deadline | Priority |
|---|--------|-------|----------|----------|
| 1 | [Task] | [Name] | [Date/TBD] | [Level] |
| 2 | [Task] | [Name] | [Date/TBD] | [Level] |

---

## Decisions Made
| # | Decision | Made By |
|---|----------|---------|
| 1 | [What was decided] | [Person] |

---

## Follow-Ups
- [ ] [Item] — Owner: [Name] — By: [Date/Next meeting]
- [ ] [Question to resolve] — Owner: [Name]

---

## Key Insights
- [Strategic observation or important context]
- [Notable commitment, concern, or relationship insight]

---

*Processed by COMM-zoom-debrief. Source: Zoom meeting [meeting_uuid] on [date].*
```

---

## Step 6: Save to Cloud Brain

Save the structured debrief to Cloud Brain:
- Tool: `mcp__cloud-brain__write_note`
- Path: `brain/meetings/[slug-from-topic]-[YYYY-MM-DD].md`
- Include full structured debrief content

Also check if there is an active project in the brain related to this meeting's topic:
- Search Cloud Brain: `mcp__cloud-brain__search_notes` with the meeting topic keywords
- If a project note is found, append a meeting reference:
  ```
  ## Meeting Update [YYYY-MM-DD]
  - [1–2 sentence summary of relevant outcomes]
  - Action items: [N items assigned]
  ```

---

## Step 7: Create Zoom Doc (Optional)

After saving to Brain, ask: *"Want me to also save this debrief as a Zoom Doc so your team can see it?"*

If yes, call `mcp__zoom__create_new_file_with_markdown`:
- `file_name`: "Meeting Debrief — [Topic] — [YYYY-MM-DD]"
- `content`: the full structured debrief in markdown

Provide the returned `file_link` to the user.

---

## Step 8: Confirm Results

Report to the user:

```
Debrief complete: [Meeting Topic] — [Date]
Attendees: [N] people

Extracted:
✅ [N] decisions
✅ [N] action items ([X] assigned to you)
✅ [N] follow-ups
✅ [N] key insights

Saved to: brain/meetings/[slug]-[date].md
[Zoom Doc link if created]

Your next actions:
1. [User's #1 action item] — [Deadline]
2. [User's #2 action item] — [Deadline]
```

---

## Error Handling

- **Meeting not found:** Ask for more details (topic, date, attendee name). Offer to show a list of recent meetings.
- **Recording not available / Zoom AI not enabled:** Proceed with whatever data is available (summary, notes). Flag what's missing.
- **No decisions or action items found:** Report: "This meeting appears to have been informational — no clear decisions or action items were identified. Key discussion points are captured under Key Insights."
- **Transcript still processing:** Use AI summary and notes. Tell user to run the debrief again in 10–15 minutes for the full transcript.
- **Permission denied on meeting assets:** "I don't have permission to access this meeting's assets. You may need to be the host or have been granted access by the host."
