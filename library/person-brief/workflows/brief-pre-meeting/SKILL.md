---
name: brief-pre-meeting
description: >-
  Generate a complete pre-meeting briefing package — attendee research, agenda recap, shared history from Cloud Brain, last 30 days of public activity per attendee, recommended talking points, and a 'don't forget' list. Designed to run as a calendar-triggered agent the morning of every important meeting.
---

# Pre-Meeting Brief

## Overview

A meeting-prep orchestrator that walks into every meeting prepared. Pulls from three sources: (1) Cloud Brain history with each attendee (prior notes, deals, context), (2) the **brief-person-30days** skill for each external attendee, (3) any meeting agenda or Zoom event the user provides. Output is a single brief saved to Cloud Brain under `calendar/`, with a chat-rendered summary that fits on one screen.

This is the skill to bind to a Reclaim.ai habit or a calendar webhook so it runs 30 minutes before every meeting automatically.

## When This Skill Applies

- User has a meeting coming up and asks for prep ("brief me for my 10am", "prep me for the Acme call", "what do I need to know about tomorrow's meeting with...")
- User pastes a calendar invite or Zoom link and says "prep this"
- Daily brief / morning routine that includes "today's meetings" — run this for each one
- User mentions a meeting by attendee name or company and wants context
- User says "pre-meeting", "meeting prep", "before the call", "intel on today's meeting"

## Pre-Flight — Preferences

1. Search Cloud Brain: `search_notes` with query `"pre-meeting preferences"` (folder: `brain/preferences`)
2. **If found:** Load, render banner, proceed
3. **If not found:** Ask in ONE message:
   - Default depth (quick / standard / deep)
   - Auto-include public activity (last 30 days) for external attendees? (yes / no / ask)
   - Auto-save briefs to Cloud Brain? (yes / no)
   - Default meeting types to skip (1:1s with team, recurring standups, etc.)
   - Save to Cloud Brain: `write_note` → title: `pre-meeting-preferences`, folder: `brain/preferences`
4. Banner:
   ```
   🎯 Pre-Meeting | Depth: {depth} | 30-day intel: {on/off/ask} | Auto-save: {on/off}
   ```

## How It Works

### Step 1: Resolve the Meeting

User input forms:
- Meeting title or topic ("Acme call")
- Specific time ("10am tomorrow")
- Calendar invite paste (parse subject, time, attendees, agenda)
- Zoom link (if `zoom-meetings` plugin is installed, detected via `mcp__zoom__*` tools, use it to pull metadata; otherwise parse the link manually for ID + topic)
- Attendee names ("meeting with Sarah and Marcus")

Output of this step: structured meeting object — `{title, time, attendees: [{name, email, company?}], agenda?}`.

If insufficient info: ask once. Never guess at attendee identity.

### Step 2: Pull Cloud Brain History

For each attendee:
1. `search_notes` for their name in `people/` → contact note (if any)
2. `search_notes` for their name across `pipeline/`, `projects/`, `research/people/`, `transcripts/`
3. `recent_activity` filter on their name → last 30 days of notes mentioning them
4. Pull any prior meeting notes from `calendar/` mentioning them

Extract: relationship status, deal stage (if CRM hit), last interaction date, open commitments, prior discussion threads.

### Step 3: Pull 30-Day Public Activity (External Attendees)

For each attendee whose email domain is NOT the user's own company (or whose contact note tags them external):

- Invoke the **brief-person-30days** skill OR run the same source sweep inline (per preferences)
- Cap at top 3 themes per person, top 5 evidence links each

Skip this for internal attendees and recurring 1:1s.

### Step 4: Synthesize

Generate the brief with these sections:

1. **Meeting at a Glance** — time, attendees, agenda, expected outcome
2. **Attendees** — for each: relationship status, last interaction, deal stage if applicable, 30-day public signal
3. **Shared History** — top 5 prior touchpoints from Cloud Brain
4. **Open Items** — commitments outstanding (from follow-ups, prior meeting notes, pipeline)
5. **Suggested Agenda Additions** — based on shared history + recent signal, what should be on the agenda that isn't
6. **Talking Points** — 3-5 specific things to bring up that demonstrate you're current
7. **Don't Forget** — small reminders: "Mary is doing Whole30 right now, no food gifts"; "Sarah mentioned her kid's graduation last call"

### Step 5: Save and Render

- `write_note` → title: `Meeting Brief — {meeting title} — {YYYY-MM-DD}`, folder: `calendar`, tags: `["meeting-brief", "{date}"]`
- Render condensed version in chat (see Output Format)
- If the `communications` plugin is installed (detected via Cloud Brain `brain/preferences/communications-preferences` note existing), append to the output: `See also: /comm-meeting-prep for deeper Cloud-Brain-history-only briefs (from communications)`. Otherwise omit the line.

## Data Structure

```markdown
# Meeting Brief — {Title}

> **Time:** {YYYY-MM-DD HH:MM TZ}
> **Generated:** {YYYY-MM-DD HH:MM}
> **Depth:** {level}

## At a Glance

- **Topic:** {topic}
- **Duration:** {minutes}
- **Attendees:** {list}
- **Agenda (provided):** {agenda or "—"}
- **Expected outcome:** {if user supplied, else "—"}

## Attendees

### {Name 1} — {Role}, {Company}
- **Relationship:** {new / known / deep, last contact {date}}
- **CRM Status:** {stage if in pipeline, deal value, last note date}
- **Cloud Brain notes:** {count} touchpoints, most recent {date}
- **Last 30 days (public):**
  - {Theme 1} — {1-line summary with date + 1 link}
  - {Theme 2} — {1-line summary}
- **Notes:** {anything specific from contact note: family, interests, no-fly topics}

### {Name 2} ...

## Shared History (Last 90 Days)

| Date | Type | Summary |
|------|------|---------|
| {date} | {meeting/email/call/note} | {1-line} |
| ... |

## Open Items / Commitments

- [{date}] {what was promised} — {by whom} — {status}
- ...

## Suggested Agenda Additions

1. {item — why it matters}
2. {item}

## Talking Points

1. {specific thing to bring up — references the 30-day signal}
2. {specific thing}
3. {specific thing}

## Don't Forget

- {personal detail / preference / sensitivity}
- {logistics: where to meet, what to bring}
- {follow-up commitment from last time}
```

## Output Format (Chat)

```
🎯 MEETING BRIEF — {Title}
{Time} • {Duration}min • {N} attendees

ATTENDEES (signal in last 30 days)
• {Name} — {1-line headline of what they're up to}
• {Name} — {1-line headline}

OPEN ITEMS (from last interaction)
1. {commitment}
2. {commitment}

TOP 3 TALKING POINTS
1. {point}
2. {point}
3. {point}

DON'T FORGET
- {detail}
- {detail}

Full brief: calendar/Meeting Brief — {title} — {date}
```

## Example Usage

**User:** "Prep me for my 10am with Acme AI tomorrow. Sarah Chen and Marcus Lee will be there."

**AI:** Resolves meeting time. Pulls Cloud Brain notes mentioning Sarah and Marcus and Acme. Runs brief-person-30days for both (external). Detects Sarah has been posting about RAG benchmarks; Marcus just open-sourced a tool. Brief saved to `calendar/`. Chat renders condensed version.

**User:** [pastes calendar invite] "Prep this."

**AI:** Parses subject, time, attendees, agenda. Identifies external vs internal. Runs full pipeline. Brief saved.

**User:** "/brief-pre-meeting Sam Lee Q2 mastermind --depth deep"

**AI:** Pulls prior meeting notes, prior masterminds, recent YouTube/podcast appearances by the person, current real estate market intel. Deep brief with extended talking points.

**User:** "Daily brief includes 3 meetings today — run pre-meeting for each."

**AI:** Loops over the day's meetings (skipping internal recurring per preferences). Three briefs saved. Chat summary lists all 3 with key signals.

## Error Handling

- **If no attendees can be identified:** Ask once: "Who's in the meeting?" — never guess.
- **If a meeting has 5+ external attendees:** Cap full 30-day briefs at the top 3 by relationship importance (CRM stage, frequency of prior contact). Note in brief: "Full intel on top 3 attendees. Run brief-person-30days for others individually."
- **If meeting is recurring internal (1:1, standup):** Skip the public-activity step entirely. Brief focuses on open items and shared history only.
- **If Cloud Brain has zero history for any attendee:** That's fine — note "new relationship — no prior touchpoints" and lean on the 30-day public signal.
- **If 30-day public signal is empty across the board:** Brief still ships with the meeting basics + Cloud Brain history + a flag: "Public activity was low — this person may be heads-down or private."
- **If the meeting is in <15 minutes:** Drop to quick depth automatically and warn: "Less than 15 minutes — running quick brief only. Run again later for deeper context."
- **If the user already has a meeting brief saved within last 24 hours for the same attendees:** Read the existing brief, only add deltas (new public activity since brief was generated, new Cloud Brain notes). Don't rebuild from scratch.
- **If zoom-meetings plugin is installed and a Zoom link is provided:** Use it to pull historical call data with the same attendees (transcripts, prior agendas, chat logs). Detected via `mcp__zoom__*` tool visibility. If not installed, skip the historical-call pull and proceed with Cloud Brain context only.

## See Also

- `/brief-person-30days` — single-person research without meeting framing (same plugin)
- `/brief-pre-call` — sales-call-specific prep with CRM stage + objections (same plugin)
- `/comm-meeting-prep` — Cloud-Brain-history-only meeting prep (from `communications`)
- `/comm-zoom-debrief` — post-meeting debrief counterpart (from `zoom-meetings`)
