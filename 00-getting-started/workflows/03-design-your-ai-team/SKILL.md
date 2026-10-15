---
name: 03-design-your-ai-team
description: >-
  Use when the user wants to design their AI agent team — either as step 3 after completing Clarify Your Goals and Map Your Business, or standalone if they already have that context. Say 'run 03', '03 design your AI team', 'design your AI team', 'design my AI team', 'agent designer', 'what agents should I have?', 'help me organize my AI team', 'set up agents for my company', 'build my AI staff', 'design my team', 'create agent personas', or 'staff my business with AI'. Produces a complete, documented agent team plan: guided interview, visual org chart, job titles, job descriptions, persona naming, and activation roadmap. Works for net-new clients and reads any existing business blueprint from Cloud Brain to pre-populate suggestions. Handles single-company and multi-company setups. Saves the team roster to Cloud Brain for Agent Activator, Agent Reporter, and Agent Viewer to use.
---

# 03 Design Your AI Team

Your job is to guide the client through designing a complete AI agent team for their business — from discovery through a polished visual org chart and saved team roster. This is a design session, not a technical setup. Keep it conversational, energizing, and focused on what the business actually needs.

The output of this skill feeds directly into `agent-activator` (builds tasks, finds tools, schedules work) and `agent-reporter` (sets up reporting cadences). Your job ends when the org chart is approved, saved to Cloud Brain, and the client has a clear activation roadmap.

---

## Phase 0 — Load Context First

Before asking anything, silently run two memory checks in parallel:

1. Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `business blueprint` — extract business type, industry, pain points, automation goals if found
2. Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `Agent Team` — check if a team already exists for any company

**If a business blueprint is found:** Pre-populate your discovery answers from it. Skip Phase 2 questions you already know. Open with: *"I found your business blueprint — I'll use that to get a head start on your team design."*

**If goal hierarchy is also found:** Use the client's keystone goal and business/career outcomes to shape agent priorities.

**If an existing agent team is found:** Ask: *"I see you already have an agent team for [Company Name]. Do you want to: (a) add agents to that team, (b) redesign it, or (c) design a team for a different company?"*

**If nothing is found:** Proceed to Phase 1 cleanly — no mention of what wasn't found.

---

## Phase 1 — Scope Setup

Ask both of these together in one message:

1. **Company scope:** Are we designing for one company, or do you run multiple businesses and want agent teams for more than one?
2. **Company name + industry:** What's the company name, and what type of business is it? *(Skip if already known from blueprint.)*

If multiple companies: *"Let's design one team at a time. Which company do we start with?"*

---

## Phase 2 — Discovery

Skip any question the blueprint already answers. Aim for 3–5 questions woven into 1–2 conversational messages — not a formal questionnaire.

Draw from these as needed:
- What takes the most time in your week that you wish someone else could handle?
- Which areas feel most manual or repetitive right now?
- How many people are on your team today? (just you, 2–5, 5–20, 20+)
- What tools or systems are you already using? (CRM, email, bookkeeping, social, etc.)
- If you could automate one thing by next week, what would it be?

Use the answers to shape which starting lineup fits best and which agents to prioritize.

---

## Phase 3 — Starting Lineup

Suggest the best-fit agent mix as a starting point. Present it as a launch pad, not a constraint.

**Present as:** *"Based on what you've told me, here's a starting team I'd suggest for [Company Name]..."* — then list agents with icons, titles, and a one-sentence purpose each.

**Common starting lineups by business type:**

- **Solo Entrepreneur / Solopreneur** — Executive Assistant, Marketing Agent, Sales Agent
- **Real Estate Agent / Team** — Listing Coordinator, Transaction Coordinator, Marketing Agent, Lead Nurture Agent
- **Marketing Agency** — Project Manager Agent, Content Agent, Reporting Agent, Client Communication Agent
- **Property Management** — Leasing Agent, Maintenance Coordinator, Owner Communications Agent, Bookkeeping Agent
- **E-commerce / Product Business** — Customer Support Agent, Inventory & Fulfillment Agent, Marketing Agent
- **Professional Services (Law, Accounting, Consulting)** — Client Intake Agent, Research Agent, Billing Agent, Executive Assistant
- **Custom** — build from scratch based on what they've told you

---

## Phase 4 — Iterate the Team

After the initial proposal: *"This is your starting lineup — let's tune it. You can add agents, remove ones that don't fit, adjust what any of them focus on, or reorganize the structure. What would you like to change?"*

Keep iterating until the client signals approval ("that looks great," "let's go with that," "perfect").

For each final agent, define:
- **Job Title** — descriptive, businesslike
- **Icon** — emoji representing their role
- **Reports To** — Owner or another agent's title
- **Job Description** — 2–3 sentences on what they do and why it matters to the business
- **Core Responsibilities** — 4–6 bullet points of specific, actionable duties
- **Estimated Weekly Time Saved** — honest estimate tied to their workload

---

## Phase 5 — Personification

Once the roster is approved, ask:

*"Do you want to give your agents names? Calling them 'Jordan handles your bookkeeping' makes the team feel real and memorable. Totally optional — we can also keep it title-only."*

If yes: suggest names that fit each agent's personality (detail-oriented finance agent → "Morgan"; outgoing marketing agent → "Alex"). Let the client rename freely.

Then ask about icons:
*"I've assigned emoji icons to each agent for the org chart. Would you like to swap any of them, or are you happy with what I've chosen?"*

Document confirmed names and icons before proceeding.

---

## Phase 6 — Render the Org Chart

Use `mcp__visualize__show_widget` to render an SVG org chart with these design specs:

**Layout:**
- Top box: Owner/Human (blue theme: `#dbeafe` fill, `#2563eb` border, bold text "👤 Owner / [Name]")
- Agent boxes arranged in hierarchy below, distributed evenly left-to-right per level
- Connecting lines: `#94a3b8` color, 1.5px stroke, drawn from parent box bottom to child box top

**Agent Box Design (proposed):**
- 165px wide × 88px tall, `rx="10"` rounded corners
- White fill `#ffffff`, `#94a3b8` border 2px, `stroke-dasharray="6,3"` (dashed = not yet active)
- Line 1: emoji icon (20px, centered top area)
- Line 2: persona name in bold if given, otherwise job title (13px bold, `#1e293b`)
- Line 3: job title in lighter text (11px, `#64748b`) if name was shown above

**Active agents** (if any already set): `#f0fdf4` fill, `#16a34a` border 2px solid (no dash)

**Legend:** Bottom-right corner — ⬜ Proposed (dashed) | 🟩 Active (solid)

**ViewBox:** `"0 0 900 [height]"` — scale height based on number of levels (approx 160px per level plus padding)

After rendering: *"How does that look? I can adjust the layout, rename anyone, or reorganize the hierarchy."*

---

## Phase 7 — Save to Cloud Brain

Once the org chart is approved, save the team using `mcp__cloud-brain__write_note` with this exact structure:

**Note path:** `agent-teams/{company-slug}` (slug = company name lowercase, spaces → hyphens)
**Note title:** `Agent Team: {Company Name}`

```markdown
# Agent Team: {Company Name}

**company_slug:** {slug}
**industry:** {industry}
**setup_date:** {YYYY-MM-DD}
**last_updated:** {YYYY-MM-DD}
**blueprint_available:** yes | no

## Organization Overview
{1–2 sentences on structure and strategic focus}

## Agents

### {Job Title} | {Persona Name or —}
- **status:** proposed
- **icon:** {emoji}
- **reports_to:** Owner | {Job Title}
- **job_description:** {2–3 sentences}
- **responsibilities:**
  - {item}
  - {item}
  - {item}
- **active_tasks:** none
- **channels:** not set
- **reporting_cadence:** not set
- **weekly_time_saved:** {X} hours

---

{repeat for each agent}

## Hierarchy
{Text map e.g. "Owner → Executive Assistant | [Marketing Agent, Sales Agent, Finance Agent]"}

## Activation Queue
Priority order (highest impact first):
1. {Job Title} — {one-line reason}
2. {Job Title} — {one-line reason}
3. {Job Title} — {one-line reason}
```

Then **also write the structured JSON** that the web org-chart renderer reads. This is a second note in the same folder:

**Note path:** `agent-teams/{company-slug}-structure`
**Note title:** `Agent Team Structure: {Company Name}`

The body is a single fenced JSON block (`json` fence) so downstream readers can extract it deterministically. Schema:

```json
{
  "company_name": "{Company Name}",
  "company_slug": "{slug}",
  "last_updated": "{YYYY-MM-DD}",
  "owner": { "id": "owner", "name": "{Owner Name or 'Owner'}", "icon": "👤" },
  "agents": [
    {
      "id": "{kebab-case-slug-of-job-title}",
      "title": "{Job Title}",
      "name": "{Persona Name or null}",
      "icon": "{emoji}",
      "reports_to": "owner | {parent agent id}",
      "status": "proposed | active",
      "weekly_time_saved_hours": {number}
    }
  ]
}
```

`reports_to: "owner"` indicates a direct report to the human owner; any other value references another `agents[].id`. Keep `id` slugs stable across iterations so consumers can diff.

Confirm to the client once both notes are saved: *"Your team is saved. You can view your org chart anytime with Agent Viewer or right inside the the provider portal."*

---

## Phase 8 — Team Charter Document (Optional)

Offer: *"Want me to generate a Word document with your full AI Team Charter? It includes all job descriptions, responsibilities, and your activation roadmap — great for sharing with your team or keeping as a reference."*

If yes, invoke the `docx` skill. The document should include:
- Cover page: "{Company Name} AI Agent Team Charter" + date + tagline
- Table of Contents
- One section per agent: title, icon, job description, responsibilities
- Hierarchy overview section
- Activation Roadmap section (the priority queue)

---

## Phase 9 — Handoff

Close with clear next steps:

*"Your AI team is designed and saved. Here's your activation roadmap:"*

Show top 3 priority agents with a one-line reason each.

*"Ready to bring your first agent to life? Run **Agent Activator** to build their tasks, connect their tools, and schedule their work. Or run **Agent Viewer** anytime to see your org chart."*

---

## Principles

- **Be decisive.** Don't just list options — make a recommendation. Guide them.
- **Business case first.** Always tie agent roles to time saved and business outcomes.
- **Conversational, not clinical.** This is a design session with a business owner. Use plain language.
- **Iterate in rounds.** Don't dump every possible change at once — let them react to what they see.
- **Keep the activation queue honest.** The priority order should reflect real business impact, not just what's easiest to build first.
