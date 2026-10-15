---
name: bizops-weekly-review
description: >-
  Weekly review — recap what got done, what slipped, assess goal progress, weekly accountability check, and plan next week's priorities, or needs to recap what got done, assess goal progress, and plan the coming week's top priorities.
---

# Weekly Review

## Overview

A structured end-of-week review process that examines what happened over the past 7 days, assesses progress against goals, identifies what slipped and why, and builds a focused plan for the next week. Reads daily logs, project files, and goals from Cloud Brain to generate a comprehensive but concise weekly review document. Saved to Cloud Brain (folder: daily) for historical reference.

## When This Skill Applies

- User asks for a weekly review or recap
- User wants to reflect on what got done and what did not
- User asks "what slipped this week?"
- User wants to plan priorities for next week
- End of week (Friday/Saturday) or start of week (Sunday/Monday) planning
- User asks for a weekly retrospective or accountability check
- User says "wrap up the week" or "plan next week"
- User wants to check weekly progress against quarterly/monthly goals


## Pre-Flight — Review Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops weekly review preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Business name and industry
   - Review day (which day of the week you do this review)
   - Business areas to cover (revenue / pipeline / operations / team / personal / all)
   - Goal tracking method (do you have goals saved in Cloud Brain, or do you want to enter them now?)
   - Save to Cloud Brain: `write_note` → title: `bizops-weekly-review-preferences`, folder: `brain/preferences`
4. Apply throughout: filter review sections to saved business areas, pull goals from Cloud Brain if available
5. Show banner at top of every output:
   ```
   📅 Weekly Review | {Business Name} | {Review Day} | Areas: {list} | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my weekly review preferences' to change settings."*

## How It Works

### Step 1: Determine the Review Period

- Default: Past 7 days (Monday through Sunday of the current or just-ended week)
- If today is Monday-Wednesday, review the prior week (Mon-Sun)
- If today is Thursday-Sunday, review the current week (Mon through today)
- User can override with a specific date range

Calculate the dates: `{start_date}` through `{end_date}`.

### Step 2: Read Daily Logs

Search Cloud Brain for daily log notes from the review period:

Use `search_notes` with keyword "daily" and look for notes in folder "daily" matching the date range. Also use `recent_activity` to find recent daily logs. For each date in the range, try `read_note` with title pattern `{YYYY-MM-DD}`.

For each day that has a log, extract:
- What was accomplished
- What was worked on
- Decisions made
- Blockers encountered
- Notes and learnings

If some days have no logs, note those gaps (they often indicate untracked work or days off).

### Step 3: Read Project Status

Use `search_notes` with keyword "project" to find project notes in Cloud Brain. Also try `read_note` with title "projects" or "project dashboard". Review any individual project notes that were updated during the review period.

For each active project, note:
- Status change from beginning to end of week
- Milestones hit or missed
- New blockers that appeared
- Progress made

### Step 4: Read Goals

Use `search_notes` with keyword "goals" to find the goals note in Cloud Brain, then `read_note` to get the full content. Extract:
- Current quarterly objectives
- Monthly focus items
- Weekly priorities (if defined for this week)

### Step 5: Assess What Got Done

Compile a list of all accomplishments from daily logs and project updates:
- Group by project or category
- Note if each item was planned (in weekly priorities) or unplanned (reactive/ad-hoc)
- Highlight wins and breakthroughs

### Step 6: Assess What Slipped

Identify items that were planned or expected but did not get done:
- Check weekly priorities from the goals note against what actually happened
- Check project milestones that were due this week
- Check any deadlines from the calendar
- For each slipped item, try to identify WHY (blocked, deprioritized, time ran out, forgotten)

### Step 7: Goal Alignment Check

For each active quarterly goal:
- Did any work this week directly advance this goal?
- If not, is that acceptable (some goals are future-quarter) or is it a warning sign?
- Calculate a rough "time allocation" — what % of the week went to which goals?

Flag:
- Goals getting zero attention that should be active
- Disproportionate time on low-priority items
- Goals that are on track vs. at risk

### Step 8: Build Next Week's Plan

Based on everything above, draft priorities for next week:

1. **Carry-forward items** — Things that slipped and still matter
2. **Goal-driven items** — Work that directly advances quarterly objectives
3. **Deadlines** — Anything due next week
4. **Quick wins** — Small items that can be knocked out to build momentum
5. **Stretch items** — Nice to do if time allows

Limit to 3-5 top priorities. More than that means nothing is truly prioritized.

### Step 9: Energy and Pattern Check

Look for patterns across the week:
- Were certain days more productive than others?
- Did reactive work (fires, interruptions) dominate?
- Is there a recurring blocker or friction point?
- Was there scope creep or context-switching?
- Any personal notes about energy, motivation, or wellbeing?

### Step 10: Save the Review

Save to Cloud Brain using `write_note` with:
- `--title` "weekly-review-{YYYY-MM-DD}" (where the date is the review date)
- `--folder` "daily"
- `--tags` "weekly-review, review"
- `--project` "brain"

Also update the goals note's weekly priorities section using `write_note` if the user approves.

## Output Format

```markdown
# Weekly Review: {Start Date} to {End Date}

> **Review Date:** {today}
> **Days Logged:** {X} of 7
> **Overall Assessment:** {Strong / Solid / Mixed / Rough}

---

## What Got Done

### {Project/Category 1}
- {accomplishment} [planned/unplanned]
- {accomplishment} [planned/unplanned]

### {Project/Category 2}
- {accomplishment}
- {accomplishment}

### Wins
- {notable win or breakthrough}
- {notable win or breakthrough}

---

## What Slipped

| Item | Was Due | Why It Slipped | Still Relevant? |
|------|---------|---------------|-----------------|
| {item} | {date/this week} | {reason} | Yes / No |
| {item} | {date} | {reason} | Yes / No |

---

## Goal Alignment

| Quarterly Goal | Work This Week | Status | Risk |
|---------------|---------------|--------|------|
| {goal 1} | {what was done} | On Track / At Risk / No Progress | {notes} |
| {goal 2} | {what was done} | {status} | {notes} |

### Time Allocation (approximate)
- {Goal/Category 1}: {X}%
- {Goal/Category 2}: {X}%
- Reactive/Unplanned: {X}%
- Admin/Overhead: {X}%

---

## Patterns and Observations

- {observation about the week's rhythm, energy, blockers, or habits}
- {observation}

---

## Next Week: {Date Range}

### Top 3 Priorities
1. **{Priority 1}** — {why, what "done" looks like}
2. **{Priority 2}** — {why, what "done" looks like}
3. **{Priority 3}** — {why, what "done" looks like}

### Carry-Forward
- {item from this week that still needs doing}

### Upcoming Deadlines
- {deadline} — {date}

### Stretch Goals (if time allows)
- {item}

---

## Accountability Score

| Metric | Score |
|--------|-------|
| Planned items completed | {X}/{Y} ({Z}%) |
| Goal-aligned work | {high/medium/low} |
| Days logged | {X}/7 |
| Overall week grade | {A/B/C/D/F} |

---

*Weekly review generated {date}.*
```

## Example Usage

**User:** "Weekly review"

**AI:** Reads all daily logs from the past 7 days, project files, and goals from Cloud Brain. Generates a full review showing 4 of 6 planned items completed, the new client portal behind schedule, content creation slipping for the 3rd consecutive week. Plans next week with the new client portal launch as the only top priority. Saves to Cloud Brain (folder: daily, title: weekly-review-2026-03-02).

**User:** "What slipped this week?"

**AI:** Focused review on just the "What Slipped" section — items that were planned but did not get done, with reasons.

**User:** "Plan next week"

**AI:** Reads current state from projects and goals, skips the backward-looking review, and drafts next week's priorities. Still checks what slipped to ensure carry-forward items are captured.

**User:** "Am I on track for Q1?"

**AI:** Reads goals.md Q1 objectives, compares against project progress and weekly output, gives an honest assessment of whether Q1 targets will be hit at the current pace.

**User:** "Wrap up the week and tell me what to focus on Monday"

**AI:** Quick review plus a focused "Monday morning" action list -- the 3 things to do first when the new week starts.

## Error Handling

- **If no daily log notes exist for the review period:** Report: "No daily logs found for {date range}. Either work was not logged this week, or the days were off. I'll base the review on project notes and goals only." Proceed with available data.
- **If no goals note exists in Cloud Brain:** Skip the goal alignment check and note: "No goals note found. I can't assess alignment without it. Use `/goals-projects` to create a goals note and enable goal tracking in future reviews."
- **If no project notes exist:** Skip project status and note: "No active projects found. The review covers daily logs and goals only."
- **If a weekly review note already exists for this date:** Ask: "A weekly review already exists for {date}. Should I overwrite it with a fresh review, or create a supplemental review (e.g., `weekly-review-{date}-v2`)?"
- **If the user asks for a review of a specific date range that has no data:** Report honestly: "I found no logs, project updates, or activity for the period {start} to {end}. This could mean the week was untracked or the dates are wrong. Want me to try a different date range?"
- **If updating the goals note weekly priorities section fails:** Save the proposed next-week priorities in the weekly review note itself and note: "I couldn't update the goals note directly — it may need reformatting. The proposed priorities are saved in the review note."
- **If the user asks to "plan next week" without doing the backward review:** Skip Steps 2-6 (what got done, what slipped) and jump directly to building next week's plan from current project states and goals. Note: "Skipping the backward review — just planning forward. Run a full `/weekly-review` later if you want the complete picture."
