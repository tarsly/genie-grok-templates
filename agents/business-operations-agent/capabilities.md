# BizOps — Agent Capabilities Manifest

*For use by system-03 (Agent Designer) and system-04 (Agent Activator)*  
*Last updated: 2026-06-12*

---

## About This Plugin

`business-operations` is a complete business operations toolkit for small business owners across all industries. It covers the full operational loop: start the day with clarity, manage clients and contacts, track leads and deals, generate invoices, monitor financial performance, track projects and vendors, build your team, document processes, and plan travel — all powered by a persistent Cloud Brain that learns your preferences over time.

**Target users:** Solo entrepreneurs, small business owners, and operations-focused professionals in any industry. Especially valuable for owners who want to systematize their business, get visibility into their pipeline and finances, and build the kind of operational infrastructure that lets them scale.

**Plugin version:** 1.1.0 (16 skills)

---

## Available Skills Catalog

Each entry lists: what the skill does, what preferences it collects at activation, its recommended schedule, and which other skills it pairs with naturally.

---

### 1. bizops-daily-brief

**What it does:** CEO-level morning briefing that pulls priorities, open fires, deadlines, project status, and focus areas from the user's goals, projects, calendar context, and recent activity. Starts every day with a structured executive summary so nothing falls through the cracks.

**Preferences collected at activation:**
- Business name and primary industry
- Number of priorities to surface each day (default: 3)
- Preferred briefing sections (e.g., top priorities, overdue follow-ups, project status, financial pulse, today's goals)
- Whether to include a motivational framing (yes/no)

**Suggested schedule:** Daily, morning — sets up the day.

**Natural pairings:** bizops-follow-up (surfaces overdue open loops), bizops-project-tracker (shows project status in briefing), bizops-kpi-dashboard (pulls in key metrics), bizops-weekly-review (complements end-of-week accountability check).

---

### 2. bizops-client-onboarding

**What it does:** Sets up everything needed to start a new client relationship — creates a contact record, project file, welcome email draft, onboarding checklist, and kickoff agenda. Ensures every new client gets a professional, consistent experience from day one.

**Preferences collected at activation:**
- Business name and service type (consulting / agency / contractor / other)
- Standard onboarding steps for your business (e.g., signed contract, deposit collected, kickoff call scheduled)
- Default payment terms (Net 15 / Net 30 / due on receipt / 50% upfront)
- Kickoff call format preference (video / phone / in-person)
- Welcome email tone (warm and personal / professional / brief)

**Suggested schedule:** On-demand — triggered when a new client is signed.

**Natural pairings:** bizops-people (creates the contact record), bizops-project-tracker (creates the client project), bizops-invoice-generator (generates the first invoice), bizops-follow-up (captures open commitments from kickoff).

---

### 3. bizops-invoice-generator

**What it does:** Creates professional invoices and logs billing history to memory. Handles project-based, hourly, retainer, and milestone billing. Maintains an invoice log so the owner always knows what's been billed, paid, and outstanding.

**Preferences collected at activation:**
- Business name, address, and contact info (for invoice header)
- Default payment terms (Net 15 / Net 30 / due on receipt)
- Standard services and rates (can be added at invoice time)
- Tax rate (if applicable)
- Accepted payment methods (bank transfer / card / PayPal / check / other)
- Invoice number format/prefix (e.g., INV-001 or 2026-001)

**Suggested schedule:** On-demand — triggered when billing a client.

**Natural pairings:** bizops-client-onboarding (first invoice often created at onboarding), bizops-financial-snapshot (invoice totals feed revenue tracking), bizops-people (client contact details pre-populated from people notes).

---

### 4. bizops-pipeline-sync

**What it does:** Pipeline intelligence and reporting layer. Analyzes deal velocity, identifies stale deals, calculates revenue forecast, ranks top deals to close this month, and produces a pipeline health report. This is Phase 1 — reads the data captured by Lead Tracker (Phase 0).

**Preferences collected at activation:**
- Business name and industry
- Pipeline stages used (customize for your sales process)
- Monthly / quarterly revenue goal
- Stale deal threshold (days without activity before flagging — default: 14)
- Forecast confidence levels by stage (e.g., 25% at Prospect, 75% at Proposal Sent)

**Suggested schedule:** Weekly — Monday morning pipeline review.

**Natural pairings:** bizops-lead-tracker (Phase 0 data source), bizops-financial-snapshot (pipeline forecast feeds revenue projections), bizops-kpi-dashboard (deal velocity is a core KPI), bizops-follow-up (stale deals generate follow-up items).

---

### 5. bizops-lead-tracker

**What it does:** Data entry and status management layer for the sales pipeline. Add new leads, update deal stages, log notes, track lead sources, and manage the pipeline record by record. This is Phase 0 — feeds Pipeline Sync (Phase 1).

**Preferences collected at activation:**
- Business name and industry
- Pipeline stages (same as pipeline-sync — set once, share across both skills)
- Lead sources to track (referral / website / outbound / social / event / other)
- Default currency
- Average deal size (for context when reviewing pipeline)

**Suggested schedule:** On-demand — triggered when adding or updating a lead.

**Natural pairings:** bizops-pipeline-sync (Phase 1 reads this data), bizops-people (lead contact records), bizops-follow-up (tracks commitments to leads), bizops-client-onboarding (triggered when a lead converts to a client).

---

### 6. bizops-follow-up

**What it does:** Tracks all follow-ups, open commitments, and pending items across the business. Add promises, set due dates, mark complete, and get alerts for overdue items. Prevents things from falling through the cracks when the owner says "I'll get back to you" or "send me that by Friday."

**Preferences collected at activation:**
- Business name
- Default follow-up reminder window (how many days before due date to surface — default: 1 day)
- Overdue flag threshold (how many days past due before escalating — default: 3 days)
- Whether to group follow-ups by person or by due date in reports

**Suggested schedule:** Daily — surfaces today's and overdue follow-ups. Integrates naturally into daily-brief.

**Natural pairings:** bizops-daily-brief (follow-ups feed the morning brief), bizops-lead-tracker (commitments to leads tracked here), bizops-client-onboarding (kickoff commitments captured), bizops-weekly-review (follow-up completion rate reviewed weekly).

---

### 7. bizops-financial-snapshot

**What it does:** Executive financial summary for non-accountants. Captures P&L by revenue stream, expense breakdown, cash flow projection, burn rate, runway estimate, margin analysis, and profitability trend. Produces actionable recommendations. Designed for business owners who want financial clarity without reading a spreadsheet.

**Preferences collected at activation:**
- Business name and industry
- Revenue streams (list each source of income separately)
- Fiscal year start month
- Primary reporting currency
- Monthly expense categories relevant to the business
- Key financial metric to watch most closely (cash runway / profit margin / MRR growth / other)

**Suggested schedule:** Monthly — financial pulse at the start or end of each month.

**Natural pairings:** bizops-budget-builder (budget vs. actuals comparison), bizops-kpi-dashboard (financial KPIs tracked here), bizops-weekly-review (financial highlights surface in weekly review), bizops-invoice-generator (outstanding invoices affect cash flow).

---

### 8. bizops-kpi-dashboard

**What it does:** Defines, tracks, and reports on the business's key performance indicators. Owner defines their KPIs once (revenue, conversion rate, customer count, churn rate, deal velocity, etc.), then updates actuals on a recurring basis. Produces a scorecard with trend analysis and target status.

**Preferences collected at activation:**
- Business name and industry
- KPIs to track (ask the owner to list 5–10 metrics that matter most)
- Current baseline values for each KPI
- Targets for each KPI (monthly / quarterly / annual)
- Reporting period preference (weekly / monthly / quarterly)

**Suggested schedule:** Weekly or monthly — depends on business cadence. Suggest weekly for fast-moving businesses, monthly for service businesses.

**Natural pairings:** bizops-financial-snapshot (financial KPIs share data), bizops-daily-brief (top KPIs surface in morning brief), bizops-weekly-review (KPI progress reviewed weekly), bizops-pipeline-sync (pipeline metrics are a key KPI category).

---

### 9. bizops-hiring-screener

**What it does:** Complete hiring package builder. Generates job descriptions, ideal candidate profiles, screening questions, interview scorecards, red/green flag guides, offer letter templates, and onboarding checklists for any role — VA, contractor, or full-time employee.

**Preferences collected at activation:**
- Business name and industry
- Typical roles hired (VA / contractor / full-time / part-time)
- Compensation structure (hourly / salary / project-based / equity)
- Hiring process steps used (e.g., application review → screening call → skills test → interview → reference check)
- Where the business typically posts jobs (Indeed / LinkedIn / referrals / Upwork / other)

**Suggested schedule:** On-demand — triggered when opening a new role.

**Natural pairings:** bizops-project-tracker (manage the hiring process as a project), bizops-people (hired candidates become contacts), bizops-sop-builder (document the role's key processes once hired).

---

### 10. bizops-sop-builder

**What it does:** Turns any described business process into a formal, step-by-step Standard Operating Procedure. Captures the process, structures it into executable steps, assigns owners, adds decision points, and saves it to memory so it can be handed off to a team member or AI agent.

**Preferences collected at activation:**
- Business name and industry
- SOP categories relevant to the business (e.g., Client Delivery, Sales, Finance, HR, Operations, Marketing)
- Preferred SOP format (numbered steps / checklist / flowchart description)
- Whether SOPs should include a "Who Does This" owner field

**Suggested schedule:** On-demand — triggered when documenting a process for the first time or standardizing a repeatable workflow.

**Natural pairings:** bizops-hiring-screener (SOPs make onboarding new hires easier), bizops-client-onboarding (the client onboarding process is an SOP candidate), bizops-project-tracker (complex projects often generate new SOPs).

---

### 11. bizops-people

**What it does:** Contact management system for the business brain. Create, search, update, and manage contact records for clients, leads, vendors, partners, team members, and referral sources. The central address book that all other skills reference.

**Preferences collected at activation:**
- Business name
- Contact categories used (e.g., Client, Lead, Vendor, Partner, Team, Referral Source, Prospect)
- Required fields for new contacts (e.g., name, email, phone, company, category, notes)

**Suggested schedule:** On-demand — triggered when adding or looking up a contact.

**Natural pairings:** bizops-client-onboarding (clients become contacts), bizops-lead-tracker (leads are contacts), bizops-hiring-screener (candidates become contacts), bizops-follow-up (follow-ups are often linked to specific contacts), bizops-vendor-tracker (vendor reps are contacts).

---

### 12. bizops-weekly-review

**What it does:** End-of-week accountability check and planning session. Reviews what got done, what slipped, goal progress across business domains, open follow-ups, and pipeline movement. Then plans the top priorities for the coming week. Keeps the owner honest about progress and focused on what matters.

**Preferences collected at activation:**
- Business name
- Week start day preference (Sunday / Monday)
- Goal domains to review each week (e.g., Revenue, Client Delivery, Team, Marketing, Personal)
- Whether to include personal goals or business only
- Preferred review format (narrative summary / structured scorecard / action list only)

**Suggested schedule:** Weekly — Friday afternoon or Sunday evening.

**Natural pairings:** bizops-daily-brief (weekly review feeds the following week's briefings), bizops-kpi-dashboard (KPI progress is a core review section), bizops-follow-up (overdue follow-ups surface in review), bizops-project-tracker (project status reviewed weekly).

---

### 13. bizops-travel-plan

**What it does:** Full trip planning from start to finish — flight search strategy, hotel recommendations, daily itinerary, ground transportation, restaurant suggestions, packing checklist, and a pre-trip preparation timeline. Handles business trips, conferences, and road trips.

**Preferences collected at activation:**
- Business name (for itinerary header)
- Home city and nearest airport
- Airline preferences (preferred carrier / frequent flyer programs)
- Hotel style preference (boutique / business / budget / chain)
- Typical travel purpose (client meetings / conferences / site visits / mixed)
- Dietary restrictions or preferences (for restaurant suggestions)

**Suggested schedule:** On-demand — triggered when planning a trip.

**Natural pairings:** bizops-follow-up (pre-trip prep items tracked as follow-ups), bizops-project-tracker (conference or site visit can be tracked as a project), bizops-client-onboarding (travel often follows a new client win).

---

### 14. bizops-budget-builder

**What it does:** Build a business budget, log actuals against it, and get variance analysis with flags and recommendations. Works in three modes: Setup (build the budget), Update (log actuals), and Report (variance analysis). Designed for business owners who want to plan their money rather than react to it.

**Preferences collected at activation:**
- Business name and industry
- Budget period (annual / quarterly — quarterly recommended for most small businesses)
- Fiscal year start month
- Primary currency
- Major expense categories (e.g., payroll, rent, software, marketing, COGS, contractors)

**Suggested schedule:** Monthly — update actuals and run variance report at the end of each month.

**Natural pairings:** bizops-financial-snapshot (budget vs. actuals context), bizops-vendor-tracker (vendor costs feed expense categories), bizops-kpi-dashboard (budget performance is a financial KPI).

---

### 15. bizops-vendor-tracker

**What it does:** Track all vendors, suppliers, software subscriptions, and contractors — contract terms, renewal dates, costs, and contacts. Flags upcoming renewals, runs spend audits, and identifies unused or overlapping subscriptions. Most small business owners have no idea what they're paying vendors in total — this skill fixes that.

**Preferences collected at activation:**
- Business name
- Vendor categories relevant to the business (software/SaaS, contractors, suppliers, utilities, marketing services, professional services)
- Renewal alert window (days ahead to flag upcoming renewals — default: 30 days)
- Monthly spend threshold for high-cost flag (e.g., anything over $500/month)

**Suggested schedule:** Monthly — renewal alert check and spend audit.

**Natural pairings:** bizops-budget-builder (vendor costs feed expense categories), bizops-financial-snapshot (vendor spend context for P&L), bizops-project-tracker (large vendor contracts often map to a project).

---

### 16. bizops-project-tracker

**What it does:** Multi-milestone internal project tracking. Create projects with phases, owners, due dates, and priority. Update milestone status, flag blockers, view a dashboard of all active projects, and close completed initiatives. For multi-step internal initiatives — product launches, website builds, process rollouts, hiring campaigns, location openings.

**Preferences collected at activation:**
- Business name
- Default project categories (e.g., Marketing, Operations, Product, Hiring, Technology, Facilities)
- Overdue threshold (days past milestone due date before flagging 🚨 — default: 3 days)
- Default milestone owners (e.g., "me" or specific team member names)

**Suggested schedule:** Weekly — project health check alongside weekly review.

**Natural pairings:** bizops-follow-up (individual commitments from project milestones tracked here), bizops-client-onboarding (creates a client project automatically), bizops-hiring-screener (manage the hiring process as a project), bizops-daily-brief (project status in morning brief).

---

## Preferences Registry

All preferences that can be collected at business owner activation, organized by category. system-04 should collect the union of all fields needed by the assigned skills and run one consolidated interview.

**Core Identity — needed by most skills:**
- Business name
- Primary industry
- Owner's name
- Business email and phone (for document headers and invoice footers)
- Business address (for invoices and travel planning)

**Sales and Pipeline:**
- Pipeline stages (used by both lead-tracker and pipeline-sync — set once)
- Lead sources to track
- Monthly / quarterly revenue goal
- Average deal size
- Stale deal threshold (days without activity)
- Forecast confidence levels by stage

**Client and Operations:**
- Service types offered
- Standard onboarding steps
- Default payment terms
- Invoice number format/prefix
- Tax rate (if applicable)
- Accepted payment methods
- Kickoff call format preference

**Financial:**
- Revenue streams (list each separately)
- Fiscal year start month
- Primary currency
- Major expense categories
- Budget period preference (annual / quarterly)
- Key financial metric to watch most closely

**Team and Hiring:**
- Typical roles hired
- Compensation structure used
- Hiring process steps
- Job posting channels

**Vendors:**
- Vendor categories in use
- Renewal alert window
- Monthly high-cost vendor threshold

**Projects:**
- Default project categories
- Overdue threshold (days)
- Default milestone owners

**KPIs:**
- KPIs to track (5–10 metrics)
- Baseline values for each KPI
- Targets per KPI
- Reporting period preference

**People / Contacts:**
- Contact categories used
- Required fields for new contacts

**Weekly Review:**
- Week start day
- Goal domains to review
- Whether to include personal goals
- Preferred review format

**Daily Brief:**
- Number of priorities to surface each day
- Preferred briefing sections

**Travel:**
- Home city and nearest airport
- Airline preferences / frequent flyer programs
- Hotel style preference
- Dietary restrictions or preferences

**SOPs:**
- SOP categories for the business
- Preferred SOP format

---

## Suggested Agent Configurations

**These are starting-point suggestions only.** Clients are free to design any configuration. Skills can be assigned to any agent in any combination. The goal is to match the configuration to how the owner actually thinks about their business.

---

### Option A: Solo Business Assistant (Most Common for Small Teams)

One "Business Operations Assistant" handles everything. Assign all 16 skills to one agent. Activation collects all preferences in one interview.

**Best for:** Solo entrepreneurs, early-stage businesses, owners who want simplicity over segmentation, or anyone who wants to start with one agent and split later.

**Activation interview:** All fields from the Preferences Registry above. Takes approximately 10–15 minutes once. All 16 skills pre-loaded and ready.

---

### Option B: Two-Agent Setup (Revenue + Operations)

**Revenue and Client Agent**
Skills: bizops-lead-tracker, bizops-pipeline-sync, bizops-client-onboarding, bizops-invoice-generator, bizops-follow-up, bizops-people

Activation fields: Core Identity, Sales and Pipeline, Client and Operations, People

**Operations and Finance Agent**
Skills: bizops-daily-brief, bizops-financial-snapshot, bizops-kpi-dashboard, bizops-budget-builder, bizops-vendor-tracker, bizops-project-tracker, bizops-hiring-screener, bizops-sop-builder, bizops-weekly-review, bizops-travel-plan

Activation fields: Core Identity, Financial, Vendors, Projects, Team and Hiring, KPIs, Weekly Review, Daily Brief, Travel, SOPs

---

### Option C: Three-Agent Team (Revenue / Ops / Finance)

**Revenue Agent**
Skills: bizops-lead-tracker, bizops-pipeline-sync, bizops-client-onboarding, bizops-follow-up, bizops-people

Activation fields: Core Identity, Sales and Pipeline, Client and Operations, People

**Operations Agent**
Skills: bizops-daily-brief, bizops-project-tracker, bizops-hiring-screener, bizops-sop-builder, bizops-weekly-review, bizops-travel-plan

Activation fields: Core Identity, Projects, Team and Hiring, SOPs, Weekly Review, Daily Brief, Travel

**Finance and Admin Agent**
Skills: bizops-financial-snapshot, bizops-kpi-dashboard, bizops-budget-builder, bizops-vendor-tracker, bizops-invoice-generator

Activation fields: Core Identity, Financial, Vendors, KPIs, Client and Operations (payment terms / invoice format)

---

### Option D: Full Team (4 Agents — for Scaling Businesses)

For business owners who want complete separation of function. Best suited to operations with distinct department owners (e.g., a sales lead, an ops manager, a bookkeeper) — each agent maps to a real person's scope. If the owner is doing all of this themselves, Option C is usually a better fit.

**Sales Agent** — bizops-lead-tracker, bizops-pipeline-sync, bizops-people

**Client Success Agent** — bizops-client-onboarding, bizops-invoice-generator, bizops-follow-up

**Operations Agent** — bizops-project-tracker, bizops-hiring-screener, bizops-sop-builder, bizops-daily-brief, bizops-weekly-review, bizops-travel-plan

**Finance Agent** — bizops-financial-snapshot, bizops-budget-builder, bizops-vendor-tracker, bizops-kpi-dashboard

---

## Recommended Schedules

For system-04 to reference when suggesting automated task cadences at activation. All schedules are suggestions — the client confirms or adjusts during activation.

| Skill | Recommended Schedule | Notes |
|---|---|---|
| bizops-daily-brief | Daily, morning | Sets up the day — run before starting work |
| bizops-follow-up | Daily, morning | Surface overdue and due-today items; integrate into daily brief |
| bizops-pipeline-sync | Weekly, Monday morning | Start-of-week pipeline review |
| bizops-kpi-dashboard | Weekly or monthly | Weekly for fast-moving businesses; monthly for service businesses |
| bizops-weekly-review | Weekly, Friday afternoon | End-of-week accountability check and next-week planning |
| bizops-project-tracker | Weekly | Alongside weekly review — project health check |
| bizops-financial-snapshot | Monthly, first week | Financial pulse at the start or end of each month |
| bizops-budget-builder | Monthly | Update actuals and run variance report at month end |
| bizops-vendor-tracker | Monthly | Renewal alert check and spend audit |
| bizops-client-onboarding | On-demand | Triggered when a new client is signed |
| bizops-invoice-generator | On-demand | Triggered when billing a client |
| bizops-lead-tracker | On-demand | Triggered when adding or updating a lead |
| bizops-hiring-screener | On-demand | Triggered when opening a new role |
| bizops-sop-builder | On-demand | Triggered when documenting a process |
| bizops-people | On-demand | Triggered when adding or looking up a contact |
| bizops-travel-plan | On-demand | Triggered when planning a trip |

---

## How system-03 Should Use This Document

When a client's brain context indicates they are a business owner or operator (from system-02 business blueprint), system-03 should:

1. **Reference the Skills Catalog** to understand what capabilities are available — do not assume; check here.
2. **Surface capabilities based on what the client says they need** — not based on a fixed structure. If a client says "I need help staying on top of my finances," point them to bizops-financial-snapshot, bizops-budget-builder, and bizops-vendor-tracker specifically.
3. **Present the skill catalog as a menu of capabilities** the client can assign freely to whatever agents they create — they are not locked into any configuration.
4. **Use the Suggested Configurations as conversation starters**, not defaults. Show Option A, B, C, or D and ask: "Does one of these match how you think about your business, or would you like to customize?"
5. **Check Natural Pairings** in each skill entry when designing multi-agent setups — these are the highest-value skill combinations.
6. **Note the Phase 0/1 relationship** between lead-tracker and pipeline-sync — these are typically assigned to the same agent and explained together as a two-phase pipeline system.
7. **Prioritize daily-brief, follow-up, and weekly-review** for any agent configuration — these three form the operational backbone that keeps the owner connected to their business daily and weekly.

---

## How system-04 Should Use This Document

When activating an agent that has been assigned `business-operations` skills:

1. **Identify which skills are assigned to this agent** from the system-03 design output.
2. **Look up each assigned skill in the Skills Catalog** to get its "Preferences collected at activation" list.
3. **Take the union of all preference fields** across all assigned skills — this is the complete set to collect. Do not run a separate interview per skill.
4. **Cross-reference the Preferences Registry** to understand which fields are shared across many skills (Core Identity, Sales and Pipeline) versus skill-specific (vendor alert window, travel preferences). Collect shared fields once.
5. **Run one consolidated setup interview** — not one per skill. Group questions naturally: "Let's start with your business basics, then your sales process, then a few financial and operational preferences."
6. **Write all preferences to a shared preferences file in Cloud Brain:**
   Each skill reads from its own path (e.g., `brain/preferences/bizops-financial-preferences.md`), but system-04 should write all relevant preferences in one interview session, routing fields to the correct note by skill domain.
7. **Reference the Recommended Schedules table** to suggest automated task cadences. Present the table to the client and ask: "Which of these would you like to run automatically, and which do you prefer to trigger on-demand?"
8. **Check if preferences already exist** before running the interview:
   ```
   mcp__cloud-brain__search_notes: "bizops preferences"
   ```
   If found, confirm with the client and skip that section — preferences carry across all agents in this plugin.
9. **After activation, confirm** which skills are live and what their schedules are. Give the client a one-line trigger phrase for each skill so they know how to use them.
10. **Highlight the Phase 0/1 pipeline system** at activation: "Lead Tracker captures your deals; Pipeline Sync analyzes them. Use Lead Tracker when you're working a deal; use Pipeline Sync when you want to see the big picture."
