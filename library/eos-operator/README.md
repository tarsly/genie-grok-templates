# EOS Operator

**Version:** 1.0.0
**Publisher:** the provider
**Compatible with:** Claude / Cowork

---

## What This Plugin Does

EOS Operator runs your business on the **Entrepreneurial Operating System** — the framework Gino Wickman lays out in *Traction*. Thirteen skills cover the full EOS toolkit: V/TO, Accountability Chart, Rocks, Scorecard, Issues, IDS, To-Dos, Level 10 Meetings, Quarterly Pulsing, Annual Planning, People Analyzer, Core Process documentation, and the EOS Organizational Checkup. Every skill reads from and writes to your Cloud Brain so every meeting builds on the last — no spreadsheets, no Notion sprawl, no losing your numbers between quarters.

---

## Skills Included

| Skill | What It Does | Example Triggers |
|-------|--------------|------------------|
| **eos-vto-builder** | Build/refresh the V/TO across all 8 questions (Core Values, Core Focus, 10-Year Target, Marketing Strategy, 3-Year Picture, 1-Year Plan, Rocks reference, Issues reference) | "build my V/TO", "update vision", "draft 3-year picture", "what are our core values" |
| **eos-accountability-chart** | Define seats, place people, GWC the seats. Hand off open or AI-eligible seats to `/agent-designer`. | "build accountability chart", "show org chart", "is [name] in the right seat" |
| **eos-rocks** | Quarterly rocks CRUD with SMART coach mode. Status tracking. ≥80% completion target. | "set Q3 rocks", "add a rock", "rock status", "rewrite this rock" |
| **eos-scorecard** | 5-15 weekly leading-indicator measurables. 13-week trailing view with sparklines. Auto-flags off-track misses as Issues. | "update scorecard", "scorecard report", "what should sales measure" |
| **eos-issues** | Short-term + long-term issues list. Prioritize top-3 for next L10. | "add issue", "show issues", "top 3 issues for L10" |
| **eos-ids** | Conversational IDS facilitator. Drives Identify → Discuss → Solve with a real owner on the solve. | "IDS this", "we have an issue with X", "solve this" |
| **eos-todos** | 7-day action items. ≥90% completion target. Auto-rolls 2-week stalls into Issues. | "add a to-do", "open to-dos", "weekly recap" |
| **eos-level10** | The 90-minute weekly L10 with full timing per segment and complete capture. Pre-meeting digest from scorecard, rocks, todos, issues. Post-meeting cascading message. | "run L10", "prep my L10", "L10 notes" |
| **eos-quarterly-pulsing** | Full-day Quarterly Pulsing meeting runner. Previous-quarter recap → V/TO review → new Rocks. | "quarterly pulse", "Q-pulse agenda", "review the quarter" |
| **eos-annual-planning** | 2-day annual offsite. Refresh 3-Year Picture + 1-Year Plan, set Q1 Rocks. | "annual offsite", "prep annual planning" |
| **eos-people-analyzer** | Score each leader on Core Values + GWC. Above The Bar / Below The Bar verdict + Three Strikes tracking. | "people analyzer", "rate the team", "is [name] above the bar" |
| **eos-process-docs** | Document a Core Process at the FBA 20% level. | "document my sales process", "build the HR FBA" |
| **eos-health-check** | The 20-question EOS Organizational Checkup. Scores 0-100% across the Six Components. | "EOS checkup", "health check", "how strong are we on EOS" |

---

## The EOS Cadence

| Rhythm | Skill(s) | Cadence |
|--------|----------|---------|
| Daily | (no skill — use a stand-up huddle) | 5-15 min daily |
| Weekly | `eos-level10` → `eos-scorecard` → `eos-rocks` → `eos-issues` → `eos-ids` → `eos-todos` | 90 min weekly |
| Quarterly | `eos-quarterly-pulsing` → `eos-rocks` → `eos-vto-builder` (Traction side) | 1 day quarterly |
| Annually | `eos-annual-planning` → `eos-vto-builder` (full refresh) → `eos-process-docs` review | 2 days annually |
| As needed | `eos-accountability-chart`, `eos-people-analyzer`, `eos-process-docs`, `eos-health-check` | Quarterly+ |

---

## The Three Numbers EOS Companies Watch

| Number | Target | Surfaced in |
|--------|--------|-------------|
| Rock completion rate per quarter | **≥80%** | `eos-rocks`, `eos-quarterly-pulsing` |
| To-Do completion rate per L10 | **≥90%** | `eos-todos`, `eos-level10` |
| L10 meeting rating (1-10) | **≥8 average** | `eos-level10` |

Plus the **EOS Organizational Checkup** score from `eos-health-check` (target: 80%+ = "Strong on EOS").

---

## Cloud Brain Integration

Every EOS skill stores state in your Cloud Brain (folder: `eos`). Skills cross-read each other on every invocation — so when you run `/eos-level10`, the meeting agenda already has your live scorecard numbers, current rock status, last week's to-dos, and the issues list ready to IDS.

Skills also read your existing onboarding context if you've run 01 Clarify Your Goals — `business blueprint`, `goal-hierarchy`, `quarterly-priorities`, and `language-and-frameworks` notes are auto-loaded so you don't re-enter your company name, industry, or current commitments.

---

## Dashboard Publishing

Most report-shaped skills (L10, Scorecard, Rocks, Quarterly, Annual, V/TO, Org Chart, Health Check) support `--publish` to push a card to your `the provider` cloud dashboard. The visualization uses progress bars, sparklines, trend arrows, and status badges to make the EOS health of your business readable at a glance.

People Analyzer, Issues, and IDS outputs do **not** auto-publish — they often contain sensitive leadership content. Opt-in only.

---

## Seats ↔ AI Agents

`eos-accountability-chart` lets you tag each seat as `human`, `ai`, `mixed`, or `open`. When you have open or AI-eligible seats, the skill hands them off to `/agent-designer` (from the **AI Agents** plugin) pre-loaded — so your EOS org chart and your AI agent team stay in sync without double-entry.

---

## Trademark Notice

EOS®, Entrepreneurial Operating System®, V/TO™, Level 10 Meeting™, Rocks™, GWC™, IDS™, and People Analyzer™ are trademarks of EOS Worldwide, LLC. This plugin helps you implement concepts from Gino Wickman's book *Traction* and the broader EOS methodology. It is not affiliated with, endorsed by, or sponsored by EOS Worldwide, LLC.


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
