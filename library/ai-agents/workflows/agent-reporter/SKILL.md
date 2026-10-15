---
name: agent-reporter
description: >-
  Set up cadence-based reporting for each active AI agent — defines what each agent reports, how often, and where the output goes (dashboard, email, SMS, or in-chat). Use this skill whenever a client wants to 'set up reporting for my agents', 'get a daily summary from my agents', 'have my agents report to me', 'set up my agent morning brief', 'schedule agent reports', 'get weekly updates from my AI team', 'publish agent output to my dashboard', 'set up my bookkeeper to report monthly', 'have my sales agent send me a daily pipeline update', or 'agent reporter'. Requires at least one active agent. Each agent can have its own independent reporting cadence — daily, weekly, monthly, hourly, or custom. Saves cadence configuration to Cloud Brain and creates scheduled reporting tasks.
---

# Agent Reporter

Your job is to configure automated reporting for each active AI agent — so the client receives the right information, from the right agent, at the right time, delivered where they want it.

Think of this as setting up each agent's "end of shift report." The goal is zero-surprise visibility: the client always knows what their AI team accomplished without having to ask.

---

## Phase 1 — Load Active Agents

Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `Agent Team`. Load the full roster.

**If multiple companies:** Ask which company's agents to configure first.

Display only agents with `status: active`. If none are active, tell the client:
*"You don't have any active agents yet. Run **Agent Activator** to bring your first agent online, then come back here to set up their reporting."*

Show the active roster:
```
Active Agents — [Company Name]

🟢 Alex | Marketing Agent  (reporting: not set)
🟢 Morgan | Finance Agent  (reporting: monthly — configured)
🟢 Jordan | Executive Assistant  (reporting: not set)
```

Ask: *"Let's set up reporting for your agents. Want to configure all of them now, or start with one in particular?"*

---

## Phase 2 — Per-Agent Cadence Configuration

Work through each agent needing a cadence. For each one, ask two things:

**1. How often should this agent report?**

Offer smart defaults based on role — frame as suggestions, not requirements:

| Agent Type | Suggested Cadence | Reason |
|-----------|------------------|--------|
| Executive Assistant | Daily morning | Start each day knowing what's on your plate |
| Sales / Pipeline Agent | Daily morning | Pipeline changes fast — daily keeps you ahead |
| Marketing / Content Agent | Weekly (Monday) | Content is planned weekly; weekly recap makes sense |
| Finance / Bookkeeper Agent | Monthly (1st of month) | Financial data is most meaningful at month-end |
| Research Agent | On-demand only | Reports when research is requested, not on a schedule |
| Transaction Coordinator | Daily (active deals) | Deadline-sensitive — daily is safer |
| Operations Agent | Weekly | Operations tend to be stable week-to-week |

Present as: *"For a [role] like [Agent], most people find [cadence] works best because [reason]. Does that work, or would you prefer a different schedule?"*

**2. What should the report include?**

Suggest a default structure based on the agent's role and their active tasks. Let the client edit:

*Example for Marketing Agent:*
```
Alex's Weekly Marketing Report will include:
  • Content published this week
  • Content scheduled for next week
  • Any active or completed campaigns
  • Recommended focus for next week

Sound right? Anything to add or remove?
```

---

## Phase 3 — Delivery Configuration

For each agent, ask where the report should go:

```
Where should [Agent Name]'s report be delivered?

  A) 📊 Cloud Dashboard (the provider) — view anytime
  B) 📧 Email — sent to your inbox
  C) 💬 iMessage — quick summary to your phone
  D) 🖥️ In Cowork — appears in chat when it runs
  E) Multiple — pick more than one

(Most clients choose Dashboard + iMessage for a quick ping when the report is ready)
```

Allow multiple delivery methods. Note each selection.

- **Dashboard:** Uses `publish-to-dashboard` skill when the scheduled task runs
- **Email:** Uses email MCP to send a formatted report
- **iMessage:** Short summary only (not the full report)
- **In Cowork:** Output appears in the chat session when the scheduled task fires

Match delivery to content: a long financial report belongs on the dashboard, not in an iMessage. Suggest the right fit if the client picks something that won't work well for the content length.

---

## Phase 4 — Schedule Creation

For each agent with a defined cadence, create the reporting scheduled task using `mcp__scheduled-tasks__create_scheduled_task`.

Task naming convention: `[Agent Name] — [Cadence] Report` (e.g., "Alex — Weekly Marketing Report")

Task description should include:
- Which skill(s) to run to generate the report content
- What the report format should be (from Phase 2)
- Where to deliver it (from Phase 3)
- The agent's name and role for context

Confirm each task was created before moving to the next agent.

---

## Phase 5 — Update Cloud Brain

After configuring each agent's reporting, update their entry using `mcp__cloud-brain__edit_note` on the `Agent Team: {Company Name}` note:

- Update `**reporting_cadence:**` with the configured schedule (e.g., "weekly-monday-7am", "daily-8am", "monthly-1st")
- Update `**last_updated:**` on the parent note

---

## Phase 6 — Reporting Summary

Once all agents are configured, show a clean summary:

```
📬 Reporting Setup Complete — [Company Name]

Agent Reports:

🟢 [Agent Name] | [Job Title]
   📅 [Schedule]
   [Delivery icons and destinations]

🟢 [Agent Name] | [Job Title]
   📅 [Schedule]
   [Delivery icons and destinations]
```

Then offer the natural next step:

*"Your agents will now report to you automatically on their schedules. Run **Agent Viewer** anytime to see your full org chart and team status. Or run **Agent Activator** to bring more agents online."*

---

## Principles

- **Smart defaults, client decides.** Always suggest the right cadence for each role — don't make the client figure it out. But always let them override.
- **Don't over-report.** More frequent isn't always better. A monthly financial report is more useful than a daily one with no new data.
- **Report content matters as much as timing.** A report that includes the right information is worth more than one that runs on a perfect schedule but says nothing useful.
- **Keep Cloud Brain in sync.** The reporting cadence in Cloud Brain is what Agent Viewer displays — always update it after configuring.
- **Delivery fit matters.** Match the channel to the content length and urgency. Long reports → dashboard. Quick status pings → iMessage.
