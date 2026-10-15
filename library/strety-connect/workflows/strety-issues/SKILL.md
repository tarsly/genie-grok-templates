---
name: strety-issues
description: >-
  Manage the Strety Issues List and Headlines live via the API — add an issue, list open issues (short-term/long-term), resolve after IDS, add or list Headlines. Use when the user says 'add an issue in Strety', 'what's on the issues list', 'resolve that issue', 'drop a headline in Strety', or wants the team's real issues list rather than a Cloud Brain mirror.
---

# Strety Issues & Headlines — Live

## Overview

Reads and writes Strety's Issues List (`/api/v1/issues`) and Headlines (`/api/v1/headlines`). Issues are the raw material of IDS; Headlines are the 90-second people/client updates at the top of the L10. This skill keeps both in Strety where the whole team sees them.

## When This Skill Applies

- "add an issue in Strety: [problem]", "we have an issue with X"
- "show the issues list", "top issues for the L10"
- "resolve [issue] — we solved it", "archive that issue"
- "add a headline: [good/bad news]", "any headlines this week?"
- User invokes `/strety-issues`

## Pre-Flight — Preferences

1. Credentials check — missing → route to `/strety-setup`.
2. `search_notes "strety preferences"` — person id, default team.

**Banner:**
```
⚠️ Strety Issues | {Team} | Open: {N} (ST: {N} / LT: {N}) | Headlines this week: {N}
```

## How It Works

```bash
S="bash ${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh"
```

### list (default)
```bash
$S GET /issues "filter[resolved]=false&filter[archive_status]=active&page[size]=20"
```
Optional `filter[issue_type]` (short_term/long_term) and `owner_id`. Sort by priority; surface the top 3 as L10 IDS candidates.

### add
```bash
$S POST /issues '{"data":{"type":"issues","attributes":{"title":"{one-line issue}","description":"{context}","issue_type":"{short_term|long_term}","priority":"{priority}"},"relationships":{"owner":{"data":{"type":"people","id":"{person_id}"}},"space":{"data":{"type":"teams","id":"{team_id}"}}}}}'
```
Default `short_term`; long-term = parking lot for the Quarterly. Keep titles as the real issue, not the symptom.

### resolve
```bash
$S PATCH /issues/{id} '{"data":{"type":"issues","id":"{id}","attributes":{"resolved_at":"{ISO-8601 now}"}}}'
```
If the solve produced an action item, offer to create it: `/strety-todos add`.

### headline
```bash
$S GET /headlines "page[size]=20"                    # list recent
$S POST /headlines '{"data":{"type":"headlines","attributes":{"title":"{headline}","description":"{detail}"},"relationships":{"space":{"data":{"type":"teams","id":"{team_id}"}}}}}'
```
Mark one discussed by PATCHing `discussed_at`.

## Data Structure

Issue attributes: `title`, `description`, `issue_type`, `priority`, `resolved_at`, `archived_at`. Relationships: `owner`, `space`. Headline attributes: `title`, `description`, `discussed_at`, `archived_at`.

## Output Format

```
⚠️ Strety Issues — {Team} ({N} open)

Top 3 for next L10:
1. 🔴 {issue} — {owner}
2. 🟠 {issue} — {owner}
3. 🟠 {issue} — {owner}

{N} more short-term · {N} long-term (parked for Quarterly)
```

## Example Usage

> **User:** we solved the shipping delay issue in today's meeting — Emma owns the fix, due Friday
> **Claude:** PATCHes the issue `resolved_at`, then creates the to-do via the strety-todos pattern: "Fix carrier cutoff config" → Emma, due Friday. Confirms both.

## Error Handling

- **No credentials** — route to `/strety-setup`.
- **412 on PATCH** — mid-flight edit; re-run (the helper re-fetches the ETag each attempt).
- **Ambiguous issue match** — show open candidates, ask which.
- **429** — helper backs off automatically.

## See Also

- `/eos-issues` and `/eos-ids` — Cloud Brain-native issues + IDS facilitator (from `eos-operator`)
- `/strety-todos` — where IDS solves land as action items
- `/strety-l10-prep` — issues section of the L10 prep pack
