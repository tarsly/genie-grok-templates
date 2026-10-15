---
name: comm-meeting-actions
description: >-
  Use when the user says '/meeting-actions', 'extract action items from my notes', 'convert this transcript to tasks', or pastes meeting notes and needs them turned into a structured task list. Outputs action items with owners and due dates, decisions made, follow-ups needed, and a shareable summary — faster than the full Meeting Transcript skill when the goal is just a clean task list.
---

# COMM-Meeting-Actions
## Structured Action Item Extraction

---

## Overview

Paste meeting notes or a transcript and get structured action items, decisions made, follow-up assignments, and owner mapping extracted automatically. Faster and more focused than the full Meeting-Transcript skill — purpose-built for converting notes into a task list.

**No regulated disclaimer required for this skill.**

---

## Pre-Flight

No preferences layer needed for this skill. Every input changes every run. Proceed directly to job inputs.

---

## Job Inputs

Ask at the start of each run:
- Paste your meeting notes or transcript
- Who were the key people in the meeting? (names to use when assigning owners)
- Do you want just action items, or also decisions and follow-ups?

Do not save these answers to Cloud Brain.

---

## Processing Logic

Scan the notes for every commitment, task, or next step. Apply these rules:

**Explicit commitments** — Statements by a named person about what they will do. Assign owner by name.

**Implied tasks** — Things that clearly need to happen but weren't assigned to anyone. Flag owner as "[UNASSIGNED — assign before sharing]".

**Date-bound items** — Any task with a deadline mentioned. Extract the date exactly as stated.

**Dependent tasks** — If one action item blocks another, note the dependency.

---

## Output Format

### Action Items — [Meeting Date or Topic]

#### 📋 Action Items
| # | Action | Owner | Due | Notes |
|---|---|---|---|---|
| 1 | [what needs to happen] | [person] | [date] | [any conditions or context] |

#### ✅ Decisions Made
- [Decision] — agreed by [person/group]

#### 🔄 Follow-Ups Needed
- [Item] → [who needs to follow up with whom] by [when]

#### ⚠️ Unassigned Items
List any tasks with no clear owner — these need to be assigned before the list is sent.

---

### Shareable Summary
A clean, formatted version ready to paste into Slack, email, or a project tool:

```
ACTION ITEMS — [Meeting / Date]

✅ [Owner]: [Action] — due [date]
✅ [Owner]: [Action] — due [date]

DECISIONS:
• [Decision]

FOLLOW-UPS:
• [Item] — [owner] to [action]
```

---

## Memory

This skill does not save to Cloud Brain by default. If action items need to be tracked over time, route them through `bizops-follow-up` instead.

Offer at the end of each run: "Want me to add any of these to your Follow-Up tracker?"

---

## Error Handling

- **Ambiguous ownership:** Flag as "[UNASSIGNED]" rather than guessing
- **No dates mentioned:** Use "ASAP" or "per team agreement" rather than fabricating a deadline
- **Conflicting commitments:** Flag them: "⚠️ [Person] agreed to both X and Y — verify these are both on their radar"
- **Very short or vague notes:** Work with what's provided and note: "⚠️ Notes were brief — verify these action items are complete before sharing"
