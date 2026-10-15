# Strety Connect — AGENTS.md

*Read by Agent Designer and Agent Activator to understand what this plugin provides.*

---

## About This Plugin

Strety Connect integrates Claude with Strety (EOS® software) through Strety's official REST API. Seven skills: a one-time OAuth setup, four live CRUD skills (To-Dos, Rocks/Goals, Scorecard/Metrics, Issues+Headlines), an L10 prep-pack builder, and a one-way Strety → Cloud Brain mirror that feeds the `eos-operator` plugin. All API plumbing (token refresh, ETag PATCH, 429 backoff) is centralized in `scripts/strety.sh`.

---

## Skills Catalog

| Skill | Trigger Phrases | What It Does |
|---|---|---|
| `strety-setup` | "connect Strety", "set up Strety", "Strety token expired" | Guided OAuth app creation + authorization-code flow; stores tokens at `~/.strety/credentials.json`; saves team/person defaults to Cloud Brain |
| `strety-todos` | "my Strety to-dos", "add a to-do in Strety", "mark that done in Strety" | Live To-Do CRUD with EOS hygiene coaching (verb-led, one owner, ≤7 days) |
| `strety-rocks` | "rock status", "check in on my rock", "add a milestone" | Rocks via Strety Goals: list, create, check-ins, milestones, backlog |
| `strety-scorecard` | "log my numbers in Strety", "Strety scorecard", "how are we tracking" | Metrics + weekly check-ins, 13-week trailing view, 2-week-miss flagging |
| `strety-issues` | "add an issue in Strety", "resolve that issue", "add a headline" | Issues List + Headlines; top-3 IDS candidates; resolve-with-todo handoff |
| `strety-l10-prep` | "prep my L10", "brief me before the weekly" | Five-call live digest across scorecard/rocks/todos/issues/headlines with EOS health flags; hands off to `/eos-level10` |
| `strety-sync` | "sync Strety", "refresh the EOS mirror" | One-way incremental Strety → Cloud Brain mirror in eos-operator note shapes; keeps a sync log |

---

## Preferences Registry

| Skill | Preferences Read | Path |
|---|---|---|
| All strety skills | Strety defaults (team/space id, person id, email) | `preferences/Strety Preferences` (cloud-brain search: "strety preferences") |
| `strety-sync` | Last sync timestamp for incremental pulls | `eos/Strety Sync Log` |

Secrets (OAuth client + tokens) are **never** in Cloud Brain — local file `~/.strety/credentials.json` only.

---

## Agent Design Notes

- Good fit for a scheduled agent: `/strety-sync` weekly before the L10, or `/strety-l10-prep` every meeting morning.
- Rate limit is 10 req/10s per token — agents batching many writes should pace themselves; the helper backs off on 429 but pacing avoids the stall.
- Strety is the system of record. Agents should write through the strety-* skills (surgical, ETag-guarded), never bulk-push mirrored notes back.
