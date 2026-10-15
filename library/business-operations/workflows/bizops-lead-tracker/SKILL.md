---
name: bizops-lead-tracker
description: >-
  Track leads through pipeline stages — add, update, and manage leads, prospects, and deals. View pipeline status, follow-up reminders, deal stage tracking, lead reports, sales funnel management, or needs to add a new prospect, update a deal stage, or see which leads need follow-up today.
---

# Lead Tracker

## Overview


> **Two-Phase Pipeline System — Phase 0:** Lead Tracker is the data entry and status management layer. It captures leads, updates stages, and logs all activity. When you want analysis, forecasting, and deal intelligence, run **Pipeline Sync (Phase 1)**, which reads everything Lead Tracker has stored and produces the full pipeline report.

A Cloud Brain-based lead and contact pipeline manager that tracks prospects through defined sales stages. All data is stored as notes in Cloud Brain in the `pipeline` folder, making it searchable, persistent across sessions, and accessible from any Claude interface. Each lead gets its own note with full history. A master pipeline index note provides the dashboard view.

## When This Skill Applies

- User wants to add a new lead or prospect
- User wants to update a lead's status or stage
- User asks to view the pipeline or see all leads
- User asks "who do I need to follow up with?"
- User wants to mark a deal as closed-won or closed-lost
- User asks for a pipeline report or summary
- User mentions CRM, sales pipeline, deal tracker, or lead management
- User says "add to pipeline", "move to negotiating", "lost the deal", etc.
- User wants to log notes or interactions against a lead


## Pre-Flight — Pipeline Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops pipeline preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Business name and industry
   - Custom pipeline stage names (or use defaults: New → Contacted → Qualified → Negotiating → Closed-Won / Closed-Lost)
   - Average deal value range
   - Primary lead sources (referral / social / outbound / website / events)
   - Follow-up interval (how many days between touches before a lead is considered stale)
   - Save to Cloud Brain: `write_note` → title: `bizops-pipeline-preferences`, folder: `brain/preferences`
4. Apply throughout: use custom stage names in all pipeline views, stale threshold for follow-up flags
5. Show banner at top of every output:
   ```
   🎯 Lead Tracker | {Business Name} | Stages: {stage list} | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my pipeline preferences' to change settings."*

## How It Works

### Data Structure

Each lead is stored as a separate note in Cloud Brain:
- **Pipeline Index:** `write_note` with title `Lead Pipeline`, folder `pipeline`, tags `["pipeline", "dashboard", "index"]`
- **Individual Leads:** `write_note` with title `Lead — {Full Name}`, folder `pipeline`, tags `["lead", "pipeline", "{stage}"]`

### Pipeline Stages

| Stage | Code | Description |
|-------|------|-------------|
| New | `new` | Just added, no outreach yet |
| Contacted | `contacted` | Initial outreach made |
| Qualified | `qualified` | Confirmed fit, has budget/need/authority |
| Negotiating | `negotiating` | Active deal discussion, terms being worked |
| Closed-Won | `closed-won` | Deal done, revenue captured |
| Closed-Lost | `closed-lost` | Deal dead, with reason logged |

### Step 1: Initialize Pipeline (first time only)

Use `search_notes` with query "lead pipeline" to check if a pipeline index exists. If not, create it using `write_note`:

- **title:** `Lead Pipeline`
- **folder:** `pipeline`
- **tags:** `["pipeline", "dashboard", "index"]`

```markdown
# Lead Pipeline

> **Last Updated:** {date}

## Dashboard

| Lead | Company | Stage | Value | Next Action | Follow-Up Date |
|------|---------|-------|-------|-------------|----------------|

## Stage Summary

| Stage | Count | Total Value |
|-------|-------|-------------|
| New | 0 | $0 |
| Contacted | 0 | $0 |
| Qualified | 0 | $0 |
| Negotiating | 0 | $0 |
| Closed-Won | 0 | $0 |
| Closed-Lost | 0 | $0 |
```

### Step 2: Add a New Lead

When user says "add a lead" or provides lead info, create a lead note using `write_note`:

- **title:** `Lead — {Full Name}`
- **folder:** `pipeline`
- **tags:** `["lead", "pipeline", "new"]`

```markdown
# {Full Name}

> **Stage:** new
> **Created:** {YYYY-MM-DD}
> **Last Updated:** {YYYY-MM-DD}
> **Follow-Up Date:** {YYYY-MM-DD, default 3 days from now}

## Contact Info

| Field | Value |
|-------|-------|
| Name | {name} |
| Company | {company or "—"} |
| Phone | {phone or "—"} |
| Email | {email or "—"} |
| Source | {where the lead came from or "—"} |

## Deal Info

| Field | Value |
|-------|-------|
| Estimated Value | ${amount or "TBD"} |
| Product/Service | {what they need} |
| Timeline | {when they need it} |
| Decision Maker | {yes/no/unknown} |

## Notes

- {YYYY-MM-DD}: Lead added. {any initial notes}

## Interaction History

| Date | Type | Summary |
|------|------|---------|
| {date} | Added | Lead created |
```

After creating the lead note, update the Lead Pipeline dashboard note using `write_note`.

### Step 3: Update a Lead

When user says "move {name} to negotiating" or "update {name}":

1. Use `search_notes` with the lead name to find their note, then `read_note` to get the content
2. Update the `> **Stage:**` field
3. Update `> **Last Updated:**` date
4. Add a note to the Notes section with what changed
5. Add entry to Interaction History
6. Save with `write_note` (update tags to reflect new stage)
7. Update the Lead Pipeline dashboard note

### Step 4: View Pipeline

When user says "show pipeline" or "pipeline status":

1. Use `search_notes` with query "lead pipeline" to find the dashboard, then `read_note` for the content
2. Optionally search for individual lead notes for detail
3. Present a clean summary:
   - Total leads by stage
   - Total pipeline value
   - Leads needing follow-up (follow-up date is today or past)
   - Leads that have been stale (no update in 7+ days)

### Step 5: Follow-Up Reminders

When user asks "who do I need to follow up with?" or "follow-up reminders":

1. Use `search_notes` with query "lead pipeline" to get all leads
2. Check each lead's `Follow-Up Date` against today's date
3. Flag any lead where:
   - Follow-up date is today or in the past
   - Lead has been in the same stage for 7+ days with no activity
   - Lead is in "contacted" or "negotiating" stage (high-priority follow-ups)
4. Present sorted by urgency

### Step 6: Pipeline Report

When user asks for a pipeline report:

1. Search Cloud Brain for all lead notes using `search_notes` with query "lead"
2. Calculate:
   - Total leads per stage
   - Total pipeline value (sum of estimated values for active leads)
   - Conversion rate (closed-won / total closed)
   - Average time in pipeline
   - This week's activity (new leads added, stages changed, deals closed)
3. Present as a formatted report

## Output Format

### Adding a Lead
```
Lead added: {Name} ({Company})
Stage: New
Estimated Value: ${amount}
Follow-up set for: {date}
Pipeline updated.
```

### Pipeline Dashboard
```
PIPELINE SUMMARY — {date}
================================
New:          {count} leads  (${value})
Contacted:    {count} leads  (${value})
Qualified:    {count} leads  (${value})
Negotiating:  {count} leads  (${value})
Closed-Won:   {count} leads  (${value})
Closed-Lost:  {count} leads  (${value})
--------------------------------
Active Pipeline: {count} leads, ${total value}

NEEDS FOLLOW-UP:
- {Name} — {stage} — follow-up was {date}
- {Name} — {stage} — no activity for {N} days
```

### Stage Update
```
{Name} moved from {old stage} to {new stage}.
Notes: {any notes added}
Next follow-up: {date}
```

## Example Usage

**User:** "Add John Smith to the pipeline. He's from ABC Motors, interested in a connection deal, estimated $5K. His email is john@abcmotors.com"

**AI:** Creates a `Lead — John Smith` note in Cloud Brain in the `pipeline` folder, updates the pipeline dashboard note, confirms.

**User:** "Move John Smith to qualified"

**AI:** Updates stage, adds note, sets follow-up for 3 days, updates dashboard.

**User:** "Show my pipeline"

**AI:** Reads all lead notes from Cloud Brain, presents dashboard with stage counts, values, and follow-up alerts.

**User:** "Who needs follow-up?"

**AI:** Scans all leads in Cloud Brain, identifies overdue follow-ups, presents sorted list.

**User:** "Mark John Smith as closed-won. Connection fee was $4,500."

**AI:** Updates stage to closed-won, logs final value, adds to interaction history, updates dashboard.

**User:** "/lead-tracker add Sarah Lee — she's a buyer from Facebook Marketplace, budget $15K, phone 555-9876"

**AI:** Creates `Lead — Sarah Lee` note with stage: new, source: Facebook Marketplace, value: $15,000, phone captured. Updates dashboard. Sets follow-up for 3 days.

**User:** "/lead-tracker pipeline"

**AI:** Reads all lead notes from Cloud Brain and produces the full pipeline summary.

**User:** "/lead-tracker update Marcus Thompson --stage closed-won --value 4500 --notes 'TC fee collected, deal closed successfully'"

**AI:** Updates Marcus from negotiating to closed-won, sets final value to $4,500, adds timestamped note and interaction history entry, updates dashboard totals.

## Error Handling

- **If the Lead Pipeline note does not exist in Cloud Brain:** Create it automatically with the template and proceed. Do not error — initialize silently.
- **If user does not provide a lead name:** Ask specifically: "What is the lead's name? I need at least a first and last name to create their record."
- **If user does not provide enough info for required fields:** Create the lead with what is available, fill missing fields with "TBD", and inform the user: "I created the lead with the info you gave me. These fields are still TBD: [list]. You can update them anytime with `/lead-tracker update [name]`."
- **If a lead note already exists for that name:** Do not overwrite. Instead, warn: "A lead already exists for {name}. Did you mean to update their record? Use `/lead-tracker update {name}` to modify existing leads." If the user confirms they want a new entry (different person, same name), create the note with a suffix: `Lead — {Full Name} 2`.
- **If the user references a lead name that does not exist:** Use `search_notes` with partial name for fuzzy matching. If a close match is found, suggest: "I didn't find '{name}' but I found '{close match}'. Did you mean them?" If no match, report: "No lead found matching '{name}'. Current leads: [list names]."
- **If the pipeline dashboard gets out of sync with individual lead notes:** When running `/lead-tracker pipeline` or `/lead-tracker status`, always rebuild the dashboard from the individual lead notes rather than trusting the cached dashboard. Update the dashboard note with fresh data.
- **If deal value is provided in non-numeric format:** Parse common formats like "$5K", "$5,000", "5000", "five thousand" and convert to numeric. If unparseable, ask: "I couldn't parse the deal value '{input}'. Can you give me a number like $5,000?"
- **If the user tries to move a lead backward in the pipeline (e.g., from negotiating back to new):** Allow it, but confirm: "Moving {name} from {current stage} back to {requested stage}. This is unusual — are you sure? I'll log the reason in the notes."
- **If the user tries to update a closed lead (closed-won or closed-lost):** Allow it, but confirm: "This lead is already marked as {stage}. Do you want to reopen it? I'll move it to the stage you specified and log the reopen in the history."

## See Also

- `/bizops-pipeline-sync` — analysis layer on top of these leads (same plugin)
- `/bizops-follow-up` — schedule follow-ups against these leads (same plugin)
- `/prospect-find` — find new B2B leads to add (from `prospecting`)
- `/prospect-enrich` — fill in missing email / phone / LinkedIn on a lead (from `prospecting`)
- `/prospect-handoff` — push prospect lists into this pipeline (from `prospecting`)
- `/brief-pre-call` — prep before calling a lead (from `person-brief`)
