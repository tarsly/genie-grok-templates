---
name: prospect-handoff
description: >-
  Push qualified prospects from a Cloud Brain prospect list into your active CRM pipeline (bizops-lead-tracker). Handles dedup, stage assignment, source attribution, and optional follow-up scheduling — turns prospecting output into pipeline input with one command.
---

# Prospect Handoff

## Overview

The bridge between prospecting and pipeline. After `prospect-find` or `prospect-bulk-list` produces a list, `prospect-handoff` converts each entry into a lead in `bizops-lead-tracker` (Cloud Brain `pipeline/` folder). Handles dedup, sets initial stage, attributes source, and optionally schedules follow-up reminders.

Requires `business-operations` plugin (for `bizops-lead-tracker`) — works in degraded mode if not installed (writes plain leads to `pipeline/` without using the dashboard pattern).

## When This Skill Applies

- User has just generated a prospect list and wants to move it to pipeline
- User says: "push to pipeline", "add to CRM", "convert to leads", "handoff prospects"
- User reviewed a list, picked a subset, and wants to track them as deals
- A campaign is about to launch and the list needs to live where the rest of the pipeline lives

## Pre-Flight — Preferences

Reads `prospecting-preferences` and `bizops-pipeline-preferences`:
- Default initial stage for new prospects (new / contacted depending on workflow)
- Default source attribution label
- Default follow-up interval

If not set, ask:
- Default initial stage when pushing prospects → CRM (default: `new`)
- Default source label (e.g., `prospect-find`, `cold-outbound-2026-Q2`)
- Auto-schedule first follow-up? (yes — N days / no)

Banner:
```
🎯 Handoff | Default stage: {stage} | Source: {label} | Auto-followup: {days/off}
```

## How It Works

### Step 1: Resolve the List

User passes either:
- `--list "exact list note name"`
- A search query (e.g., "the bulk list from yesterday")
- No arg → list recent prospect lists in `pipeline/prospects/` and ask which

Use `read_note` to load the chosen list. Parse the CSV-ready data block.

### Step 2: Apply Filters

Filter the list before pushing:
- `--min-fit N` → only prospects with fit ≥ N
- `--top N` → only top N by fit
- `--require-email` → only prospects with verified email
- Default: push everything in the list unless user passes a filter

Show the filtered count vs total before continuing.

### Step 3: Dedup Against Existing Pipeline

For each prospect to push:
1. `search_notes` for their name in `pipeline/`
2. If a lead already exists:
   - Skip if stage is `negotiating` or `closed-won` (don't disturb live deals)
   - Update if stage is `new` or `contacted` — refresh enriched data, add interaction history entry
   - Reopen if stage is `closed-lost` — only if user explicitly asks with `--reopen-lost`
3. If no lead exists: create new

### Step 4: Create / Update Leads

For each new lead, use `write_note` with the same template as `bizops-lead-tracker`:

- **title:** `Lead — {Full Name}`
- **folder:** `pipeline`
- **tags:** `["lead", "pipeline", "{stage}", "{source-label}"]`

Pre-populate every field that came from the prospect list (email, phone, company, LinkedIn, fit score, signals). Add an Interaction History entry: `{date} | Imported | From {list name} via prospect-handoff`.

### Step 5: Update Pipeline Dashboard

After all leads are created/updated:
- `read_note` → Lead Pipeline dashboard
- Refresh totals (new count, total value, etc.)
- `write_note` to update dashboard

### Step 6: Schedule Follow-Ups (Optional)

If `--auto-followup N` is set:
- Set each new lead's Follow-Up Date to `today + N days`
- Stagger across N days if list is large (don't pile 200 follow-ups on the same day)

### Step 7: Render Summary

```
✓ HANDOFF COMPLETE
List: {list name}
Pushed: {N} new, {M} updated, {S} skipped
Source: {source-label}

NEW LEADS BY STAGE
new: {N}

SKIPPED
- {N} already in pipeline (active stages, not disturbed)
- {N} already closed-lost (use --reopen-lost to override)

UPDATED
- {N} refreshed with new enrichment data

FOLLOW-UPS
- {N} scheduled across next {days} days
```

## Data Structure

The handoff creates / updates leads in the existing `bizops-lead-tracker` format. Reference: `plugins/business-operations/skills/bizops-lead-tracker/SKILL.md`.

Each created lead's note includes:
- Standard Lead template (from lead-tracker)
- Additional "Prospect Source" section:
  ```markdown
  ## Prospect Source

  - **List:** {list name}
  - **Generated:** {YYYY-MM-DD}
  - **Provider:** {Apollo / Hunter / etc.}
  - **Fit Score:** {N}/100
  - **Buying Signals at Import:** {bullet list if any}
  - **Source Label:** {source label}
  ```

## Output Format (Chat)

```
✓ HANDOFF — {list name}
{X} new • {Y} updated • {Z} skipped

NEW
1. {Name} @ {Company} — stage: {stage} — followup: {date}
2. ...
(showing top 10, full list saved to pipeline)

UPDATED (already in pipeline, refreshed enrichment only)
1. {Name} — {prior stage} — {what was refreshed}

SKIPPED
- {N} already in active deal stages (untouched)
- {M} previously closed-lost (skipped)
- {K} below min-fit threshold

NEXT
{if Lead Pipeline dashboard exists in Cloud Brain — business-operations installed}
- See pipeline: /bizops-lead-tracker pipeline
- Check follow-ups: /bizops-lead-tracker follow-ups
{else}
- Install `business-operations` to see these leads in a pipeline dashboard
{endif}
{if mcp__cloud-brain check for comm-email-drafter — communications installed}
- Generate outreach: /comm-email-drafter --leads-from-list "{list name}"
{else}
- Install `communications` to draft outreach emails for this list
{endif}
```

## Example Usage

**User:** "Push the agency-clients bulk list to my pipeline, only fit 70+"

**AI:** Loads `Bulk List — agency-clients — 2026-06-20 (200)`. Filters to fit ≥ 70 = 47 prospects. Dedups against pipeline. Creates 42 new leads, updates 3 existing, skips 2 closed-lost. Updates dashboard. Schedules first follow-ups staggered over 5 days.

**User:** "/prospect-handoff --list 'Prospect List — re-sellers-utah — 2026-06-20' --top 25 --source 'Utah outbound June'"

**AI:** Top 25 by fit from the RE seller list. Source label `Utah outbound June`. Pushes to pipeline. Renders summary.

**User:** "Handoff the bulk list and reopen any closed-lost matches"

**AI:** Standard handoff + reopens prior closed-lost entries. Logs the reopen reason in each reopened lead's Interaction History.

**User:** "I have a list of 500 prospects — push the top 100 by fit, no follow-ups yet"

**AI:** Top 100 only. `--auto-followup off`. Pushes. Notes the remaining 400 are still available in the list note.

## Error Handling

- **If list note can't be found:** List all available lists in `pipeline/prospects/`, ask which one.
- **If list note is malformed (no CSV block):** Refuse to push partial data. Suggest regenerating the list.
- **If pushing would create > 100 leads in one run:** Confirm explicitly. Pipeline visibility degrades fast if you flood it.
- **If `bizops-lead-tracker` isn't installed:** Push to plain `pipeline/` notes without dashboard updates. Warn user: "Install business-operations to get the full pipeline dashboard."
- **If a lead has the same name but different company than an existing closed-lost lead:** Treat as different person, create new. Don't conflate.
- **If `--auto-followup` is set to 0:** Treat as off. Don't schedule follow-ups for the same day as import.
- **If user's pipeline already has 500+ active leads:** Flag: "Your pipeline already has {N} active leads. Adding {M} more may dilute focus. Consider archiving stale leads first via `/bizops-pipeline-sync`."
- **If the source list is from buy-box mode (RE) and pipeline is set up for SaaS-style CRM:** Use the SaaS stage names anyway, but adjust default values (e.g., for RE, stage `new` = "lead added", `contacted` = "first contact made", `qualified` = "shows interest in selling", `negotiating` = "negotiating price/terms", `closed-won` = "under contract", `closed-lost` = "won't sell").

## See Also

- `/prospect-find`, `/prospect-bulk-list` — upstream skills that produce the lists this consumes (same plugin)
- `/bizops-lead-tracker` — destination CRM (from `business-operations`)
- `/bizops-pipeline-sync` — pipeline analysis on the resulting leads (from `business-operations`)
