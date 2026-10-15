# Getting Started (Foundation)

**Version:** 1.0.0
**Author:** the provider
**Skills:** 3

---

## What This Plugin Does

Genie Foundation is the starting point for every Genie client. It provides three guided skills that work best in sequence — but each one stands on its own and can be run at any time.

**The arc:**
1. **Get clear on where you're headed** — map your goals from long-term vision down to daily habits
2. **Map your business for AI** — identify the highest-value automation opportunities across your operations
3. **Design your AI team** — build a tailored agent roster with a visual org chart and activation roadmap

Run them in order for the most connected, context-aware experience. Or jump to whichever one you need right now.

---

## The Three Skills

### 01 Clarify Your Goals
A structured coaching interview that reverse-engineers your ideal life — from 10-year vision down to quarterly priorities and daily habits. Includes conflict analysis (which goals are quietly working against each other), keystone goal identification, and outputs a Word document + PDF you can keep.

**Trigger phrases:** "run 01", "clarify your goals", "goal architect", "set my goals", "build my goal hierarchy", "where do I want to be in 5 years", "I don't know what I'm working toward", "life planning", "reverse engineer my goals", "goal setting"

**What gets saved:** Goal hierarchy, quarterly priorities, conflicts, language and framework preferences — all saved to your Cloud Brain for use by Goals Pulse and all future Genie skills.

**Time required:** 30–45 minutes (Full Life View) or 15–20 minutes (Business Focus)

---

### 02 Map Your Business
A multi-level discovery interview that maps your business from goals down to specific manual tasks — then matches each task to the right Claude product and MCP integration. Outputs a Business Automation Blueprint with 3–5 ready-to-implement agentic workflows, an ROI estimate for each, and an implementation roadmap.

**Trigger phrases:** "run 02", "map your business", "biz blueprint", "map my business", "automate my business", "how can AI help my business", "business automation plan", "what should I automate", "find my automation opportunities", "where can I use AI agents"

**What gets saved:** Business Automation Blueprint saved to your Cloud Brain for reference by 03 Design Your AI Team and future sessions.

**Time required:** 20–40 minutes depending on business complexity

---

### 03 Design Your AI Team
A guided team design session that builds your complete AI agent roster — org chart, job titles, job descriptions, persona names, and a prioritized activation roadmap. Reads your business blueprint automatically to pre-populate suggestions. Outputs a visual org chart rendered inline and an optional Word document Team Charter.

**Trigger phrases:** "run 03", "design your AI team", "agent designer", "design my agent team", "what agents should I have", "build my AI staff", "design my team", "set up agents for my company", "staff my business with AI", "create agent personas"

**What gets saved:** Full agent team roster saved to your Cloud Brain — available to Agent Activator, Agent Reporter, and Agent Viewer.

**Time required:** 20–30 minutes

---

## Recommended First-Time Setup

For the best experience on first install, run the skills in order:

```
01 Clarify Your Goals → 02 Map Your Business → 03 Design Your AI Team
```

Each skill reads from the previous one — Clarify Your Goals' outcomes inform Map Your Business's priorities, and Map Your Business's automation map informs Design Your AI Team's recommendations. Running them out of sequence still works; you'll just miss some of the contextual pre-population.

---

## Returning to This Plugin

You don't have to run all three every time. Common return scenarios:

- **Annual planning** → re-run 01 Clarify Your Goals (choose "Annual review" when prompted)
- **Business pivot or new venture** → re-run 02 Map Your Business
- **Growing your AI team** → re-run 03 Design Your AI Team to add new agents or redesign existing ones

---

## What Gets Saved to Your Brain

| Skill | Saved Data | Cloud Brain Path |
|---|---|---|
| 01 Clarify Your Goals | Goal hierarchy, quarterly priorities, conflicts, keystone goal | `goals/`, `preferences/language-and-frameworks` |
| 02 Map Your Business | Business Automation Blueprint | `brain/projects/biz-blueprint-[date].md` |
| 03 Design Your AI Team | Agent team roster and org chart | `agent-teams/{company-slug}` |

All data is saved to your private Cloud Brain — it belongs to you and is not shared.

---

## How to Update Your Preferences

**Clarify Your Goals preferences** (EOS language, coaching style, goal mode): Say "update my goal preferences" at the start of a Clarify Your Goals session and it will walk you through resetting them.

**Design Your AI Team preferences** (team roster): Say "redesign my agent team" or "add agents to my team" to re-enter Design Your AI Team and update your org chart.

---

## What's in This Version

**v1.0.0** — Initial release. Three foundation skills packaged together for the first time: 01 Clarify Your Goals, 02 Map Your Business, 03 Design Your AI Team. All skills use Cloud Brain for persistent memory.

---

## Companion Plugin

After completing Genie Foundation, install **Genie Admin** for ongoing system health:
- **Goals Pulse** — weekly 5–10 minute goal alignment check-in (can be scheduled to run automatically every Monday)
- **Update Blueprint** — monthly platform update check


## Grok Bot import
See the root README. Files: `system-prompt.md`, `workflows/*/SKILL.md`, `capabilities.md`.
