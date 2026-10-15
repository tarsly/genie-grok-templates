# Genie Foundation — AGENTS.md

*Read by 03 Design Your AI Team and Agent Activator to understand what this plugin provides.*

---

## About This Plugin

Genie Foundation is the starting point for every Genie client. It contains three guided skills that help a client get clear on where they're headed (01 Clarify Your Goals), map their business for AI automation (02 Map Your Business), and design their AI agent team (03 Design Your AI Team).

These are setup skills. They are designed to be run in sequence on first install, and revisited at major milestones (annual planning, business pivots, new ventures). They are not scheduling-oriented — each skill is a one-time or annual workflow, not a daily or weekly task.

---

## Skills Catalog

| Skill | Trigger Phrases | What It Does |
|---|---|---|
| `01-clarify-your-goals` | "run 01", "goal architect", "set my goals", "build my goal hierarchy", "where do I want to be in 5 years", "I don't know what I'm working toward", "life planning", "reverse engineer my goals" | Guided coaching interview → 5-level goal hierarchy → conflict analysis → keystone goal → saved to Cloud Brain |
| `02-map-your-business` | "run 02", "biz blueprint", "map my business", "automate my business", "how can AI help my business", "business automation plan", "what should I automate" | Business automation discovery interview → agentic workflow recommendations → implementation roadmap |
| `03-design-your-ai-team` | "run 03", "agent designer", "design my agent team", "what agents should I have", "build my AI staff", "design my team", "set up agents for my company" | Guided team design interview → visual org chart → saved team roster in Cloud Brain → activation roadmap |

---

## Preferences Registry

| Skill | Preferences Saved | Path |
|---|---|---|
| `01-clarify-your-goals` | EOS language preference, coaching style, goal mode (full/business) | `preferences/language-and-frameworks` |
| `02-map-your-business` | None — all inputs are job-specific | — |
| `03-design-your-ai-team` | Agent team roster per company | `agent-teams/{company-slug}` |

---

## Suggested Configuration

### First-Time Client Setup (recommended sequence)
Run these in order on first install for the clearest, most connected experience:

1. **01 Clarify Your Goals** — establishes what the client is building toward; this context flows into all future work
2. **02 Map Your Business** — maps the business and identifies the highest-impact automation opportunities
3. **03 Design Your AI Team** — reads the business blueprint and designs a tailored AI team based on what was discovered in steps 1 and 2

No sequence enforcement — clients can run any skill independently at any time.

### Annual Revisit
- **01 Clarify Your Goals** annually (choose "Annual review" when prompted)
- **03 Design Your AI Team** when the business grows and needs new agents

---

## Schedules Table

These skills are setup and planning tools, not recurring tasks. They are not typically scheduled via Agent Activator. The exception is if a client wants an annual reminder to revisit their goal hierarchy.

| Task | Skill | Suggested Schedule |
|---|---|---|
| Annual goal hierarchy review reminder | `01-clarify-your-goals` | Once per year (typically set via the 01 Clarify Your Goals built-in scheduler) |

For ongoing system health (weekly goal check-ins, platform updates), see the **Genie Admin** plugin.
