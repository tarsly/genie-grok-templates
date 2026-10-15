---
name: bizops-project-tracker
description: >-
  Track multi-milestone business projects — create projects with phases and owners, update status, view active workload, flag blockers and overdue milestones, and get a project health report, or needs to track milestones, flag blockers, or pull a project health report for an internal initiative.
---

# Project Tracker — Internal Initiative Management

## Overview

Project Tracker manages multi-milestone internal projects: product launches, website builds, process rollouts, location openings, hiring campaigns, anything with phases, owners, and deadlines. It is different from the follow-up tracker (single commitments) and client onboarding (client-facing setup). This is for your own business initiatives that have more than one moving part and need to be tracked over time.

## When This Skill Applies

- User says "create a project" or "track a project" or "start a project"
- User says "what projects are we working on?" or "project status"
- User says "update [project]" or "mark [milestone] complete"
- User says "what's due this week?" or "what's overdue?"
- User says "project report" or "project health check"
- User says "we're launching [something]" — capture it as a project
- User says "[project] is blocked" or "what's blocking [project]?"
- User says "close out [project]" or "project complete"
- User mentions a multi-step initiative that needs to be tracked (website, new hire, product launch, location, campaign, system rollout)
- User says "project dashboard" or "what are we building right now?"

## Pre-Flight — Project Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops project preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Business name
   - Default project categories (e.g., Marketing, Operations, Product, Hiring, Technology, Facilities)
   - Overdue threshold (how many days past a milestone due date before flagging 🚨 — default: 3 days)
   - Default milestone owners (e.g., "me" or specific team member names)
   - Save to Cloud Brain: `write_note` → title: `bizops-project-preferences`, folder: `brain/preferences`
4. Apply throughout: use categories for project tagging, overdue threshold for all deadline flags
5. Show banner at top of every output:
   ```
   🗂️ Project Tracker | {Business Name} | Overdue threshold: {X} days | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my project preferences' to change settings."*

## How It Works

### Project Note Template

Each project is stored as a note in Cloud Brain. Use `write_note` with:
- **title:** `project-{project-name-slug}`
- **folder:** `brain/projects`
- **tags:** `["project", "{category}", "active"]`

```markdown
# {Project Name}

> **Category:** {Marketing / Operations / Product / Hiring / Technology / Facilities / Other}
> **Status:** Active / On Hold / Complete / Cancelled
> **Owner:** {Primary owner}
> **Started:** {YYYY-MM-DD}
> **Target Completion:** {YYYY-MM-DD or "Ongoing"}
> **Priority:** High / Medium / Low

## Goal

{One sentence: what does success look like when this project is done?}

## Milestones

| # | Milestone | Owner | Due Date | Status | Notes |
|---|-----------|-------|----------|--------|-------|
| 1 | {milestone} | {owner} | {YYYY-MM-DD} | Not Started / In Progress / Complete / Blocked | |
| 2 | {milestone} | {owner} | {YYYY-MM-DD} | Not Started | |
| 3 | {milestone} | {owner} | {YYYY-MM-DD} | Not Started | |

## Blockers

{List any current blockers. "None" if clear.}

## Notes & Updates

| Date | Update |
|------|--------|
| {today} | Project created |
```

---

### Operations

#### Create a Project

1. Collect: project name, goal/outcome, category, priority, target completion, initial milestones with owners and due dates
2. Check for duplicates: `search_notes` with project name before creating
3. Create note using template above
4. Confirm: "✓ {Project} created with {X} milestones. Next due: {milestone} on {date}."

**When collecting milestones:** Ask the user to list the key phases or steps, not every micro-task. A project should have 3–10 milestones. If the user describes too many, help them group related steps into a single milestone.

#### Update a Project

1. Find via `search_notes` → `read_note`
2. Apply the update:
   - Mark milestone complete → update status to "Complete", log update with today's date
   - Add new milestone → append to table
   - Flag a blocker → add to Blockers section, update milestone status to "Blocked"
   - Update due date → update in table, note the change in the update log
3. `write_note` with `overwrite: true`
4. Report what changed

#### Project Status — Single Project

Load and display:
```
PROJECT STATUS: {Project Name}
================================
Status:   {Active / On Hold}
Priority: {High / Medium / Low}
Owner:    {name}
Target:   {YYYY-MM-DD} ({X days away / X days overdue})

MILESTONES
| # | Milestone | Owner | Due | Status |
|---|-----------|-------|-----|--------|
| 1 | {name} | {owner} | {date} | ✅ Complete |
| 2 | {name} | {owner} | {date} | 🔄 In Progress |
| 3 | {name} | {owner} | {date} | ⏳ Not Started |
| 4 | {name} | {owner} | {date} | 🚨 Blocked |

Progress: {X of Y milestones complete} ({X}%)

BLOCKERS
{List or "None"}

NEXT ACTION: {specific next step}
```

Status icons: ✅ Complete | 🔄 In Progress | ⏳ Not Started | 🚨 Blocked | ⚠️ Overdue

#### Project Dashboard — All Active Projects

1. `search_notes` with query `"project"` (folder: `brain/projects`, tags: `["active"]`)
2. Load each project note
3. Calculate: milestones complete, next due date, days until completion, blockers

```
PROJECT DASHBOARD — {Business Name}
====================================

ACTIVE PROJECTS ({X} total)

| Project | Category | Priority | Progress | Next Due | Status |
|---------|----------|----------|----------|----------|--------|
| {name} | {cat} | High | 3/7 (43%) | {date} | 🔄 On Track |
| {name} | {cat} | Medium | 1/4 (25%) | {date} | ⚠️ Overdue |
| {name} | {cat} | High | 0/5 (0%) | {date} | 🚨 Blocked |

NEEDS ATTENTION
🚨 {Project} — Blocked: {blocker description}
⚠️ {Project} — "{Milestone}" is {X} days overdue (owner: {name})

DUE THIS WEEK
• {Project} → {Milestone} due {date} (owner: {name})
• {Project} → {Milestone} due {date} (owner: {name})
```

#### Close a Project

1. Find via `search_notes` → `read_note`
2. Confirm all milestones are complete (or ask user to confirm if some are not)
3. Update note: Status → `Complete`, add close date and outcome to Notes section
4. Update tags: replace `"active"` with `"complete"`
5. `write_note` with `overwrite: true`
6. "✓ {Project} closed. {X of Y milestones complete}. Archived in Cloud Brain."

## Memory Paths

| Content | Path |
|---------|------|
| Preferences | `brain/preferences/bizops-project-preferences.md` |
| Project files | `brain/projects/project-{slug}.md` |

## Integration with Other Skills

- **bizops-follow-up:** Use Follow-Up Tracker for individual commitments that arise from project milestones (e.g., "Follow up with {vendor} by Friday"). Keep them separate — projects track the plan; follow-ups track the promises.
- **bizops-client-onboarding:** Client onboarding creates a project file automatically. Project Tracker can manage that file for ongoing client engagements.
- **bizops-hiring-screener:** Use Project Tracker to manage the hiring process itself as a project (e.g., "Q3 Sales Hire" with milestones: job description posted → applications reviewed → interviews scheduled → offer made → hired).

## Error Handling

- **Project not found:** "I don't have a project called {name} on file. Say 'create a project' to start one."
- **No active projects:** "No active projects on file. Say 'create a project' to start tracking one."
- **Overdue milestone with no owner:** Flag it, suggest: "This milestone has no owner. Assign one so there's accountability."
- **User gives very vague project description:** Ask: "To set this up properly, I need: (1) what does success look like when this is done? (2) what are the 3–5 major steps to get there? (3) who's responsible?"
- **Milestone count too high (>15):** "That's a lot of milestones. Let me help you group them into phases — it'll be easier to track. Here's a suggested grouping: {groups}. Want to use these or keep your original list?"
- **Duplicate project name:** "A project named {name} already exists. Show you the existing one, or create a new project with a different name?"
