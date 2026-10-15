# Strety Connect

**Version:** 1.0.0
**Publisher:** the provider
**Compatible with:** Claude / Cowork

---

## What This Plugin Does

Strety Connect wires Claude directly into [Strety](https://strety.com) — the EOS® software — via Strety's official REST API (beta). Manage your team's real To-Dos, Rocks, Scorecard, Issues, and Headlines from the terminal; walk into every L10 with a live prep pack; and mirror Strety into your Cloud Brain so the **EOS Operator** plugin's meeting runners work from the numbers your team actually sees.

Strety stays the system of record. This plugin reads and writes it surgically — no bulk two-way sync that could clobber teammates' edits.

---

## Skills Included

| Skill | What It Does | Example Triggers |
|-------|--------------|------------------|
| **strety-setup** | One-time guided OAuth connection. Tokens stored locally (`~/.strety/`, chmod 600), auto-refreshed. | "connect Strety", "set up Strety" |
| **strety-todos** | List/add/complete/update To-Dos live in Strety. | "my Strety to-dos", "add a to-do in Strety" |
| **strety-rocks** | Rocks (Strety Goals): list with status, create, check in, milestones, backlog. | "rock status", "check in on my rock" |
| **strety-scorecard** | View the scorecard with 13-week trailing trend, log weekly numbers, add measurables. Flags 2-week misses. | "log my numbers in Strety", "how's the scorecard" |
| **strety-issues** | Issues List + Headlines: add, list top-3 IDS candidates, resolve, headline capture. | "add an issue in Strety", "resolve that issue" |
| **strety-l10-prep** | One-shot pre-meeting digest: scorecard, rocks, to-dos completion rate, top issues, headlines. Hands off to `/eos-level10`. | "prep my L10" |
| **strety-sync** | One-way Strety → Cloud Brain mirror in `eos-operator`'s note shapes. Incremental after first run. | "sync Strety", "refresh the EOS mirror" |

---

## Setup

1. Install the plugin, then run `/strety-setup`.
2. You'll create an OAuth app in Strety (**My Integrations → My Apps**), paste back the client ID/secret, and authorize in your browser. Two minutes, once.
3. Credentials live in `~/.strety/credentials.json` — local only, never written to Cloud Brain. Access tokens (2-hour life) refresh automatically.

## API Notes

- Base: `https://2.strety.com/api/v1` (JSON:API format; OpenAPI spec at `/api/docs/v1/openapi.yaml`)
- Rate limits: 10 requests / 10 seconds per token (the bundled helper backs off on 429 automatically)
- PATCH requires an ETag `If-Match` header — the helper handles this
- All API plumbing lives in `scripts/strety.sh`; skills stay thin

## Works Great With

- **eos-operator** — `/strety-sync` populates its Cloud Brain notes; `/strety-l10-prep` feeds `/eos-level10`
- **business-operations** — pipe Strety scorecard data into `/bizops-kpi-dashboard` reviews

## Disclaimer

Strety® is a product of Strety, Inc. EOS® is a registered trademark of EOS Worldwide. This plugin is an independent integration built on Strety's public API and is not affiliated with, endorsed by, or certified by Strety, Inc. or EOS Worldwide. The Strety API is in beta and may change.


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
