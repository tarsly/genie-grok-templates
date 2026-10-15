---
name: agent-activator
description: >-
  Activate AI agents by building their task list, finding the right tools, and scheduling automated work. Reads the designed agent team from Cloud Brain and brings each agent to life one at a time. Use this skill whenever a client wants to 'activate an agent', 'set up tasks for my agent', 'schedule my AI agents', 'bring my agent team to life', 'build tasks for [agent name]', 'what should my [agent] do?', 'turn on my agents', 'start my AI team', or 'agent activator'. Also use after completing Agent Designer when the client is ready to make agents operational. Maps each agent's responsibilities to real tasks, recommends Genie plugins and tools for each task, guides tool setup (native connectors vs. Composio), walks through custom skill creation when needed, creates scheduled tasks, and updates agent status to active. Run this skill one agent at a time.
---

# Agent Activator

Your job is to take a designed (but not yet active) agent from the team roster and fully activate them — mapping their responsibilities to concrete tasks, equipping them with the right tools, setting schedules, and flipping their status to active in Cloud Brain.

This skill is focused and practical. **One agent per session.** Get in, build the tasks, confirm the tools are in place, confirm tasks are running, and get out. The energy should feel like "your Marketing Agent is now online."

---

## Phase 1 — Load the Team

Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `Agent Team` to find existing agent teams.

**If multiple companies exist:** Ask: *"Which company's agent team are we working with?"*

**If one company exists:** Load it directly. No need to ask.

**If no team exists:** Tell the client: *"It looks like you haven't designed your agent team yet. Run **Agent Designer** first to set up your roster — it takes about 15 minutes. Then come back here to activate."*

---

## Phase 2 — Show the Roster

Display the full agent roster with clear status indicators:

```
[Company Name] Agent Team — Activation Status

⬜ PROPOSED (not yet active)
  🗂️ Executive Assistant | Jordan
  📱 Marketing Agent | Alex
  💰 Sales Agent

🟢 ACTIVE
  💵 Finance Agent | Morgan  ← activated [date]
```

Then ask: *"Which agent would you like to activate today?"*

If this is their first activation, recommend the top item from the Activation Queue: *"Based on your setup, I'd suggest starting with [Agent] — they'll save you the most time the fastest."*

**Adding an agent not on the org chart:**
If the client names an agent that doesn't exist in their team roster, say: *"[Agent Name] isn't on your org chart yet. No problem — let's define them first."* Run a quick mini-design (job title, icon, 3–5 responsibilities) and ask: *"Want me to add them to your saved team in Cloud Brain?"* If yes, update the team note with their entry before proceeding.

---

## Phase 3 — Agent Profile Review

Load the selected agent's full profile from the Cloud Brain note. Display it clearly:

```
Activating: 📱 Marketing Agent | Alex
Job: Keeps the brand visible and content flowing without you touching a keyboard every day.

Core Responsibilities:
  • Weekly social media content batches
  • Content calendar planning
  • Repurposing existing content across platforms
  • Email campaign support

Estimated Time Saved: 4–6 hours/week
```

Ask: *"Does this still reflect what you want [Agent Name] to do? Or would you like to adjust anything before we build the tasks?"*

Allow edits before proceeding.

---

## Phase 4 — Channel Setup

Before mapping tasks, establish how the client will interact with this agent and how the agent reaches them.

Present the channel options clearly, with cost and requirements for each:

```
How do you want to interact with [Agent Name]?

📅 Scheduled only
   Agent runs automatically — no direct interaction needed
   Cost: free

📱 SMS (text messaging)
   Text [Agent] a command, get a response back in seconds
   Cost: ~$2–$12/month + one-time ~$20 setup fee (first agent only)
   Requires: Twilio account + agent SMS server

📧 Email
   [Agent] has their own email address; you email them, they reply
   Cost: free (uses your existing email account)
   Requires: Gmail MCP connected

💬 Slack
   [Agent] lives in a Slack channel — great for team-facing agents
   Cost: free (uses your existing Slack workspace)
   Requires: Slack MCP connected

You can combine channels — for example, Scheduled + SMS for an agent that
runs weekly reports AND responds to your texts throughout the week.
```

Ask: *"Which channel(s) would you like for [Agent Name]?"*

### If SMS is selected:

Show cost breakdown before proceeding:

```
💰 SMS Channel — Cost Summary

One-time setup costs (first agent only):
  • Twilio A2P Brand registration:     ~$4.50
  • Twilio A2P Campaign vetting fee:   ~$15.00
  ─────────────────────────────────────────────
  One-time total:                      ~$19.50

Monthly costs:
  • Twilio phone number:               ~$1.15/month  (per agent)
  • A2P Campaign fee:                  ~$1.50–$10/month (shared across all agents)
  • Render server (shared):            $7/month (covers unlimited agents)
  • Anthropic API (message usage):     ~$0.01–$0.05/month at typical SMS volume
  ─────────────────────────────────────────────
  Estimated monthly (first agent):     ~$10–$20/month total
  Each additional agent:               ~$1.15/month (just the phone number)
```

Ask: *"Does that work for your budget? Ready to proceed with SMS?"*

If yes, ask: *"Do you already have the agent SMS server set up?"*

- **If YES:** Generate a ready-to-paste `agents_config.py` entry for this agent, then walk them through adding it and setting the Twilio webhook.
- **If NO:** Explain it's a one-time ~60-90 minute setup, point them to the SMS Channel Setup Guide, and offer to walk through it together before returning to activation.

### If Email is selected:

Ask: *"What email address should [Agent Name] have?"* Confirm the Gmail MCP is connected. Help set up a filter/label so incoming emails are easy to find.

### If Slack is selected:

Ask: *"Which Slack workspace and channel should [Agent Name] operate in?"* Confirm the Slack MCP is connected.

### Document channels in Cloud Brain

After channel setup, note the agent's channel(s) in their brain entry:
- `**channels:** SMS (+1XXXXXXXXXX)`
- `**channels:** Email (agent@yourbusiness.com)`
- `**channels:** Slack (#channel-name)`
- `**channels:** Scheduled only`

---

## Phase 5 — Task Mapping & Tool Discovery

This is the most important phase. For each responsibility, define a concrete task — and make sure the agent has the right tool to actually do it.

Work through responsibilities one at a time (or in small batches of 2–3). For each task, answer three questions, then run the tool discovery check below.

**For each task, determine:**

1. **What triggers this task?**
   - **Scheduled** — runs automatically on a time interval
   - **On-demand** — client asks for it directly
   - **Event-based** — triggered by something external (form submission, new lead, etc.)

2. **What does the task do?**
   - What's the action or output?
   - What inputs does it need?

3. **Who sees the result?**
   - Client only, in Cowork
   - Published to the cloud dashboard
   - Sent via email or iMessage

---

### Tool Discovery Check (run for every task)

After defining what a task does, determine what tool the agent needs to accomplish it. Work through this decision tree:

**Step 1 — Does an Genie plugin cover this?**

Fetch the Genie plugin catalog: `GET <your-provider-settings-url>`

Scan the catalog for plugins whose skills match this task. If a match is found:

- Tell the client: *"For [task], the **[Plugin Name]** plugin has a skill that handles this — it's called [skill name]. [One sentence on what it does.] Is that plugin already installed?"*
- If not installed: *"You can install it from the Genie Library: [marketplace_url]. Once it's in, [Agent Name] can run the `[skill-name]` skill automatically for this task."*
- If installed: *"Great — [Agent Name] will use the `[skill-name]` skill for this task. No additional setup needed."*

Proceed to the next task if a plugin covers it.

**Step 2 — Does a native Claude connector work?**

If no Genie plugin covers the task, check whether a native Claude MCP connector is sufficient:

- A native connector is appropriate when: the client uses only **one account** for that service (one Gmail, one calendar, one Slack workspace), the connector exists in Claude's marketplace, and multi-instance is not needed now or in the foreseeable future.
- A native connector is **not** appropriate when: the client has or might need multiple accounts for the same service (two Gmail accounts, multiple client calendars, several CRMs), or the service doesn't have a native Claude connector.

If a native connector works, confirm it's connected: *"For [task], the [Service] connector in Claude can handle this. Is your [Service] account already connected? If not, you can connect it in Settings → Connectors."*

**Step 3 — Recommend Composio**

If the task requires connecting to an external service AND any of these are true:
- The client has (or is likely to need) multiple accounts for that service
- No native Claude connector exists for the service
- The task involves multi-step workflows across more than one external service

→ **Recommend Composio.**

Say: *"For [task], I'd recommend using Composio rather than the native Claude connector. Composio is better here because [reason — multiple accounts / no native connector / multi-service workflow]. Composio lets the agent connect to [service] reliably and supports multiple accounts if you ever need them."*

Walk them through it:
1. *"Go to composio.dev and create a free account if you don't have one."*
2. *"In Composio, connect your [service] account — it takes about 2 minutes."*
3. *"In Claude's MCP settings, add the Composio MCP server. Once connected, the agent can reach [service] through Composio."*

If the client is unsure about Composio, explain simply: *"Composio is a connector hub that sits between Claude and your external apps. The main advantage over native connectors is that it handles multiple accounts — if you have two Gmail addresses or want to manage several clients' calendars, Composio supports all of that. Native Claude connectors only allow one account per service."*

**Step 4 — Does a custom skill need to be built?**

If the task involves **logic, formatting, analysis, or automation** that doesn't require an external connector (or the connector piece is handled), check whether a skill needs to be created:

- If the Genie library has a skill that fits: point to it
- If no existing skill covers it: *"This task would need a custom skill. Want to build one now? Run **Skill Builder** to walk through it — I can hand off to that skill right now if you'd like."* If the client says yes, invoke the `skill-builder` skill.

**Step 5 — Flag if nothing fits**

If after all the above, no clear tool path exists, be honest: *"I'm not finding a clear tool for [task] right now. You have a few options: (1) skip this task for now and come back to it, (2) build a custom skill for it using Skill Builder, or (3) keep it as a manual task you handle yourself. What works best?"*

---

### Important: Claude in Chrome is not the right tool for agentic work

Do not recommend the Claude in Chrome browser extension as the solution for any agent task. Browser automation is fragile, requires a human at the computer, and cannot run scheduled or truly autonomous tasks. The goal is agents that work **without the client needing to be present**. If a task seems like it would need browser control, look harder for a proper connector or API-based approach via Composio or a native MCP.

---

### Build the task list

As you work through each responsibility, build a running task list:

```
[Agent Name]'s Tasks:

Task 1: [Task Name]
  Trigger: [Schedule / On-demand phrase / Event]
  Tool: [Genie skill / Composio / Native connector / Custom skill]
  Action: [What it does]
  Output: [Dashboard / Email / iMessage / In chat]
  Status: ⏳ ready to schedule

Task 2: [Task Name]
  Trigger: On-demand — "Alex, [trigger phrase]"
  Tool: [tool]
  Action: [What it does]
  Output: [Delivered in chat]
  Status: 📋 manual — no scheduling needed
```

Confirm with the client before scheduling: *"Here are [Agent Name]'s tasks. Ready to turn them on?"*

---

## Phase 6 — Schedule Creation

For each task marked as "scheduled," create it using `mcp__scheduled-tasks__create_scheduled_task`.

When creating each scheduled task:
- Use a clear name: `[Agent Name] — [Task Name]` (e.g., "Alex — Weekly Content Batch")
- Set the correct cron expression or interval
- Include the skill name and any relevant parameters in the task description

For on-demand tasks: document the exact trigger phrase the client should say. Write it clearly so they know what to say.

Confirm each task was created successfully before moving to the next.

---

## Phase 7 — Update Cloud Brain

Once all tasks are created, update the agent's entry in Cloud Brain using `mcp__cloud-brain__edit_note` on the `Agent Team: {Company Name}` note:

- Change `**status:** proposed` → `**status:** active`
- Update `**active_tasks:**` with the names of all scheduled tasks created
- Update `**channels:**` with the channel(s) configured in Phase 4
- Update `**last_updated:**` date on the parent note

---

## Phase 8 — Activation Summary

Show a clean summary of what was built:

```
✅ [Agent Name] ([Job Title]) is now ACTIVE

📱 Channel: SMS — text [number] to reach [Agent] directly
   (or: 📧 Email — agent@yourbusiness.com)
   (or: 📅 Scheduled tasks only)

Scheduled Tasks:
  📅 [Task Name] — [schedule] → [output destination]
  📅 [Task Name] — [schedule] → [output destination]

On-Demand Tasks:
  💬 "[trigger phrase]" → [skill/action]

Tools configured:
  • [Plugin / Composio / native connector] for [task]

Estimated weekly time saved: ~[X] hours
```

Then offer the natural next step:

*"[Agent Name] is online. Want to activate another agent now? Or would you like to set up reporting so you get a summary of what [Agent Name] does each week? Run **Agent Reporter** for that."*

---

## Principles

- **One agent per session.** Don't try to activate multiple agents at once — tasks get missed and tool setup becomes confusing.
- **Tool-first thinking.** Every task needs a real tool behind it. An agent with no tools is just a chatbot. Don't move on from a task until you've identified what will actually power it.
- **Composio is usually the right answer for external connectors.** Most small business clients have multiple accounts (two email addresses, several clients' calendars, multiple social logins). Native Claude connectors only support one instance per service. Default to Composio for any external service connection unless the client has confirmed they will only ever need one account.
- **Claude in Chrome is not agentic.** Never recommend it for agent tasks. It requires a human at the keyboard and can't run on a schedule.
- **Genie plugins first.** Before recommending Composio or a custom skill, always check the plugin catalog. A client shouldn't build what's already been built.
- **Channel first, tasks second.** Always establish how the client interacts with the agent before mapping tasks — it shapes what "on-demand" means.
- **Be transparent about costs.** Before setting up any paid channel or tool, show the cost clearly.
- **Confirm before scheduling.** Show the full task list and get approval before creating scheduled tasks.
- **Celebrate the activation.** This is a meaningful moment — their AI team member is now working for them. Mark it accordingly.
