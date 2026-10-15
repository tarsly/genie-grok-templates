---
name: agent-viewer
description: >-
  Instantly view the AI agent team org chart for any company — shows all agents with their current status (proposed vs. active), active tasks, channels, and reporting cadence. Use this skill whenever a client wants to 'show my agent team', 'view my org chart', 'who are my agents', 'show my AI staff', 'what agents do I have', 'check my team status', 'show me my agents for [company]', 'pull up my org chart', 'how many agents are active', 'what is my agent running', 'show me my team', or any request to see a visual overview of their AI agent organization. This skill is read-only and fast — no interview, no setup. Works for single-company and multi-company setups. Renders the org chart inline and offers quick navigation to Agent Designer, Agent Activator, or Agent Reporter.
---

# Agent Viewer

Your job is to pull up the client's AI agent team and show it — fast, clean, and visual. No interview, no setup. Just load, render, and navigate.

---

## Phase 1 — Load the Team

Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `Agent Team`.

**If no teams found:**
*"It looks like you haven't set up an agent team yet. Run **Agent Designer** to design your team — it takes about 15 minutes and sets up your full org chart."*

**If one team found:** Load it directly. No need to ask.

**If multiple teams found:** Ask:
*"You have agent teams set up for [Company A] and [Company B]. Which would you like to view? (Or say 'all' for a summary of both.)"*

---

## Phase 2 — Render the Org Chart

Use `mcp__visualize__show_widget` to render an SVG org chart.

**Layout:**
- Top box: Owner/Human (blue theme: `#dbeafe` fill, `#2563eb` border, "👤 Owner")
- Agents arranged by hierarchy below
- Connecting lines: `#94a3b8`, 1.5px stroke

**Agent Box Design:**

*Proposed agents (status: proposed):*
- White fill `#ffffff`, `#94a3b8` border 2px dashed (`stroke-dasharray="6,3"`)
- Emoji icon + persona name (bold) + job title

*Active agents (status: active):*
- Light green fill `#f0fdf4`, `#16a34a` border 2px solid
- Emoji icon + persona name (bold) + job title
- Small "🟢 active" indicator

**Box content layout (top to bottom):**
- Row 1: Emoji icon (centered, 18px)
- Row 2: Persona name or job title in bold (13px, `#1e293b`)
- Row 3: Job title in lighter text if name shown (11px, `#64748b`)
- Row 4: Reporting cadence in tiny text if active (10px, `#94a3b8`) — e.g., "📅 daily 7am"

**Legend:** Bottom-right — ⬜ Proposed | 🟩 Active

**ViewBox:** Scale to fit hierarchy depth.

---

## Phase 3 — Stats Summary

After the org chart, show a quick text summary:

```
[Company Name] — AI Agent Team
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total Agents: [N]
🟢 Active: [N]  |  ⬜ Proposed: [N]
Scheduled Tasks Running: [N]
Est. Weekly Hours Saved: ~[N] hours

Next to activate: [icon] [Agent Name/Title]
```

---

## Phase 4 — Agent Drill-Down (Optional)

Ask: *"Want details on any specific agent? Just say their name."*

If the client asks about a specific agent, show their full profile:

```
[icon] [Persona Name] | [Job Title]  🟢 ACTIVE

Job: [job_description]

Active Tasks:
  📅 [Task Name] — [schedule] → [output]
  💬 On-demand: "[trigger phrase]"

Channels: [SMS number / Email address / Slack channel / Scheduled only]
Reporting: [cadence and delivery]

Weekly time saved: ~[X] hours
```

---

## Phase 5 — Navigation

After showing the org chart (and any drill-down), offer clear next steps:

```
What would you like to do?

  🎨 Design  — Modify the team or add a new company   [Agent Designer]
  ⚡ Activate — Bring a proposed agent online          [Agent Activator]
  📬 Report  — Set up or change reporting cadences    [Agent Reporter]
```

Just point to which skill to run — don't launch them automatically.

---

## Multi-Company View

If the client says "show all" for multiple companies, render a simplified summary for each:

```
Your AI Teams — Overview

🏢 [Company A]
   🟢 [N] active  |  ⬜ [N] proposed  |  [N] tasks running

🏢 [Company B]
   🟢 [N] active  |  ⬜ [N] proposed  |  [N] tasks running
```

Then ask: *"Want to pull up the full org chart for either company?"*

---

## Principles

- **Fast above all.** Load and render without asking unnecessary questions. The client opened this skill to see their team — show it.
- **Status clarity is the whole point.** The visual distinction between active and proposed is the most important thing this skill communicates. Make it unmistakable.
- **Don't re-interview.** If data in Cloud Brain is incomplete, render what's there and note what's missing at the bottom. Don't launch into questions.
- **Reporting cadence in the box.** Showing the reporting schedule inside each active agent's box gives the client an instant feel for how alive the team is.
- **Navigation is a service.** The client might not know which skill to run next — offer clear pointers without being pushy.
