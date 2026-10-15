---
name: strety-sync
description: >-
  Mirror live Strety data into Cloud Brain so eos-operator skills and offline queries see current rocks, scorecard, to-dos, and issues. One-way Strety → Cloud Brain snapshot with a sync log. Use when the user says 'sync Strety', 'pull Strety into my brain', 'refresh the EOS mirror', or schedules a recurring Strety sync.
---

# Strety Sync — Strety → Cloud Brain Mirror

## Overview

One-way snapshot sync: reads Strety (system of record) and writes/updates Cloud Brain notes in the `eos` folder using the same note shapes `eos-operator` skills expect. After a sync, `/eos-rocks`, `/eos-scorecard`, `/eos-todos`, and `/eos-issues` see the team's live data, and everything stays queryable in Cloud Brain even offline.

**Direction is deliberate:** Strety → Brain only. Writing back to Strety happens through the purpose-built skills (`/strety-todos`, `/strety-rocks`, etc.), never as a bulk push — bulk two-way sync risks clobbering teammates' edits.

## When This Skill Applies

- "sync Strety", "pull Strety into my brain", "refresh the mirror"
- Recurring schedule (e.g., every Monday before the L10)
- After `/strety-setup`, as the first population of the mirror
- User invokes `/strety-sync`

## Pre-Flight — Preferences

1. Credentials check — missing → route to `/strety-setup`.
2. `search_notes "strety preferences"` — default team.
3. `search_notes "strety sync log"` — last sync time; use it for `updated_after` incremental pulls.

**Banner:**
```
🔄 Strety Sync | {Team} | Last sync: {datetime or "never"} | Scope: {scopes}
```

## How It Works

```bash
S="bash ${CLAUDE_PLUGIN_ROOT}/scripts/strety.sh"
```

Default scope: `rocks,scorecard,todos,issues`. `vto` (Visions + Roles Charts, both read-only in the API) is opt-in since it changes rarely.

Per scope (paginate `page[size]=20`; pass `updated_after={last_sync}` for incremental syncs after the first):

| Scope | Strety Source | Cloud Brain Note (folder `eos`) |
|---|---|---|
| rocks | `/goals` + latest check-ins | `EOS Rocks {Team} (Strety Mirror)` |
| scorecard | `/metrics` + 13 check-ins | `EOS Scorecard {Team} (Strety Mirror)` |
| todos | `/todos` (open + last 7d completed) | `EOS To-Dos {Team} (Strety Mirror)` |
| issues | `/issues` open + `/headlines` recent | `EOS Issues {Team} (Strety Mirror)` |
| vto | `/visions`, `/roles_charts` + roles | `EOS VTO {Team} (Strety Mirror)`, `EOS Accountability Chart {Team} (Strety Mirror)` |

For each note: `search_notes` for the existing mirror note → `edit_note` (replace content) if found, `write_note` if not. Folder is `eos` — never a `brain/` prefix. Tags as YAML lists, e.g. `tags=["eos", "strety", "mirror", "rocks"]`.

Finish by upserting the sync log note (Data Structure below).

Rate-limit note: a full first sync of a large account can be 10–20 requests — the helper's 429 backoff handles it, but expect it to take a minute or two. Incremental syncs are usually 4–6 calls.

## Data Structure

Sync log note `Strety Sync Log` in folder `eos`:

```markdown
# Strety Sync Log

- **Last sync:** {ISO-8601}
- **Team:** {name} ({id})
- **Scope:** rocks, scorecard, todos, issues
- **Counts:** {N} rocks / {N} metrics / {N} todos / {N} issues
- **Mode:** incremental (updated_after {prev sync})
```

Mirror notes carry a header line so eos-operator users know provenance:

```markdown
> ⚡ Mirrored from Strety on {date} by /strety-sync. Strety is the system of
> record — edit there or via /strety-* skills; this note is overwritten on sync.
```

## Output Format

```
🔄 Strety Sync complete — {Team}

| Scope | Pulled | Note updated |
|---|---|---|
| Rocks | 6 | EOS Rocks Leadership (Strety Mirror) |
| Scorecard | 11 metrics | EOS Scorecard Leadership (Strety Mirror) |
| To-Dos | 14 open | EOS To-Dos Leadership (Strety Mirror) |
| Issues | 8 + 3 headlines | EOS Issues Leadership (Strety Mirror) |

eos-operator skills now see live data. Next: /eos-level10 or /strety-l10-prep.
```

## Example Usage

> **User:** sync Strety before my Monday L10
> **Claude:** Incremental pull since last sync, four mirror notes refreshed, sync log updated, table printed. Offers to schedule it weekly.

## Error Handling

- **No credentials** — route to `/strety-setup`.
- **Partial failure** (one scope errors) — finish the other scopes, report which one failed and why; don't update the sync log's `last sync` for the failed scope's benefit — note the gap in the log instead.
- **First sync on a big account** — warn it may take a couple of minutes under the 10 req/10s cap; proceed.
- **Mirror note edited by hand** — overwrite (the provenance header warns about this), but mention it happened.

## See Also

- `/eos-rocks`, `/eos-scorecard`, `/eos-todos`, `/eos-issues` — consume the mirror (from `eos-operator`)
- `/strety-l10-prep` — one-shot live digest when you don't need a persistent mirror
