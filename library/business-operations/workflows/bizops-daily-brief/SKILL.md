---
name: bizops-daily-brief
description: >-
  CEO-level daily briefing — today's priorities, fires, deadlines, project status, and focus areas pulled from goals, projects, calendar, and recent activity, or wants a CEO-level morning overview of priorities, deadlines, and fires from across their goals, projects, and recent activity.
---

# Morning Briefing — CEO-Level Daily Intelligence Report

## Overview

Morning Briefing generates a structured, executive-level daily report that tells you exactly what matters today. It reads your goals, active projects, calendar, recent daily logs, and pending follow-ups to produce a single briefing that replaces the mental overhead of figuring out where to start. This is the difference between waking up and reacting vs. waking up and executing. Your AI reads your entire operating context so you don't have to.

## When This Skill Applies

- User says "morning briefing" or "brief me"
- User says "what should I focus on today" or "what's on my plate"
- User says "start my day" or "kick off my day"
- User says "daily brief" or "morning report" or "CEO briefing"
- User says "today's priorities" or "what matters today"
- User says "give me the rundown" or "daily rundown"
- User says "what do I need to know today"
- User triggers the `/morning-briefing` command
- It's the start of a work session and the user wants to orient

## Category

Clone Your Decision-Making

## Pre-Flight — Brief Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops daily brief preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Business name
   - Calendar categories or labels to exclude (e.g., personal events, self-care, family time)
   - Names of personal/family contacts whose meetings should be excluded from prep
   - Home city *(optional — only useful if a weather or calendar MCP is connected)*
   - Save to Cloud Brain: `write_note` → title: `bizops-daily-brief-preferences`, folder: `brain/preferences`
4. Apply throughout: use exclusions for all calendar filtering; use home city only if a weather/location integration is active
5. Show banner at top of every output:
   ```
   ☀️ Daily Brief | {Business Name} | {Today's Date} | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my daily brief preferences' to change settings."*

## How It Works

### Step 1: Gather Intelligence

Read the following files to build a full picture of the user's current state. Do NOT load everything at once — read in order of priority and stop early if context is sufficient.

**Required reads (via Cloud Brain MCP):**
1. Use `search_notes` for "goals" (project: "brain") then `read_note` — Current weekly priorities, monthly focus, what's slipping, decision framework
2. Use `search_notes` for "projects" (project: "brain", folder: "projects") then `read_note` — Active project dashboard, status, next actions
3. Use `recent_activity` and `search_notes` for recent dates (project: "brain", folder: "daily") — Check for yesterday's log and today's log (if it exists). Read the most recent 1-2 daily logs for continuity.

**Conditional reads (search Cloud Brain, use if found):**
4. Use `search_notes` for current month (project: "brain", folder: "calendar") — This week's plan and deadlines
5. Use `search_notes` for "follow-ups" (project: "brain", folder: "pipeline") — Any pending follow-ups due today or overdue
6. Use `search_notes` for relevant contact names (project: "brain", folder: "people") — Context for any contacts referenced in today's tasks

**Meeting intelligence reads (do this every briefing):**
7. After reading the calendar, scan today's entries for appointments, calls, or meetings with named people. Apply these filters before doing any intelligence lookup:

   **Skip entirely (do not include in briefing):**
   - Recurring personal habits: gym, exercise, scriptures, meditation, prayer, workout, or any similar self-care routine — these are assumed handled and don't need briefing space.
   - Any calendar categories or labels the user has flagged to exclude (see User Preferences step).
   - Meetings with contacts the user has designated as personal/family (see User Preferences step). Personal time is sacred and doesn't need intelligence prep.

   **For all remaining appointments with named people:**
   - Use `search_notes` with the person's name (project: "brain", folder: "people") to check for a contact note.
   - If found, use `read_note` to get the full content and surface: relationship context, investment profile, deal interests, and especially personal details — upcoming birthdays, trips they mentioned, life events, anything that helps make a genuine human connection. These small details matter.
   - If no note exists for that person, note them as "unknown contact" in the briefing and suggest running /brain-dump after the meeting.

### Step 1b: Load User Preferences

Check Cloud Brain for a preferences note before filtering any meetings or calendar items:
- Use `search_notes` with query "daily brief preferences" (project: "brain", folder: "preferences")
- If found, use `read_note` to extract:
  - **Excluded calendar categories:** Labels or calendar names to always skip (e.g., "Personal", "Family", "Team Social")
  - **Personal contacts:** Names, relationship types, or tags to exclude from meeting intelligence
  - **Home location:** Used for weather lookup if not inferred from context
  - **Custom preferences:** Any additional sections to add or remove from the briefing
- If no preferences note exists, apply these defaults:
  - Skip only recurring self-care habits (gym, meditation, prayer, etc.)
  - Include all named meetings in meeting intelligence
  - After the first briefing, suggest: "Want to customize your daily brief? Say 'update my daily brief preferences' and I'll save your settings."

### Step 2: Analyze and Prioritize

Using the Decision Framework from goals.md, analyze everything you've read and determine:

1. **The #1 thing that matters today** — The single most important task or decision
2. **Top 3 priorities** — What must get done today (not what could get done)
3. **Active fires** — Anything marked 🔥 Behind or with a deadline within 48 hours
4. **Carry-overs** — Tasks from yesterday that didn't get completed
5. **Upcoming deadlines** — Anything due in the next 7 days
6. **What's slipping** — Patterns of avoidance or repeated delays

### Step 3: Check External Context

If web search is available:
- Check weather for the user's location (relevant for outdoor meetings, commute, energy)
- Check if today is a holiday or notable date that affects business operations

If web search is not available:
- Skip this step. Note in the briefing that weather/external context was not checked.

### Step 4: Generate the Briefing

Produce the briefing in the format specified in the Output Format section below. The tone should be direct and actionable — like a chief of staff handing a CEO a one-page brief before the first meeting of the day. No pleasantries. No filler. Just what matters.

### Step 5: Save to Daily Log (Optional)

If the user wants the briefing saved:
- Use `write_note` to save to Cloud Brain (project: "brain", folder: "daily", title: "{YYYY-MM-DD}")
- Add the briefing under a `## Morning Briefing` section
- If today's log note already exists, read it first with `read_note`, then append and save with `write_note` — do not overwrite

If the user does not specify, print to terminal only. Ask: "Want me to save this to today's daily log?"

## Output Format

```
================================================================
  MORNING BRIEFING — [Day of Week], [Month DD, YYYY]
================================================================

THE ONE THING
─────────────
[Single sentence: the most important thing to accomplish today]

TODAY'S TOP 3
─────────────
1. [Priority 1 — specific, actionable]
2. [Priority 2 — specific, actionable]
3. [Priority 3 — specific, actionable]

ACTIVE FIRES
─────────────
[If any items are marked 🔥 Behind or have imminent deadlines]
- [Fire 1 — what it is, why it's urgent, what to do about it]
- [Fire 2]
(If no fires: "All clear. No emergencies.")

CARRY-OVERS FROM YESTERDAY
───────────────────────────
[Tasks from yesterday's log that weren't completed]
- [Task — original context — recommended action today]
(If none: "Clean slate. Yesterday's tasks were handled.")

DEADLINES THIS WEEK
────────────────────
| Deadline | What | Days Left |
|----------|------|-----------|
| [Date] | [Description] | [X days] |

PROJECT PULSE
─────────────
[One-line status for each active project from the dashboard]
- [Project Name] — [Status emoji] — [Next action or current blocker]

WHAT'S SLIPPING
───────────────
[Patterns of delay or avoidance identified from goals.md and recent logs]
- [Item — how long it's been slipping — suggested intervention]
(If nothing: "Everything's on track.")

FOLLOW-UPS DUE
──────────────
[Any follow-ups due today or overdue from Cloud Brain pipeline folder]
- [Person/topic — what was promised — due date]
(If none or file doesn't exist: "No pending follow-ups tracked.")

TODAY'S MEETINGS — RELATIONSHIP INTEL
──────────────────────────────────────
[Only include this section if today has qualifying appointments — non-recurring, non-OC-Calendar, non-family meetings]
For each qualifying meeting:
  👤 [Person Name] — [Time / Meeting Type]
  - Relationship: [How you know them, when you met, context]
  - Key details: [Investment interests, deal criteria, personal notes — especially personal touches like an upcoming birthday, a trip they mentioned, a milestone in their life. These details help make real connections.]
  - Open items: [Any pending action items or commitments from previous interactions]
  - Conversation angles: [1-2 natural talking points for today]
(If no qualifying meetings today: omit this section entirely — no placeholder needed)
(If a qualifying contact has no brain file: "⚠️ [Name] — no profile found. Run /brain-dump after this meeting to capture notes.")

WEATHER & CONTEXT
─────────────────
[Weather summary if available, or "External context not checked."]
[Any relevant holidays, market events, or calendar notes]

THE MINDSET
───────────
[One sentence — motivational but grounded. Tied to the user's actual
goals and current situation. Not generic inspiration. Something that
connects today's work to the bigger vision from goals.md.]
================================================================
```

## Example Usage

**User:** "Morning briefing"

**AI reads:** goals.md, projects/README.md, daily/2026-03-02.md (yesterday), calendar/2026-03.md

**AI output:**
```
================================================================
  MORNING BRIEFING — Monday, March 3, 2026
================================================================

THE ONE THING
─────────────
Complete the proposal for Meridian Consulting and send it to the client before 5pm.

TODAY'S TOP 3
─────────────
1. Finalize the Meridian proposal — client is expecting it today
2. Follow up with the 3 leads from this week's outreach campaign
3. Record and post this week's content piece (script is ready)

ACTIVE FIRES
─────────────
- Quarterly contract renewal — 🔥 Behind by 2 weeks. Client is waiting
  on updated terms. This has been pushed twice. Block 1 hour today.

CARRY-OVERS FROM YESTERDAY
───────────────────────────
- Follow up with Alex Rivera on proposal status — was on Friday's
  list, didn't happen. Do it before lunch today.

DEADLINES THIS WEEK
────────────────────
| Deadline | What | Days Left |
|----------|------|-----------|
| Mar 5 | Proposal deadline, Meridian project | 2 days |
| Mar 7 | Monthly client newsletter | 4 days |

PROJECT PULSE
─────────────
- Portfolio Redesign — 🟡 In Progress — Designer mockups under review
- Q1 Acquisitions — 🟢 On Track — 3 new leads to analyze
- Tenant Portal — 🔴 Not Started — Deprioritized until Q2
- Tax Prep — ⏸️ Paused — Waiting on Q4 receipts

WHAT'S SLIPPING
───────────────
- YouTube content: 2 weeks behind schedule. You've skipped the last
  2 planned recording sessions. Block 90 minutes Wednesday — non-negotiable.
- Investor updates: No newsletter sent since January. Erodes trust.
  Draft one this week even if it's short.

FOLLOW-UPS DUE
──────────────
- Alex Rivera — pending proposal response — due today
- Sam (accountant) — send Q4 receipts — overdue by 5 days

WEATHER & CONTEXT
─────────────────
Austin, TX: 74°F, partly cloudy. Clear this afternoon.
No holidays or major disruptions today.

THE MINDSET
───────────
You're one proposal away from closing out your best quarter yet.
Every conversation today is a step toward that. The proposal isn't
paperwork — it's the key that unlocks the relationship. Ship it.
================================================================
```

## Error Handling

- **If no goals note is found in Cloud Brain:** Warn the user: "Your goals are empty. I can't generate a meaningful briefing without knowing your priorities. Run /brain-dump to add your goals first." Generate a minimal briefing with just project status if available.
- **If no projects note is found in Cloud Brain:** Skip the Project Pulse section. Note: "No active projects tracked. Add projects to your brain to see them here."
- **If no daily logs exist yet in Cloud Brain:** Skip Carry-Overs section. Note: "No previous daily logs found. This briefing will improve as you use the system — your AI learns your patterns over time."
- **If no current month calendar note is found in Cloud Brain:** Skip the Deadlines This Week section unless deadlines are mentioned in goals. Note: "No monthly calendar found. Add a calendar note to your brain for deadline tracking."
- **If no follow-ups note is found in Cloud Brain:** Skip Follow-Ups section. Note: "No follow-up tracker found. Use /follow-up add to start tracking commitments."
- **If web search is unavailable:** Skip Weather & Context or note "External context not checked — web search unavailable." Do not let this block the rest of the briefing.
- **If today's calendar has a qualifying meeting with a named person but no brain file exists for them:** Include them in the TODAY'S MEETINGS section with a ⚠️ flag and suggest running /brain-dump after the meeting. Do not skip the section entirely — the reminder to capture notes is itself valuable.
- **If today's calendar has no qualifying meetings (all are recurring habits, excluded categories, or personal contacts):** Omit the TODAY'S MEETINGS section entirely. No placeholder needed.
- **Recurring habits (gym, exercise, scriptures, meditation, etc.):** Never appear in the briefing. They are assumed done. Do not mention them anywhere in the output.
- **Excluded calendar categories (from preferences):** Silently excluded from all briefing sections. If no preference is set, only recurring self-care habits are excluded by default.
- **Personal contacts (from preferences):** Silently excluded from TODAY'S MEETINGS intelligence. They may still appear in carry-overs or follow-ups if there's a specific open item, but never as meeting intel subjects. If no preferences are set, no contacts are excluded by default.
- **If today's daily log already has a morning briefing:** Ask: "Today's log already has a morning briefing. Want me to overwrite it with a fresh one, or just print this to terminal?"
- **If the user runs this at night or late afternoon:** Adjust the tone. Instead of "morning briefing" frame it as "end-of-day status check" or "evening review" depending on the time. The structure stays the same but the framing shifts from "what to do today" to "where do things stand."

## Cost Estimate

| Scenario | Estimated Cost |
|----------|---------------|
| Standard morning briefing (reads 4-6 files, generates report) | $0.05–$0.15 |
| Briefing with web search (weather, market data) | $0.10–$0.25 |
| Briefing saved to daily log | Same as above (minimal write cost) |

## What Makes This Premium

A human executive assistant doing this would need to:
1. Read your entire goal framework and remember what matters this quarter
2. Check every active project and know the current status
3. Review yesterday's work and identify what carried over
4. Scan your calendar and flag upcoming deadlines
5. Remember your follow-up commitments
6. Check the weather and any relevant external factors
7. Synthesize all of that into a one-page brief in under 5 minutes

That's a $180K/year chief of staff doing a 30-minute morning routine. This skill does it in seconds, every single day, without forgetting anything, without bias, and without needing coffee first.
