---
name: comm-meeting-transcript
description: >-
  Use when the user says '/meeting-transcript', 'process this transcript', 'debrief my meeting', or pastes a recording summary or rough notes and needs a full structured output. Extracts decisions, action items, follow-ups, open questions, and key insights into a clean debrief — and cross-references with any Meeting Prep brief saved for that meeting.
---

# COMM-Meeting-Transcript
## Meeting Transcript Processing

---

## Overview

This skill processes meeting recordings, transcripts, or rough notes — extracting decisions, action items, follow-ups, and key insights into a clean, structured debrief. Your AI meeting analyst that never misses a detail.

**No regulated disclaimer required for this skill.**

---

## Pre-Flight — Client Preferences

This is primarily a job-input skill. The only identity preference worth saving is output format. Keep the preferences layer minimal.

1. Search Cloud Brain: `search_notes` with query "comm preferences"
2. If found: check for saved output format preference only — apply it silently
3. If not found: no setup needed — proceed to job inputs

---

## Job Inputs

Ask at the start of each run:
- Paste the transcript, recording summary, or meeting notes below
- Meeting context (optional but helpful): who attended, what was the meeting about, what decisions were expected?
- Output format preference for this run: full debrief / action items only / executive summary

Do not save these answers to Cloud Brain.

---

## Processing Logic

Work through the transcript systematically. Identify and extract:

**Decisions Made** — Any explicit agreement, approval, or resolution reached during the meeting. Look for: "we'll go with," "decided to," "agreed that," "moving forward with."

**Action Items** — Any commitment to do something. Look for: "I'll," "you'll," "we need to," "by [date]," "before the next call," "someone should."

**Follow-Ups** — Items that need more information, a second conversation, or an external response. Look for: "let me check on," "get back to you," "pending," "waiting on."

**Open Questions** — Issues raised but not resolved. Items that remain ambiguous or need a decision later.

**Key Insights** — Important context, revelations, or signals that came up. Things worth remembering for the relationship or the project.

---

## Output Format

### Meeting Debrief — [Date]
**Meeting:** [Subject/purpose if provided]
**Attendees:** [List if provided]
**Duration:** [If known]

---

#### ✅ Decisions Made
| Decision | Owner | Notes |
|---|---|---|
| [decision] | [person who owns it] | [any conditions] |

#### 📋 Action Items
| Action | Owner | Due |
|---|---|---|
| [what needs to happen] | [who] | [by when] |

#### 🔄 Follow-Ups
| Item | Who to follow up with | By when |
|---|---|---|
| [item] | [person] | [timeframe] |

#### ❓ Open Questions
- [question] — [why it matters / what's blocking it]

#### 💡 Key Insights
- [insight or signal worth remembering]

---

### Executive Summary (optional add-on)
3–5 sentence plain-language summary of the meeting: what was covered, what was decided, and what happens next. Useful for sharing with someone who wasn't there.

---

## Cross-Reference with Meeting Prep

If a meeting prep brief exists in Cloud Brain for this meeting, pull it and compare:
- Were the planned talking points covered?
- Was the desired outcome achieved?
- What was different from what was expected?

Surface this comparison as a brief "Planned vs. Actual" section.

---

## Memory — Save Debrief

After processing, save to Cloud Brain:

**Path:** `brain/communications/meetings/debrief-[meeting-slug]-[YYYY-MM-DD].md`

Include: decisions, action items, and key insights. These are searchable for future reference — when the user meets with the same person again or returns to the same project, prior debriefs will surface automatically.

---

## Error Handling

- **Incomplete or rough transcript:** Work with what's provided — flag gaps with "⚠️ [Section unclear — verify with attendees]"
- **No speaker labels:** Note it, extract actions as best as possible, flag owner fields as "TBD — verify who owns this"
- **Very long transcript (90+ min):** Process in sections; surface the most important 5 action items first before the full breakdown
- **No transcript provided:** Ask the user to paste the content, or offer to work from bullet-point notes instead
