# BizOps — Business Operations Suite

**Version:** 1.1.0  
**Publisher:** the provider  
**Compatible with:** Claude / Cowork

---

## What This Plugin Does

BizOps is a complete business operations toolkit for small business owners across all industries. It covers the full operational loop: start the day, manage clients, track deals, handle money, build your team, document processes, and plan travel — all powered by a persistent AI brain that learns your preferences over time.

Every skill reads your saved preferences from memory on first run, so it adapts to your business rather than starting generic every time.

---

## Skills Included

| Skill | What It Does | Example Triggers |
|-------|-------------|-----------------|
| **bizops-daily-brief** | CEO-level morning briefing — priorities, fires, deadlines, project status | "daily brief", "start my day", "what's on my plate" |
| **bizops-client-onboarding** | Full new client setup — contact, project file, welcome email, checklist, kickoff agenda | "onboard [client name]", "new client", "set up a client" |
| **bizops-invoice-generator** | Professional invoice creation and billing history log | "create an invoice", "invoice [client]", "bill for [service]" |
| **bizops-pipeline-sync** | Pipeline health, deal velocity, stale deals, revenue forecast | "pipeline report", "deal flow", "CRM sync" |
| **bizops-lead-tracker** | Add, update, and manage leads through pipeline stages | "track a lead", "add a lead", "update a deal" |
| **bizops-follow-up** | Track follow-ups and open commitments, get reminders | "follow up on [name]", "open loops", "what do I owe" |
| **bizops-financial-snapshot** | P&L, revenue by stream, cash flow projection, margin analysis | "financial snapshot", "P&L summary", "how's our cash flow" |
| **bizops-kpi-dashboard** | Define, update, and track key performance indicators | "KPI report", "update my metrics", "business scorecard" |
| **bizops-hiring-screener** | Job descriptions, screening questions, scorecards, offer templates | "hire a VA", "job description for [role]", "interview scorecard" |
| **bizops-sop-builder** | Turn any described process into a formal, AI-executable SOP | "build an SOP for [process]", "document this workflow" |
| **bizops-people** | Contact management — create, lookup, and update people in memory | "who is [name]?", "add [name] to contacts", "update [name]'s email" |
| **bizops-weekly-review** | Weekly accountability check — what got done, what slipped, plan ahead | "weekly review", "week in review", "plan next week" |
| **bizops-travel-plan** | Full trip planning — flights, hotels, itinerary, restaurants, packing | "plan my trip to [city]", "trip itinerary", "packing list for [destination]" |
| **bizops-budget-builder** | Build a budget, log actuals, analyze variances, flag overspend | "build a budget", "budget vs actuals", "variance report", "am I over budget" |
| **bizops-vendor-tracker** | Track vendors, contracts, renewal dates, costs, and spend | "add a vendor", "vendor renewals", "vendor audit", "what am I paying for" |
| **bizops-project-tracker** | Multi-milestone project tracking with owners, phases, blockers | "create a project", "project status", "project dashboard", "what's overdue" |

---

## Three-Phase Revenue Cycle

The pipeline and sales skills form a complete revenue cycle:

1. **Lead Tracker (Phase 0)** — capture and manage individual leads through your pipeline stages
2. **Pipeline Sync (Phase 1)** — analyze your pipeline: velocity, stale deals, revenue forecast, top deals to close
3. **Client Onboarding** — once a deal closes, immediately set up the client relationship

---

## Two-Phase Design

Some skills work best in pairs:

- **Pipeline:** `bizops-lead-tracker` (capture and track) → `bizops-pipeline-sync` (analyze and forecast)
- **Hiring:** `bizops-hiring-screener` (build the package) → use the scorecard during actual interviews
- **Clients:** `bizops-client-onboarding` (set up everything) → `bizops-follow-up` (track open items)

---

## Preferences Layer

Each skill that needs personalization checks Cloud Brain for a saved preferences note before running. On first use, it collects your preferences, saves them, and applies them going forward. You can update preferences at any time by saying "update my [skill] preferences."

**Preference notes are saved to:**  
`brain/preferences/[skill]-preferences.md`

---

## Memory Paths

This plugin reads and writes to the following Cloud Brain locations:

| Path | Contents |
|------|----------|
| `brain/preferences/` | Per-skill user preferences |
| `brain/projects/` | Client project files and onboarding checklists |
| `brain/pipeline/` | Deal and lead data |
| `people/` | Contact records |
| `brain/invoices/` | Invoice logs |
| `brain/drafts/` | Email drafts, agendas, welcome messages |
| `brain/sops/` | Standard operating procedures |
| `brain/budgets/` | Budget files with actuals |
| `brain/vendors/` | Vendor records and contracts |

---

## Version History

### v1.1.0 — 2026-06-12
- Added 3 new skills: `bizops-budget-builder`, `bizops-vendor-tracker`, `bizops-project-tracker`
- Added preferences layer to all 16 skills (full compliance with build guide)
- Added regulated disclaimers to `bizops-financial-snapshot` and `bizops-hiring-screener`
- Documented Lead Tracker + Pipeline Sync as Phase 0/1 pair
- Fixed `plugin.json` spec compliance (author format, keywords, memory_provider)

### v1.0.0 — 2026-06-08
- Initial release — 13 skills covering the full business operations lifecycle
- Industry-neutral: works for any business type
- Cloud Brain integration throughout for persistent context

---

## Requirements

- Claude / Cowork with Cloud Brain MCP connected
- No other integrations required (optional: calendar, email, CRM MCPs enhance some skills)

---

*Built by the provider — the provider*


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
