---
name: strety-l10-prep
description: >-
  Build a pre-meeting L10 prep pack from live Strety data — scorecard week-over-week, rock statuses, open to-dos with completion rate, top issues, fresh headlines, all in one brief. Use when the user says 'prep my L10', 'L10 prep from Strety', 'what should I know before the weekly meeting', or before running /eos-level10 when the team's data lives in Strety.
---

# Strety L10 Prep — Walk In Already Briefed

## Overview

Pulls everything the L10 agenda touches from Strety in one pass — scorecard, rocks, to-dos, issues, headlines — and assembles the pre-meeting digest. The eos-operator `eos-level10` skill runs the meeting itself; this skill makes sure the data walking into it is the team's live Strety data, not a stale mirror.

## When This Skill Applies

- "prep my L10", "brief me before the weekly meeting"
- "pull this week's numbers from Strety", "L10 digest"
- Called before `/eos-level10` when Strety is the system of record
- User invokes `/strety-l10-prep`

## Pre-Flight — Preferences

1. Credentials check — missing → route to `/strety-setup`.
2. `search_notes "strety preferences"` — default team; `--team` overrides.

**Banner:**
```
📋 L10 Prep | {Team} | {date} | Source: Strety (live)
```

## How It Works

```bash
S="bash ${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh"
```

Fetch the five L10 data sets (respect the 10 req/10s limit — this is 5 calls):

1. **Scorecard:** `$S GET /metrics "include=latest_check_ins&limit_check_ins=2&page[size]=20"` — this week vs last, ✅/❌ vs target.
2. **Rocks:** `$S GET /goals "filter[archive_status]=active&include=latest_check_ins&limit_check_ins=1&page[size]=20"` — status counts, off-track callouts.
3. **To-Dos:** `$S GET /todos "filter[completed]=false&page[size]=20"` plus `filter[completed]=true&updated_after={7 days ago}` for the completion rate.
4. **Issues:** `$S GET /issues "filter[resolved]=false&filter[archive_status]=active&page[size]=20"` — top 3 by priority.
5. **Headlines:** `$S GET /headlines "created_after={7 days ago}&page[size]=20"`.

Then synthesize the digest (Output Format below). Flag EOS health signals:
- To-do completion < 90% → call it out
- Scorecard metric missed 2+ weeks → recommend it become an issue
- Rock off track with < 4 weeks left in quarter → escalate to top of IDS candidates

With `--post-to-brain`, also write the digest to Cloud Brain (folder `eos`, title `L10 Prep {team} {YYYY-MM-DD}`, tags `["eos", "l10", "strety", "prep"]`) so `/eos-level10` picks it up as the pre-meeting digest.

## Data Structure

Cloud Brain note (only with `--post-to-brain`):

```markdown
# L10 Prep — {Team} — {YYYY-MM-DD}

## Scorecard ({N}/{M} on target)
| Measurable | Target | This Wk | Last Wk |
...

## Rocks ({N} on track / {N} off / {N} done)
...

## To-Dos ({pct}% completion last 7 days)
...

## Top 3 Issues (IDS candidates)
...

## Headlines
...
```

## Output Format

```
📋 L10 Prep — {Team} — {date}

**Scorecard:** {N}/{M} on target. Misses: {metric} ({owner}, 2nd straight week ⚠️)
**Rocks:** {N} on track, {N} off. Off track: {rock} — {owner}
**To-Dos:** {pct}% done last week {✅/⚠️ below 90%}. {N} open, {N} overdue.
**Top 3 Issues:** 1. {…} 2. {…} 3. {…}
**Headlines:** {N} new — {one-liners}

Recommended IDS order: {issue}, {issue}, {off-track rock}
Ready to run the meeting? → /eos-level10
```

## Example Usage

> **User:** prep my L10
> **Claude:** Five API calls, one digest, ends with the recommended IDS order and the `/eos-level10` handoff.

## Error Handling

- **No credentials** — route to `/strety-setup`.
- **Empty data sets** — render the section as "none this week" rather than omitting it; an empty issues list is itself a signal worth stating.
- **429 mid-prep** — helper backs off; the 5 calls fit inside one rate window, but retries may add a pause. Tell the user if the prep takes a beat.
- **Multiple teams and no default** — list teams, ask which.

## See Also

- `/eos-level10` — run the actual 90-minute meeting (from `eos-operator`)
- `/strety-sync` — persistent Strety → Cloud Brain mirror if you want history queryable offline
