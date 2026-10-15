# EOS Operator — AGENTS.md

*Read by Agent Designer and Agent Activator to understand what this plugin provides.*

---

## About This Plugin

EOS Operator is the complete Entrepreneurial Operating System toolkit for businesses running on (or moving to) EOS. Thirteen skills cover the entire EOS Toolbox — V/TO, Accountability Chart, Rocks, Scorecard, Issues, IDS, To-Dos, Level 10 weekly meeting, Quarterly Pulsing, Annual Planning, People Analyzer, Core Process documentation, and the EOS Organizational Checkup. All state lives in Cloud Brain so meetings build on each other across weeks, quarters, and years.

---

## Skills Catalog

| Skill | Trigger Phrases | What It Does |
|---|---|---|
| `eos-vto-builder` | "build my V/TO", "update vision", "core values", "3-year picture", "10-year target" | Walks through the 8 V/TO questions and writes a versioned snapshot to Cloud Brain |
| `eos-accountability-chart` | "build accountability chart", "show org chart", "is [name] in the right seat", "open seats" | Functional seats with GWC and `staffing` tag; hands AI-eligible seats to `/agent-designer` |
| `eos-rocks` | "set Q3 rocks", "add a rock", "rock status", "rewrite this rock", "what should my rocks be" | Quarterly rocks CRUD + SMART coach. Tracks On Track / Off Track / Done. ≥80% target |
| `eos-scorecard` | "update scorecard", "scorecard report", "log my numbers", "what should I measure" | 5-15 weekly leading-indicator measurables with 13-week trailing view. Auto-flags off-track misses to Issues |
| `eos-issues` | "add issue", "show issues", "top 3 issues" | Short-term + long-term issues list |
| `eos-ids` | "IDS this", "we have an issue with X", "solve this", "drill into this" | Conversational Identify → Discuss → Solve facilitator. Spawns a To-Do or Rock on solve |
| `eos-todos` | "add a to-do", "open to-dos", "weekly recap" | 7-day action items with ≥90% completion target; auto-rolls 2-week stalls into Issues |
| `eos-level10` | "run L10", "prep my L10", "weekly meeting", "L10 notes" | The 90-minute weekly L10 with full per-segment timer. Pre-meeting digest + post-meeting cascading message |
| `eos-quarterly-pulsing` | "quarterly pulse", "Q-pulse agenda", "review the quarter" | Full-day quarterly meeting runner |
| `eos-annual-planning` | "annual offsite", "prep annual planning", "yearly planning" | 2-day annual planning workbook + runner |
| `eos-people-analyzer` | "people analyzer", "rate the team", "is [name] above the bar" | Core Values + GWC scoring with Three Strikes tracking |
| `eos-process-docs` | "document my sales process", "build the HR FBA", "core process" | Core process documentation at the FBA 20% level |
| `eos-health-check` | "EOS checkup", "health check", "how strong are we on EOS" | The 20-question EOS Organizational Checkup scored 0-100% across the Six Components |

---

## Preferences Registry

| Skill | Preferences Read | Path |
|---|---|---|
| All EOS skills | EOS company preferences | `brain/preferences/eos-company-preferences` |
| All EOS skills | Business blueprint (from system 01 onboarding) | `business blueprint` (cloud-brain search) |
| `eos-vto-builder`, `eos-rocks` | Goal hierarchy + quarterly priorities (from 01 Clarify Your Goals) | `goal-hierarchy`, `quarterly-priorities` |
| All EOS skills | Language and framework preference (EOS vs plain) | `preferences/language-and-frameworks` |

No skill in this plugin maintains its own standalone preferences file — they all share `eos-company-preferences`. If the user has gone through the system 01 onboarding, the EOS skills pre-fill from those notes and confirm rather than re-asking.

---

## Cloud Brain Folder

All EOS data goes in the `eos/` folder. Discrimination by note title and tags. Key notes:

- `EOS V/TO` — current Vision/Traction Organizer
- `EOS Accountability Chart` — current org chart with staffing tags
- `EOS Core Values` — core values + behavioral examples
- `EOS Rocks — YYYY QN` — quarterly rock index
- `Rock — {Quarter} — {Owner} — {Title}` — individual rocks
- `EOS Scorecard` — measurables + 13-week rolling log
- `EOS Issues List` — short-term + long-term
- `EOS To-Dos` — 7-day action items
- `EOS L10 — YYYY-MM-DD` — per-meeting minutes
- `EOS Quarterly — YYYY QN` — quarterly meeting minutes
- `EOS Annual — YYYY` — annual meeting minutes
- `EOS Process — {Name}` — per documented core process
- `EOS People Analyzer — {Name} — YYYY QN` — quarterly people ratings
- `EOS Health Check — YYYY-MM-DD` — organizational checkup score

---

## Cross-Plugin Integration

- **System 01 Onboarding** — Every EOS skill reads `business blueprint`, `goal-hierarchy`, `quarterly-priorities`, `language-and-frameworks` on first run and pre-fills.
- **AI Agents plugin** — `eos-accountability-chart` writes a `EOS Accountability Chart — Agent Candidates` note that `agent-designer` picks up; conversely it reads the `Agent Team` note on every run to auto-flag seats already staffed by AI (🤖).
- **Cloud Dashboard** — Most report-shaped skills support `--publish` to push a card via `mcp__cloud-dashboard__publish_report`. People Analyzer, Issues, and IDS do NOT auto-publish.

---

## Suggested Configuration

### Configuration A — Full Cadence (recommended for teams running EOS)

Assign to the Integrator or an Executive Assistant agent:

- **eos-level10** — Weekly, your chosen L10 day (commonly Monday 9:00 AM)
- **eos-scorecard** — Weekly, the morning of L10 (~30 min before) for owners to log numbers
- **eos-quarterly-pulsing** — Quarterly, first business day of each quarter
- **eos-annual-planning** — Annually, second week of December
- **eos-health-check** — Quarterly, the day before Quarterly Pulsing
- **eos-people-analyzer** — Quarterly, before Quarterly Pulsing

### Configuration B — On-Demand

Run any skill any time by typing the trigger phrase. No scheduling required. Good for self-implementers reading *Traction* and getting started.

---

## Schedules Table

| Task | Skill | Suggested Schedule | Notes |
|---|---|---|---|
| Weekly L10 meeting | `eos-level10` | Weekly, your L10 day/time | Pulls scorecard + rocks + todos + issues |
| Scorecard logging | `eos-scorecard` | Weekly, day-of L10 (early) | Owners log numbers before L10 |
| Quarterly Pulsing | `eos-quarterly-pulsing` | Quarterly, day 1 of quarter | Full-day; sets new Rocks |
| Annual Planning | `eos-annual-planning` | Annually, mid-December | 2-day offsite |
| EOS Organizational Checkup | `eos-health-check` | Quarterly | Tracks 6-component score over time |
| People Analyzer | `eos-people-analyzer` | Quarterly | Before each Quarterly Pulsing |
| Publish L10 recap | `eos-level10 --publish` | After each L10 | Pushes to cloud dashboard |
| Publish quarterly recap | `eos-quarterly-pulsing --publish` | After each Quarterly | Pushes to cloud dashboard |
