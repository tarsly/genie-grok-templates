---
name: 02-map-your-business
description: >-
  Use when the user wants to give Claude a clear picture of their business — who they serve, what they do, and how they operate — so every future skill has context to work from. Say 'run 02', '02 map your business', 'map your business', 'map my business', 'biz blueprint', 'automate my business', 'how can AI help my business', 'business automation plan', 'what should I automate first', 'find my automation opportunities', or 'where can I use AI agents'. Runs a guided discovery interview across goals, roles, workflows, and tasks, then produces a Business Automation Blueprint — a concrete plan for the highest-impact automation opportunities in the business. Saves the blueprint to Cloud Brain so downstream skills (including step 03) can pre-populate from it.
---

# 02 Map Your Business

This skill guides you through a comprehensive, multi-level interview to identify specific, actionable ways a client can automate their business using the Claude ecosystem (Claude Code, Claude Cowork, Claude Scheduled Tasks, and MCP integrations). The output is a concrete **Business Automation Blueprint** — a tailored, step-by-step agentic automation plan.

## Core Objective

Your goal is to guide the client from high-level business goals down to specific, manual tasks that can be handed off to an AI agent. You are not just suggesting tools; you are helping the client design an **agentic workflow** where Claude operates autonomously.

---

## Phase 0 — Load Context

Before starting the interview, check Cloud Brain for prior context:

1. `mcp__cloud-brain__search_notes` with query `goal-hierarchy` — if found, read the business/career and financial goals to inform the discovery conversation
2. `mcp__cloud-brain__search_notes` with query `business blueprint` — if a prior blueprint exists, offer to update it rather than start from scratch

If goal hierarchy is found: *"I can see your goal hierarchy — I'll use that as context for where your business is headed."*

---

## The Interview Process

Conduct the interview sequentially, one level at a time. **Do not ask all questions at once.** Wait for the client's response before moving to the next level.

### Interview Levels:

**Level 1: Business Context & Goals** (The "Why")

Objective: Understand the business, its core value proposition, and high-level goals.

Questions to ask (pick the most relevant):
- "To start, could you tell me a bit about your business? What is your core product or service, and who are your main customers?"
- "What are the primary goals for your business right now? (e.g., scaling revenue, reducing operational costs, improving customer satisfaction)"
- "What are the biggest bottlenecks or pain points preventing you from reaching those goals?"

**Level 2: Roles & Responsibilities** (The "Who")

Objective: Map out the key roles required to run the business and identify where the most time is spent.

Questions to ask:
- "What are the key roles or departments in your company? (e.g., Sales, Marketing, Operations, Customer Support, Finance)"
- "For you personally, or for your key employees, what does a typical day look like?"
- "Which roles or specific people are currently overwhelmed with repetitive or manual work?"

**Level 3: Workflow & Process Mapping** (The "How")

Objective: Drill down into specific daily workflows for the roles identified in Level 2.

Questions to ask:
- "Let's look closer at [Specific Role/Department]. Can you walk me through the step-by-step process they follow to complete [Specific Task]?"
- "What software tools or platforms are used in this process? (e.g., Gmail, Slack, Notion, Salesforce, Excel)"
- "Where does data need to be moved manually from one system to another?"
- "What tasks require reading, summarizing, or synthesizing large amounts of information?"

**Level 4: Task-Level Automation Identification** (The "What")

Objective: Pinpoint exact tasks that can be automated using Claude's specific capabilities.

Questions to ask:
- "You mentioned spending a lot of time on [Specific Task]. Does this involve reading emails, analyzing documents, or updating spreadsheets?"
- "Do you ever need to trigger tasks while away from your desk, perhaps from your phone?"
- "If you had an assistant who could perfectly execute one 30-minute computer task for you every day, what would you give them?"

*Note: If the client already provides specific tasks in their initial message, fast-track to Level 3 or 4 — but ensure you understand the broader context first.*

---

## Mapping to Claude Capabilities

Once you have identified specific manual tasks (Level 4), map them to the appropriate Claude products and MCP integrations:

| Product | Best For |
|---|---|
| **Claude Cowork (Desktop)** | File-heavy workflows, local data processing, multi-step tasks (organizing files, synthesizing research, financial reconciliation) |
| **Claude Code (CLI/IDE)** | Developers, repo-wide refactoring, technical workflows |
| **Claude Scheduled Tasks** | Recurring operations, monitoring, reporting — running prompts on a cadence without manual triggers |
| **MCP Integrations** | The "hands" of the agent — connect Claude to external tools (Slack, Notion, Google Drive, GitHub, HubSpot, Stripe, QuickBooks) |

### Common Automation Categories

**Email & Communications:** Claude Cowork + Gmail/Slack MCP → daily inbox triage, urgency categorization, draft replies, send summary to Slack.

**Research & Content Pipeline:** Claude Cowork + Web Search MCP → drop raw notes into a folder, synthesize into a formatted Word document research report.

**Data Analysis & Financial Reconciliation:** Claude Cowork + Stripe/QuickBooks MCP → match transactions, flag missing invoices, generate reconciled Excel file.

**CRM & Sales Operations:** Claude Cowork + HubSpot/GHL MCP → pull call transcript, extract action items, update deal stage, draft follow-up email.

**Project Management:** Claude Scheduled Tasks + Linear/Asana MCP → daily standup summaries, ticket updates, weekly stakeholder reports.

### The Agentic Mindset Shift

Help the client shift from a "chatbot" mindset to an "agentic" mindset:
- **Chatbot:** "I ask a question, it gives an answer."
- **Agentic:** "I define the goal and the constraints, provide access to my tools (via MCP) and files, and Claude executes the multi-step process autonomously."

---

## Delivering the Blueprint

After completing the interview and mapping the tasks, generate a comprehensive **Business Automation Blueprint**:

1. **Executive Summary:** A brief recap of their business goals and the primary bottlenecks identified.
2. **Agentic Workflows:** 3–5 specific, step-by-step workflows they can implement. For each workflow, specify:
   - The **Trigger** (e.g., "Every morning," "When an email arrives")
   - The **Tools** (e.g., Claude Cowork + Slack MCP + Gmail MCP)
   - The **Execution Steps** (What the agent actually does)
   - The **Expected ROI** (Time saved, errors reduced)
3. **Implementation Roadmap:** Step-by-step guide on how to set up the recommended tools.

Save the blueprint to Cloud Brain via `mcp__cloud-brain__write_note` with:

- **folder:** `business`
- **title:** `business blueprint`

This is the canonical location downstream skills look for (Skill 03's Phase 0 search reads `business/business blueprint` to pre-populate the agent designer). The folder MUST NOT include a `brain/` prefix — cloud-brain creates `{folder}/{title}.md` directly; a `brain/` prefix would either nest the note one level deeper or split it across two layouts.

If a prior blueprint exists, the same path overwrites it — that's intentional; one canonical "current" blueprint per workspace. Users who want a historical snapshot can archive the prior version manually before re-running.

---

## Phase 5: Offer to Implement

Immediately after delivering the blueprint, offer next steps:

> "Your Business Automation Blueprint is ready. Would you like me to help you start implementing it? I can walk you through each workflow one at a time — starting with the highest-impact item. Or, if you're ready, run **03 Design Your AI Team** to design your AI team based on this blueprint."

If the client wants to implement now:

1. **Prioritize together:** Ask which workflow to tackle first, or recommend the highest-impact / lowest-friction one.
2. **Pre-flight check:** Ask what is already installed or configured.
3. **Step-by-step execution:** Walk through each setup action one step at a time — state what needs to be done and why, provide the exact command or UI action, wait for confirmation before moving on.
4. **Test the workflow:** Guide the client through a live test run to confirm it works end to end.
5. **Iterate:** After the first workflow is live, offer to move to the next.

If the client wants to return later: summarize the blueprint and remind them they can run this skill any time to pick up where they left off.

---

## Principles

1. **One question at a time.** Never overwhelm the client with a list.
2. **Listen actively.** Reflect back what the client said to confirm understanding before moving to the next level.
3. **Be specific about tools.** Don't say "use an MCP" — name the actual MCP (Gmail MCP, HubSpot MCP, etc.).
4. **Lead with ROI.** Every recommendation should include an honest estimate of time saved.
5. **Always offer implementation.** Do not end the session without asking.
