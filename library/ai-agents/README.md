# AI Agents

**Version 1.1.0** | by the provider

Build, activate, and manage a complete AI agent team for your business — from org chart design through live autonomous agents running on schedule.

---

## What's in This Plugin

| Skill | What It Does |
|---|---|
| **Agent Designer** | Design your AI team org chart through a guided interview. Define agent roles, responsibilities, and personas. Saves your team roster to Cloud Brain. |
| **Agent Activator** | Bring one agent to life at a time. Maps responsibilities to concrete tasks, recommends the right tools (Genie plugins, Composio, or native connectors), sets up channels, and schedules automated work. |
| **Agent Viewer** | View your org chart and team status at any time. See which agents are active, what they're running, and what's next. |
| **Agent Reporter** | Configure automated reporting for each active agent — what they report, how often, and where it goes (dashboard, email, iMessage, or in chat). |
| **Skill Builder** | Build custom skills for tasks no existing plugin covers. Guides you through intent capture, drafting, testing, and packaging. |
| **Cloud Migration Audit** | Health-check every automation you have. Shows which scheduled tasks actually run in the cloud vs. only on your machine, scores each one, and migrates the ones worth moving — with your approval, one at a time. |

---

## How to Use It

### Step 1 — Design your team
Run **Agent Designer**. It takes about 15 minutes and walks you through:
- Discovering which agents your business needs
- Defining their roles and responsibilities
- Giving them names and personalities (optional but fun)
- Rendering a visual org chart
- Saving the team to your Cloud Brain

### Step 2 — Activate one agent at a time
Run **Agent Activator** and choose an agent to bring online. For each agent, it will:
- Review their responsibilities with you
- Set up how you interact with them (scheduled, SMS, email, Slack)
- Map each responsibility to a concrete task with the right tool behind it
- Recommend Genie plugins that cover each task
- Guide you through Composio setup for external service connections
- Hand off to Skill Builder if a custom skill needs to be built
- Schedule automated tasks and document on-demand trigger phrases
- Update your Cloud Brain and flip the agent to ACTIVE

### Step 3 — Set up reporting
Run **Agent Reporter** to configure each active agent's reporting cadence — daily, weekly, monthly, or on-demand. Deliver reports to your dashboard, email, iMessage, or in Cowork.

### Step 4 — Check in anytime
Run **Agent Viewer** to see your org chart, team status, and what's running.

### Step 5 — Verify it's actually running
Run **Cloud Migration Audit** to confirm your agents' scheduled tasks fire in the cloud — not just inside a session that ends when you close your laptop. Good to run right after activating an agent, and periodically after that.

---

## Tool Selection: How Agent Activator Chooses Tools

Agent Activator follows a specific decision order to find the best tool for each agent task:

1. **Genie Plugin first** — checks the plugin catalog for a skill that already covers the task. If found, recommends the install.
2. **Native Claude connector** — appropriate only for single-account, single-instance service connections where multi-account is not needed.
3. **Composio (recommended for most external services)** — Claude's native connectors only support one account per service. If you have (or might ever have) multiple Gmail accounts, multiple client calendars, or multiple CRM instances, Composio is the right choice. It handles multi-account connections reliably.
4. **Custom skill via Skill Builder** — for tasks that require custom logic, formatting, or automation not covered by existing tools.

> **Note on Claude in Chrome:** The browser plugin is not recommended for agent tasks. It requires a human at the keyboard and cannot run on a schedule. The goal of this plugin is agents that work autonomously — without you needing to be present.

---

## Two-Phase System

**Agent Designer → Agent Activator** is a two-phase workflow. Agent Designer creates the team roster in Cloud Brain. Agent Activator reads that roster and uses it to build tasks. You don't need to re-enter your agent roster each time — it's already saved.

---

## Updating Your Team

- **Add a new agent:** Run Agent Designer and choose "add agents to my existing team."
- **Change an agent's responsibilities:** Run Agent Activator for that agent — you can edit the profile before building tasks.
- **Add an agent not on the org chart:** Agent Activator handles this — just name the agent you want to activate, and it will walk through a quick design before activation.
- **Update reporting:** Run Agent Reporter anytime to change cadences or delivery channels.

---

## What's New in v1.1.0

- **New skill: Cloud Migration Audit** — audits cloud Routines, local session cron jobs, and local-dependent agents; scores each on reliability, compute, remote access, cost, and privacy; migrates approved items to cloud Routines one at a time with explicit per-item approval
- Catches the most common silent failure: a "scheduled" task that was really a session-bound local cron job and died with the session

## What's New in v1.0.0

- Initial release
- Full five-skill suite: Designer, Activator, Viewer, Reporter, Skill Builder
- Agent Activator includes guided tool discovery (Genie plugin catalog, Composio, native connectors)
- All skills use Cloud Brain (`mcp__cloud-brain__*`) for multi-tenant isolated memory
- Composio-first tool recommendation for external service connections


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
