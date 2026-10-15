---
name: comm-zoom-digest
description: >-
  Weekly Zoom meeting digest — automatically pulls all recordings from the past week (or a specified period), summarizes each meeting, surfaces unresolved action items that should have been completed, flags recurring themes across meetings, and delivers a clean executive digest. Designed to be scheduled every Monday morning so you start the week knowing exactly what happened and what's still open. Triggers on: 'weekly meeting digest', 'summarize my meetings this week', 'what happened in my meetings last week', 'meeting digest', 'weekly recap of my calls', 'what are my open action items from meetings', 'zoom digest', 'pull my meeting history for the week', 'review all my meetings from last week'.
---

# COMM-ZOOM-DIGEST — Weekly Meeting Digest

> **Requires:** Zoom MCP connector. Designed to be scheduled weekly — recommend Monday 7:00 AM.
> **Best paired with:** bizops-daily-brief (surfaces open items), comm-zoom-debrief (deeper dive on any meeting).

---

## Overview

Start every week knowing exactly what happened and what's still unfinished. This skill pulls all cloud recordings from the past week, generates a summary of each, cross-references Cloud Brain to identify action items that should have been completed, flags recurring themes that suggest bigger issues or opportunities, and delivers a clean weekly digest — including a prioritized list of open loops you're carrying into the new week.

Scheduled weekly, this becomes an automatic accountability layer: you never lose a commitment, a follow-up, or a strategic thread buried in a recording.

---

## When This Skill Applies

- User says "weekly meeting digest" or "zoom digest"
- User says "what happened in my meetings last week?"
- User says "summarize all my meetings from this week"
- User says "what are my open action items from recent meetings?"
- User says "review my Zoom recordings for the week"
- **Scheduled run:** every Monday morning automatically

---

## Pre-Flight — User Preferences

Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `"zoom digest preferences"`.

**If preferences ARE found:** Confirm in one line — *"Running your weekly digest for [user name] — [date range]."*

**If NOT found:** Collect once before proceeding:

1. Your name (for identifying your action items vs. others')
2. Preferred digest time window (default: past 7 days — Mon–Sun)
3. Preferred digest depth: **Brief** (one paragraph per meeting) or **Detailed** (full decisions + action items per meeting)?
4. Should the digest be saved as a Zoom Doc? Yes / No
5. Which categories of meetings should be excluded from the digest, if any? (e.g., internal team standups, personal 1:1s)

Save to `brain/preferences/zoom-digest-preferences.md`.

---

## Step 1: Determine the Digest Period

Default: past 7 days (last Monday through last Sunday).

If the user specifies a different period ("this month", "last two weeks", specific dates), use that range instead.

Set `from` and `to` dates accordingly in UTC format.

---

## Step 2: Pull All Recordings

Call `mcp__zoom__recordings_list` with:
- `from`: start of the digest period (YYYY-MM-DD)
- `to`: end of the digest period (YYYY-MM-DD)
- `page_size`: 50 (capture all meetings in the period)

This returns all cloud recordings the user hosted during the period.

**If no recordings found:** Report — *"No cloud recordings found for [date range]. Either no meetings were recorded, or recordings are stored locally rather than in the cloud. You may need to enable cloud recording in Zoom settings."*

**Filter out excluded categories** (if the user set exclusion preferences): skip meetings whose topics match excluded patterns.

---

## Step 3: Summarize Each Meeting

For each recording, call `mcp__zoom__get_meeting_assets` to get the AI summary, participants, and any Zoom-generated next steps.

If the brief summary from the AI is insufficient for the user's preferred depth setting, also call `mcp__zoom__get_recording_resource` with `types: "summary,nextStep"` to get topic-level summaries and Zoom's next steps list.

For each meeting, extract:
- **Meeting title and date**
- **Attendees** (external contacts flagged separately from internal)
- **What it was about** (1–3 sentences depending on depth preference)
- **Key decisions made** (if any)
- **Action items generated** (owner + task + deadline)
- **Follow-ups outstanding** (what needs to happen next)

---

## Step 4: Cross-Reference Brain for Unresolved Items

This is the accountability layer. Check whether action items from previous weeks' digests were actually completed.

Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `"zoom digest"` to find prior weekly digest notes.

For each prior digest found (look back 2–4 weeks):
- Read the action items that were listed as open
- Check if any of those same items appear to have been resolved (look for completion signals in this week's meeting summaries or in other brain notes)
- Items that were open last week and haven't appeared as resolved = **Carried Over Open Items**

Flag these prominently in the digest — they are the highest priority output.

---

## Step 5: Identify Cross-Meeting Themes

Scan across all meeting summaries from the week and identify:

**Recurring topics:** Any subject that came up in 2+ meetings. These often signal a priority that deserves focused attention.

**Recurring people:** Any external person who appeared in multiple meetings this week. This might indicate an important relationship that's heating up.

**Unresolved patterns:** Action items from this week that duplicate action items from prior weeks — suggesting chronic follow-through issues on a topic.

**Escalation signals:** Any topic where the language escalated week over week (e.g., "looking into it" → "still pending" → "urgent").

---

## Step 6: Build the Digest

Assemble the complete weekly digest:

```markdown
# Weekly Meeting Digest — Week of [Mon Date] to [Sun Date]
*Generated [Today's Date] | [N] meetings | [N] total recordings*

---

## ⚠️ Open Items Carried Over (Resolve These First)
[Items from prior weeks that are still unresolved]

| Item | Source Meeting | Originally Assigned | Weeks Open |
|------|---------------|---------------------|------------|
| [Action item] | [Meeting name, date] | [Owner] | [N] weeks |

---

## Your Action Items This Week
[All action items assigned to the user across all this week's meetings]

| # | Action | From Meeting | Deadline |
|---|--------|-------------|----------|
| 1 | [Task] | [Meeting name] | [Date/TBD] |
| 2 | [Task] | [Meeting name] | [Date/TBD] |

---

## Weekly Meeting Summaries

### 1. [Meeting Topic] — [Date]
**Attendees:** [Names] ([N] people)  
**Summary:** [1–3 sentences about what happened]  
**Decisions:** [Key decisions, or "None recorded"]  
**Action items:** [N items — list or "None"]  
**Follow-up needed:** [Yes/No — and what]

### 2. [Meeting Topic] — [Date]
...

[Continue for all meetings]

---

## Cross-Meeting Themes
[Recurring topics, people, or patterns worth noting]

- **[Theme 1]:** [Brief observation — appeared in [Meeting A], [Meeting B]]
- **[Theme 2]:** [Brief observation]

---

## Stats for the Week
- Total meetings with recordings: [N]
- External meetings: [N]
- Internal meetings: [N]
- Total action items generated: [N]
- Action items assigned to you: [N]
- Open items carried over from prior weeks: [N]

---

*Digest generated by COMM-zoom-digest. Data source: Zoom cloud recordings [date range].*
```

---

## Step 7: Save to Cloud Brain and Optionally Create Zoom Doc

**Save to Cloud Brain:**
- Tool: `mcp__cloud-brain__write_note`
- Path: `brain/digests/zoom-digest-[YYYY-MM-DD].md`
- Content: full digest

**Create Zoom Doc (if user preference = Yes):**
Call `mcp__zoom__create_new_file_with_markdown`:
- `file_name`: "Weekly Meeting Digest — Week of [Mon Date]"
- `content`: full digest markdown

Provide the Zoom Doc link to the user.

---

## Step 8: Deliver the Digest

Present the digest and close with:

*"[N] meetings this week. [N] action items for you, [N] open loops carried over from prior weeks. Anything from this week's meetings you want to dive deeper on with a full debrief?"*

---

## Scheduling Note

This skill is designed to run automatically every Monday morning before the user's workday starts.

Recommended schedule: **Every Monday at 7:00 AM** (user's timezone).

When running as a scheduled task, the skill runs silently and saves the digest to Cloud Brain and/or Zoom Docs per preferences. No interactive questions — all inputs are read from saved preferences. If preferences haven't been saved yet, the scheduled run will alert the user to run the skill manually once to set them up.

---

## Error Handling

- **No cloud recordings in the period:** Report clearly and suggest enabling cloud recording if the user wasn't aware it was off.
- **Zoom AI summary unavailable for a meeting:** Use whatever data is available (recording metadata, title, duration, participants). Flag: "AI summary not available for this meeting — Zoom AI Companion may not have been enabled."
- **Large number of meetings (10+):** Generate full digest but offer a "top 5 most important" view based on meeting duration, number of attendees, and whether action items were generated.
- **Preferences not saved (scheduled run):** Skip interactive questions. Note at the top of the digest: "Default preferences used — run COMM-zoom-digest manually to customize your digest settings."
- **Prior digest not found in Brain (first run):** Skip the open items cross-reference. Note: "This is your first digest — no prior weeks to cross-reference. Open items from this week will be tracked in future digests."
