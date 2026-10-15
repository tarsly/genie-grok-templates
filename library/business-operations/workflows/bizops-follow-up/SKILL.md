---
name: bizops-follow-up
description: >-
  Track follow-ups and commitments — add, list, complete, and get reminders for promises, deadlines, open loops, pending items, accountability checks, overdue tasks, or needs to log a commitment, check what's overdue, or see everything they're waiting on from others.
---

# Follow-Up Tracker — Never Drop a Ball

## Overview

Follow-Up Tracker is your AI accountability system. Every promise you make, every commitment you take on, every "I'll get back to you" — it gets logged here. You can add follow-ups, list all pending items, mark them done, and get reminded when things are due or overdue. No more lying awake at 2am wondering "Did I send John that proposal?" It's tracked. It's managed. Nothing falls through the cracks. This is the single most valuable habit in business: doing what you said you'd do, every single time.

## When This Skill Applies

- User says "follow up" or "follow-up" or "remind me to"
- User says "I need to" or "I promised" or "don't let me forget"
- User says "track this" or "add a follow-up" or "add to my follow-ups"
- User says "what follow-ups do I have" or "what's pending" or "what's overdue"
- User says "mark as done" or "completed follow-up" or "close the loop"
- User says "what did I promise" or "who am I waiting on" or "open loops"
- User says "accountability check" or "what do I owe people"
- User triggers the `/follow-up` command
- The morning briefing skill references follow-ups
- Any conversation where the user makes a commitment to someone

## Category

Never Drop a Ball

## Data Structure

All follow-ups are stored in a single Cloud Brain note titled `Follow-Up Tracker` in the `pipeline` folder.

Use `mcp__cloud-brain__search_notes` with query "follow-up tracker" to find it, then `read_note` to get the full content.

The note uses this format:

```markdown
# Follow-Up Tracker
> **Last Updated:** [YYYY-MM-DD]
> **Open Items:** [count]
> **Overdue:** [count]

---

## Pending Follow-Ups

| # | Due | Person | Commitment | Context | Priority | Added |
|---|-----|--------|-----------|---------|----------|-------|
| 1 | 2026-03-05 | John Smith | Send proposal for Oak Street deal | Discussed on Monday call | High | 2026-03-03 |
| 2 | 2026-03-07 | Sarah Chen | Share Q1 investor update | Monthly update, usually by first week | Medium | 2026-03-01 |
| 3 | 2026-03-10 | Self | Renew business insurance | Policy expires March 15 | High | 2026-02-28 |

## Waiting On (Others Owe You)

| # | Expected | Person | What They Owe | Context | Last Nudge | Added |
|---|----------|--------|--------------|---------|------------|-------|
| 1 | 2026-03-04 | Marcus (CPA) | Q4 tax estimates | Sent all docs Feb 25 | 2026-03-01 | 2026-02-25 |
| 2 | 2026-03-06 | Designer | Website mockups v2 | Revision requested Feb 28 | — | 2026-02-28 |

## Completed (Last 30 Days)

| # | Completed | Person | Commitment | Original Due |
|---|-----------|--------|-----------|-------------|
| 1 | 2026-03-02 | Tom Bradley | Sent investment commitment letter | 2026-03-01 |
| 2 | 2026-03-01 | Self | Filed LLC annual report | 2026-02-28 |

---
*Use /follow-up add, /follow-up list, /follow-up done, or /follow-up remind*
```


## Pre-Flight — Follow-Up Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops follow-up preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Default follow-up reminder window (e.g., 3 days before due date)
   - Overdue threshold (how many days past due before flagging 🚨)
   - Default priority when none specified (high / medium / low)
   - Save to Cloud Brain: `write_note` → title: `bizops-followup-preferences`, folder: `brain/preferences`
4. Apply throughout: use saved thresholds for all overdue and reminder calculations
5. Show banner at top of every output:
   ```
   🔁 Follow-Up Tracker | Overdue threshold: {X} days | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my follow-up preferences' to change settings."*

## How It Works

### Action: ADD — Add a New Follow-Up

**Trigger:** User says "remind me to...", "I need to...", "follow up with...", "add a follow-up", or provides details about a commitment.

**Process:**
1. Parse the commitment details:
   - **Who** — Person involved (or "Self" for personal items)
   - **What** — The specific commitment or action
   - **When** — Due date (parse natural language: "by Friday", "next week", "March 10", "in 3 days")
   - **Context** — Why this matters, background info
   - **Priority** — High (affects revenue, relationships, or deadlines), Medium (important but flexible), Low (nice to have)
   - **Direction** — "Pending" (you owe someone) or "Waiting On" (someone owes you)

2. Use `search_notes` with query "follow-up tracker" to find the existing tracker note, then `read_note` to get its content. If it does not exist, create it with `write_note` using the template above.

3. Add the new item to the correct table (Pending or Waiting On)

4. Update the header counts (Open Items, Overdue)

5. Save the updated note using `write_note` with title `Follow-Up Tracker`, folder `pipeline`, tags `["follow-up", "pipeline", "accountability"]`

6. Confirm: "Follow-up added: [summary]. Due: [date]. I'll flag this if it gets close."

**Natural language parsing examples:**
- "Remind me to send John the proposal by Friday" → Person: John, What: Send proposal, Due: this Friday, Priority: High
- "I told Sarah I'd have the numbers by next week" → Person: Sarah, What: Send the numbers, Due: next Monday, Priority: Medium
- "Marcus is supposed to get me the tax docs by March 10" → Waiting On: Marcus, What: Tax docs, Expected: March 10
- "Follow up with the designer in 3 days" → Person: Designer, What: Follow up on status, Due: 3 days from today

### Action: LIST — Show All Pending Follow-Ups

**Trigger:** User says "what follow-ups do I have", "what's pending", "show my follow-ups", "open loops", "what did I promise"

**Process:**
1. Use `search_notes` with query "follow-up tracker", then `read_note` to get the full content
2. Sort by due date (most urgent first)
3. Flag overdue items with a warning
4. Display in clean format:

```
FOLLOW-UP STATUS — [Today's Date]

OVERDUE (Action Required)
  [Person] — [What] — was due [date] ([X days ago])
  [Person] — [What] — was due [date] ([X days ago])

DUE TODAY
  [Person] — [What]
  [Person] — [What]

DUE THIS WEEK
  [Date] — [Person] — [What]
  [Date] — [Person] — [What]

DUE LATER
  [Date] — [Person] — [What]

WAITING ON OTHERS
  [Person] — [What they owe] — expected [date]
   Last nudge: [date or "never"]

Open: [X] items | Overdue: [X] items | Waiting on: [X] items
```

### Action: DONE — Mark a Follow-Up Complete

**Trigger:** User says "done", "completed", "mark as done", "I sent the...", "close the loop on..."

**Process:**
1. Identify which follow-up was completed (by person name, description, or number)
2. If ambiguous, show the list and ask which one
3. Move the item from "Pending" (or "Waiting On") to "Completed (Last 30 Days)"
4. Add completion date
5. Update header counts
6. Save the updated note using `write_note`
7. Confirm: "Follow-up completed: [summary]. [X] items remaining."
8. Clean up: Remove completed items older than 30 days from the Completed section

### Action: REMIND — Get a Summary of What's Due

**Trigger:** User says "remind me", "what's due", "accountability check", "what's overdue"

**Process:**
1. Use `search_notes` with query "follow-up tracker", then `read_note` to get the content
2. Categorize items: Overdue, Due Today, Due This Week
3. For overdue items, suggest specific actions:
   - "This is [X] days overdue. Want me to draft a message to [person]?"
   - "This keeps slipping. Should we reschedule or delegate?"
4. For "Waiting On" items that are overdue, suggest nudge messages:
   - "[Person] is [X] days late on [item]. Want me to draft a follow-up message?"

### Action: NUDGE — Draft a Follow-Up Message

**Trigger:** User says "nudge [person]", "send a reminder to [person]", "follow up with [person] about [item]"

**Process:**
1. Find the relevant follow-up item
2. Draft a professional but direct follow-up message (email or text style)
3. Tone: Respectful, brief, with a clear ask and deadline
4. Present the draft for user approval

## Automatic Behaviors

These happen without the user explicitly asking:

1. **Morning Briefing Integration:** When `/morning-briefing` runs, it searches Cloud Brain for the follow-up tracker and includes any items due today or overdue in the briefing.

2. **Brain Dump Detection:** When the user does a brain dump that includes a commitment ("I told John I'd..."), automatically suggest adding it as a follow-up.

3. **Meeting-to-Actions Integration:** When processing meeting notes, any action items assigned to the user get suggested as follow-ups.

4. **Overdue Escalation:** If an item is overdue by more than 3 days and hasn't been addressed, flag it with increasing urgency in any status check or briefing.

## Error Handling

- **If the follow-up tracker note does not exist in Cloud Brain:** Create it with `write_note` using the template header and empty tables. This is first-time setup — do it silently and confirm: "Follow-up tracker initialized. Your first item has been added."
- **If the note exists but is empty or malformed:** Recreate the structure and preserve any data that can be parsed. Warn: "Follow-up tracker was malformed — I've restructured it and preserved what I could."
- **If the user says "done" but the item isn't found:** Show the full list and ask: "I couldn't find that exact item. Which of these did you complete?" Present numbered list for easy selection.
- **If a due date is in the past when adding:** Ask: "That date is already past. Did you mean [next occurrence]? Or should I add it as already overdue?"
- **If the user adds a duplicate:** Check for similar items (same person + similar description). If found: "This looks similar to an existing follow-up: [item]. Want me to update that one instead of creating a duplicate?"
- **If no due date is provided:** Default to 3 business days from today and confirm: "No due date specified — I've set it for [date]. Adjust?"
- **If the tracker has more than 50 pending items:** Flag: "You have 50+ open follow-ups. That's a lot of open loops. Want me to help you triage — close stale items, batch similar ones, or identify what can be delegated?"

## Example Usage

**User:** "Follow up — remind me to send Tom the partnership agreement by Thursday. Also, I'm waiting on Sarah for the market analysis she promised last week."

**AI:**
1. Parses two items: one Pending (Tom, agreement, Thursday), one Waiting On (Sarah, market analysis, already overdue)
2. Reads or creates the follow-up tracker note in Cloud Brain
3. Adds both items to their respective tables
4. Confirms:

```
Two follow-ups added:

1. PENDING: Send Tom the partnership agreement
   Due: Thursday, March 6 | Priority: High

2. WAITING ON: Sarah — market analysis
   Expected: last week (OVERDUE)
   Want me to draft a nudge message to Sarah?

Open items: 7 | Overdue: 2
```

## Cost Estimate

| Scenario | Estimated Cost |
|----------|---------------|
| Add a follow-up | $0.02–$0.05 |
| List all follow-ups | $0.02–$0.05 |
| Mark as done | $0.02–$0.05 |
| Full remind/triage with nudge drafts | $0.05–$0.15 |

## What Makes This Premium

The number one way professionals destroy trust is by forgetting what they promised. "I'll send that over tomorrow" — and then they don't. "Let me get back to you on that" — and silence. A $180K/year executive assistant's primary job is making sure the boss never drops a ball. Every promise tracked, every deadline flagged, every commitment honored. That's what this skill does. It's not a to-do list — it's an accountability engine that protects your reputation, one kept promise at a time.
