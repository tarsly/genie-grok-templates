---
name: strety-todos
description: >-
  Manage Strety To-Dos live via the Strety API — list open to-dos (mine or anyone's), add a to-do with owner/due-date/priority, mark complete, update, or delete. Use when the user says 'my Strety to-dos', 'add a to-do in Strety', 'mark that done in Strety', 'what's open for [name] in Strety', or wants the team's real to-do list rather than a Cloud Brain mirror.
---

# Strety To-Dos — Live Team To-Do List

## Overview

Reads and writes To-Dos in Strety itself — the team's system of record — via `${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh`. Where `eos-todos` (from `eos-operator`) tracks to-dos in your Cloud Brain, this skill operates on what the whole team sees in Strety's L10s.

## When This Skill Applies

- "what are my Strety to-dos", "show open to-dos in Strety"
- "add a to-do in Strety: [action]", "assign [name] a to-do"
- "mark [X] complete in Strety", "delete that to-do"
- User invokes `/strety-todos`

## Pre-Flight — Preferences

1. Confirm `~/.strety/credentials.json` exists — if not, route to `/strety-setup`.
2. `search_notes "strety preferences"` — default team id, user's person id. If missing, `GET /people` and `GET /teams` to resolve names, and suggest `/strety-setup` to persist them.

**Banner:**
```
✅ Strety To-Dos | {Team} | Open: {N} | Due This Week: {N}
```

## How It Works

All calls go through the helper — it handles token refresh, ETag, and rate limits (10 req/10s; keep list calls minimal):

```bash
S="bash ${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh"
```

### list (default)
```bash
$S GET /todos "filter[completed]=false&assignee_id={person_id}&page[size]=20"
```
Omit `assignee_id` for the whole team. Paginate via `page[number]` if `meta` shows more (20/page max). Render as a table: title, owner, due date, priority, overdue flag.

### add
Resolve the assignee name to a person id (`GET /people`), then:
```bash
$S POST /todos '{"data":{"type":"todos","attributes":{"title":"{verb-led action}","due_date":"{YYYY-MM-DD}","priority":"{priority|none}"},"relationships":{"assignee":{"data":{"type":"people","id":"{person_id}"}},"space":{"data":{"type":"teams","id":"{team_id}"}}}}}'
```
Coach EOS hygiene: verb-led title, one owner, due within 7 days (default: 7 days out).

### done
Find the todo id (from list or by title match), then:
```bash
$S PATCH /todos/{id} '{"data":{"type":"todos","id":"{id}","attributes":{"completed_at":"{ISO-8601 now}"}}}'
```
To un-complete: set `completed_at` to `null`.

### update
Same PATCH shape with changed attributes (`title`, `due_date`, `priority`, `description`).

### delete
Confirm with the user first (destructive), then `$S DELETE /todos/{id}`.

## Data Structure

Todo attributes (from the Strety OpenAPI spec): `title`, `description`, `priority` (`none|highest|high|medium|low|lowest`), `due_date` (date), `completed_at` (timestamp or null). Relationships: `assignee` (person), `space` (team). Responses are JSON:API — items under `data[]`, each with `id`, `attributes`, `relationships`.

## Output Format

```
✅ Strety To-Dos — {Team} ({N} open)

| To-Do | Owner | Due | Priority |
|---|---|---|---|
| Call Bob about renewal | Jordan | Jul 8 ⚠️ overdue | high |

{completion-rate comment if listing: X of Y completed this week}
```

## Example Usage

> **User:** add a to-do in Strety for Emma to send the client proposal by Friday
> **Claude:** Resolves Emma's person id, POSTs the todo with due_date Friday, confirms: "✅ Added: 'Send the client proposal' → Emma, due Jul 10."

## Error Handling

- **No credentials / refresh failure** — route to `/strety-setup`; don't retry blindly.
- **412 on PATCH** — the todo changed between ETag fetch and update (the helper fetches ETags automatically, so this means a mid-flight edit); re-run the operation.
- **429** — the helper backs off automatically via `Retry-After`; batch reads to stay under 10 req/10s.
- **Ambiguous name match** — if "mark the proposal one done" matches multiple todos, show candidates and ask.

## See Also

- `/eos-todos` — Cloud Brain-native to-do tracking (from `eos-operator`)
- `/strety-sync` — mirror Strety to-dos into Cloud Brain so `eos-operator` skills see them
- `/strety-l10-prep` — to-do review section of the weekly L10 prep pack
