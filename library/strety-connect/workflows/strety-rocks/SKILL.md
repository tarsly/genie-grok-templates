---
name: strety-rocks
description: >-
  Manage Rocks (Strety Goals) live via the Strety API — list this quarter's rocks with status, create a rock, post a check-in (on track / off track / progress value), manage milestones, backlog a rock. Use when the user says 'my Strety rocks', 'rock status in Strety', 'check in on my rock', 'add a milestone', or wants the team's real quarterly rocks rather than a Cloud Brain mirror.
---

# Strety Rocks — Quarterly Goals, Live

## Overview

Strety models Rocks as **Goals** (`/api/v1/goals`) with check-ins and milestones. This skill reads and writes them directly, so rock status reflects what the leadership team sees in Strety. EOS target: ≥80% of rocks done by quarter end.

## When This Skill Applies

- "what are my rocks in Strety", "rock status", "how are the Q3 rocks"
- "check in on [rock] — we're on track", "update rock progress to 60%"
- "add a rock", "add a milestone to [rock]", "backlog that rock"
- User invokes `/strety-rocks`

## Pre-Flight — Preferences

1. Credentials check — missing → route to `/strety-setup`.
2. `search_notes "strety preferences"` — person id, default team.

**Banner:**
```
🪨 Strety Rocks | {Team} | Q{N}: {N} rocks | On Track: {N} | Off Track: {N} | Done: {N}
```

## How It Works

```bash
S="bash ${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh"
```

### list (default)
```bash
$S GET /goals "assignee_id={person_id}&filter[archive_status]=active&include=latest_check_ins&limit_check_ins=1&page[size]=20"
```
Omit `assignee_id` for team-wide. Filter to the current quarter by `start_date`/`due_date`. Show: title, owner, status, latest check-in, % of quarter elapsed vs progress.

### add
```bash
$S POST /goals '{"data":{"type":"goals","attributes":{"title":"{SMART rock}","description":"{context}","start_date":"{quarter start}","due_date":"{quarter end}","check_in_type":"{status_or_value}","company_goal":false},"relationships":{"assignee":{"data":{"type":"people","id":"{person_id}"}},"space":{"data":{"type":"teams","id":"{team_id}"}}}}}'
```
Coach SMART phrasing before posting: specific, measurable, one owner, due by quarter end. For measurable rocks set `value_format`, `start_value`, `target_value`.

### checkin
```bash
$S POST /goals/{goal_id}/check_ins '{"data":{"type":"goal_check_ins","attributes":{"status":"{on_track|off_track|...}","value":{N or null},"context":"{one-line update}"}}}'
```
Off-track check-in? Suggest dropping an issue in Strety too (`/strety-issues add`).

### milestones
```bash
$S GET /goals/{goal_id}/milestones
$S POST /goals/{goal_id}/milestones '{"data":{"type":"goal_milestones","attributes":{"title":"...","due_date":"..."}}}'
```

### backlog
`$S POST /goals/{goal_id}/backlog` (and `DELETE` on the same path to restore). Confirm before backlogging — it removes the rock from the active quarter view.

## Data Structure

Goal attributes: `title`, `description`, `company_goal` (bool), `check_in_type`, `start_date`, `due_date`, `status`, `value_format`, `start_value`, `target_value`, `backlogged_at`, `archived_at`. Relationships: `assignee`, `space`, `parent` (company rock a department rock rolls into), `latest_check_ins`. GoalCheckIn: `status`, `value`, `context`.

## Output Format

```
🪨 Strety Rocks — {Team}, Q{N} {year} ({pct}% of quarter elapsed)

| Rock | Owner | Status | Last Check-in |
|---|---|---|---|
| Launch wholesale portal v2 | Jordan | 🟢 On Track | Jul 1 — "API integration done" |
| Hire 2 AEs | Mary | 🔴 Off Track | Jun 24 — "1 offer out" |

Completion pace: {N}/{total} done — {ahead of/behind} the ≥80% target.
```

## Example Usage

> **User:** check in on my portal rock — on track, integration is done
> **Claude:** Finds the goal by title match, POSTs a check-in with status `on_track` and that context, confirms with the updated rock line.

## Error Handling

- **No credentials** — route to `/strety-setup`.
- **Multiple rocks match a title fragment** — list candidates, ask which.
- **Check-in type mismatch** — a value check-in on a status-type rock (or vice versa) will 4xx; read the goal's `check_in_type` first and shape the payload accordingly.
- **429** — helper backs off; use `include=latest_check_ins` instead of N+1 per-rock check-in fetches.

## See Also

- `/eos-rocks` — Cloud Brain-native rocks with SMART coach (from `eos-operator`)
- `/strety-scorecard` — the weekly measurables behind the rocks
- `/strety-l10-prep` — rock review section of the L10 prep pack
