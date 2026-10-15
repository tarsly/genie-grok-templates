# AI Agents Plugin — AGENTS.md

*Read by Agent Designer and Agent Activator to understand what this plugin provides.*

---

## About This Plugin

The AI Agents plugin is the system for building, activating, and managing an autonomous AI team. It covers the full lifecycle: design the org chart → activate agents with real tools → configure automated reporting → build custom skills when needed.

This plugin is the foundation of the Genie AI OS. Every other plugin plugs into it — Agent Activator reads the Genie plugin catalog to recommend the right skills for each agent task.

---

## Skills Catalog

| Skill | Trigger Phrases | What It Does |
|---|---|---|
| `agent-designer` | "design my agent team", "what agents should I have", "build my AI staff", "agent designer" | Guided interview → visual org chart → saved team roster in Cloud Brain |
| `agent-activator` | "activate an agent", "set up tasks for my agent", "turn on my agents", "agent activator" | Maps responsibilities to tasks, finds tools, sets schedules, activates one agent at a time |
| `agent-viewer` | "show my agent team", "view my org chart", "who are my agents", "check my team status" | Read-only org chart view with status, tasks, and navigation |
| `agent-reporter` | "set up reporting for my agents", "get a daily summary from my agents", "agent reporter" | Configures reporting cadences and delivery channels for each active agent |
| `skill-builder` | "build a custom skill", "create a skill for my agent", "make a skill", "skill builder" | Guides skill creation, testing, iteration, and packaging |
| `agent-cloud-audit` | "audit my automations", "should this run in the cloud", "is my agent local or cloud", "why didn't my scheduled task fire", "move my cron jobs to the cloud" | Inspects cloud Routines, local session cron, and local-dependent agents; scores each on reliability, compute, remote access, cost, and privacy; migrates approved items to cloud Routines one at a time |

---

## Preferences Registry

The AI Agents plugin does not maintain a standalone preferences file. Team data (org charts, agent profiles, task lists, channel configs, reporting cadences) is stored per-company in Cloud Brain under the path `agent-teams/{company-slug}`.

The brain note for each company (`Agent Team: {Company Name}`) serves as the living record for all agent state. Agent Designer creates it; Agent Activator, Agent Reporter, and Agent Viewer all read and update it.

---

## Suggested Configurations

### Configuration A — Solo Operator (most common)
A single business owner who wants to delegate repetitive work to AI agents.

Recommended activation order:
1. **Agent Designer** — design the team (Executive Assistant, Marketing Agent, one domain-specific agent)
2. **Agent Activator** — activate the Executive Assistant first (highest impact fastest)
3. **Agent Reporter** — set up daily morning brief
4. Repeat activation for remaining agents

### Configuration B — Multi-Company / Investor
An operator with multiple businesses or investment portfolios who wants separate agent teams per entity.

Same flow as above, run once per company. Agent Viewer supports a multi-company "show all" view.

---

## Schedules Table

| Task Pattern | Skill | Suggested Schedule |
|---|---|---|
| Executive Assistant daily brief | Any skill the EA runs | Daily, 7:00 AM |
| Marketing content batch | content-social-calendar or custom | Weekly, Monday 7:00 AM |
| Financial report | Any finance skill | Monthly, 1st of month, 8:00 AM |
| Sales pipeline update | bizops-lead-tracker or GHL | Daily, 8:00 AM |
| Transaction coordinator check | realtor-toolkit:transaction-coordinator | Daily, 7:30 AM (active deals only) |

Schedules are created during Agent Activator (Phase 6) and Agent Reporter (Phase 4) using `mcp__scheduled-tasks__create_scheduled_task`.

---

## Tool Selection Framework

Agent Activator uses this decision order when equipping each task:

1. **Genie Plugin** — check `<your-provider-settings-url>` first
2. **Native Claude connector** — only if single-instance, connector exists, and multi-account is not needed
3. **Composio** — default for any external service where multi-account is possible or connector is missing
4. **Custom skill via Skill Builder** — for logic/automation tasks with no existing tool

Claude in Chrome is never recommended for agent tasks (not autonomous, requires human presence).
