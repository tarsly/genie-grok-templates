---
name: strety-scorecard
description: >-
  Work the Strety Scorecard live via the API — view weekly measurables (Metrics) with targets and trailing check-ins, log this week's numbers, create a new measurable, spot off-track streaks. Use when the user says 'Strety scorecard', 'log my numbers in Strety', 'update the scorecard', 'how are we tracking on [metric]', or wants real scorecard data rather than a Cloud Brain mirror.
---

# Strety Scorecard — Weekly Measurables, Live

## Overview

Strety models scorecard measurables as **Metrics** (`/api/v1/metrics`) with per-week **check-ins**. This skill views the scorecard, logs actuals, and flags off-track streaks — straight against the numbers the team reviews in the L10.

## When This Skill Applies

- "show the Strety scorecard", "how's the scorecard looking"
- "log my numbers: 14 demos, $32k collected", "update [metric] to [value]"
- "add a measurable to Strety", "is [metric] on track"
- User invokes `/strety-scorecard`

## Pre-Flight — Preferences

1. Credentials check — missing → route to `/strety-setup`.
2. `search_notes "strety preferences"` — person id, default team.

**Banner:**
```
📊 Strety Scorecard | {Team} | Measurables: {N} | On Target This Week: {N}/{M}
```

## How It Works

```bash
S="bash ${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh"
```

### view (default)
```bash
$S GET /metrics "include=latest_check_ins&limit_check_ins=13&page[size]=20"
```
Filter by `assignee_id` or `check_in_frequency=weekly` as needed. Render a 13-week trailing view per EOS convention: metric, owner, target, last 13 values (sparkline with ✅/❌ vs target), current streak.

### log
Resolve the metric by title, then post a check-in keyed to the ISO week:
```bash
$S POST /metrics/{metric_id}/check_ins '{"data":{"type":"metric_check_ins","attributes":{"value":{N},"iso_week":{W},"iso_week_year":{YYYY},"context":"{optional note}"}}}'
```
Default to the current ISO week (`date +%V` / `date +%G`). To correct an existing week's number, PATCH the check-in instead of double-posting (list check-ins, find the week, PATCH by id).

Logging multiple numbers in one message ("14 demos, $32k collected") → resolve each metric and post each check-in, respecting the 10 req/10s limit.

### add
```bash
$S POST /metrics '{"data":{"type":"metrics","attributes":{"title":"{measurable}","checkin_frequency":"weekly","number_format":"{number|currency|percent}","target_type":"{gte|lte|between}","target_value":{N}},"relationships":{"assignee":{"data":{"type":"people","id":"{person_id}"}},"space":{"data":{"type":"teams","id":"{team_id}"}}}}}'
```
Coach EOS scorecard hygiene: 5–15 measurables, weekly, leading indicators, one owner each.

### Off-track detection

After any view, flag metrics that missed target 2+ consecutive weeks and suggest: "Drop it on the Issues List? (`/strety-issues add`)"

## Data Structure

Metric attributes: `title`, `description`, `checkin_frequency`, `number_format`, `target_type`, `target_value`, `target_min_value`, `target_max_value`, `archived_at`. MetricCheckIn: `value`, `iso_week`, `iso_week_year`, `month`, `quarter`, `year`, `context`. Relationships: `assignee`, `space`, `latest_check_ins`.

## Output Format

```
📊 Strety Scorecard — {Team}, week {W}

| Measurable | Owner | Target | This Wk | 13-wk trend |
|---|---|---|---|---|
| Sales demos | Emma | ≥12 | 14 ✅ | ▂▄▆▄▆█▆▄▆▆▄▆█ |
| Cash collected | Jordan | ≥$30k | $32k ✅ | 10/13 on target |

⚠️ Off-track 2+ weeks: {metric} — recommend adding to Issues.
```

## Example Usage

> **User:** log my numbers in Strety — 14 demos and 32000 collected
> **Claude:** Resolves both metrics, posts two check-ins for the current ISO week, re-renders the affected rows with ✅/❌ vs target.

## Error Handling

- **No credentials** — route to `/strety-setup`.
- **Duplicate week check-in** — if the API rejects or the week already has a value, PATCH the existing check-in rather than creating a second.
- **Ambiguous metric name** — show candidates with owners, ask which.
- **429** — helper backs off; when logging many metrics, pace the POSTs.

## See Also

- `/eos-scorecard` — Cloud Brain-native scorecard (from `eos-operator`)
- `/strety-issues` — where 2-week misses should land
- `/strety-l10-prep` — scorecard review section of the L10 prep pack
