# Brain Coach

> Life-coach-framed Cloud Brain tooling. Seven brain rituals that turn your Cloud Brain into a daily performance system.

Brain Coach treats the **Cloud Brain itself** as the product. Every skill in this plugin is a thin, well-prompted shell over `mcp__cloud-brain__*` calls — capture, recall, journal, daily-goals, mantras, habits, and evening reflection. The "life coach" framing only shows up in *what* the skill prompts for and *how* it writes to the brain, not in skill names.

This plugin is intentionally scoped to **personal/identity** rituals. Business workflows (CEO briefings, pipeline, EOS Rocks, weekly business reviews) live in other plugins (`business-operations`, `eos-operator`, `genie-admin`) and Brain Coach does not duplicate them.

---

## Skills

| Skill | What it does |
|---|---|
| **`brain-dump`** | Quick-capture brain dump — speak or type any thought, AI routes it to the right Cloud Brain folder (people, projects, ideas, inbox, daily). |
| **`brain-recall`** | Natural-language search across Cloud Brain with the canonical short-keyword → `recent_activity` → semantic fallback cascade. |
| **`brain-journal`** | Multi-mode personal journal: `free`, `gratitude`, `decision`, `emotional`, `dream`. |
| **`brain-evening`** | Evening reflection ritual — 3 gratitudes, Win of the Day, what worked / what didn't, tomorrow's top 3. Appends to today's daily note. |
| **`brain-daily-goals`** | Daily goal-writing ritual — reads your goal hierarchy, captures today's expression of quarterly priorities + 1MIT + identity declaration. |
| **`brain-mantras`** | Affirmations / identity statements — manage, run a daily read-aloud session, quarterly audit. |
| **`brain-habits`** | Non-negotiables tracker — single-message daily checklist, current streaks, weekly heatmap. |

---

## Why this plugin exists

Cloud Brain is a remarkable piece of infrastructure — but raw infrastructure isn't a practice. A performance coach knows that **the practice is the product**: writing your goals every day, journaling consistently, closing the day with reflection, tracking the non-negotiables you said you'd live by. Brain Coach is the practice layer for that infrastructure.

Each skill is engineered around **one principle**: the friction between the intention and the act is where habits die. So:

- `brain-dump` is one prompt → one save → one-line confirm. No menus.
- `brain-recall` translates your English into the right kind of Cloud Brain query so you never miss content that's actually there.
- `brain-journal` modes are pre-configured prompt flows — no blank-page paralysis.
- `brain-evening` and `brain-daily-goals` are 5-10 minute rituals, not 30-minute deep work sessions.
- `brain-mantras read` paces one mantra at a time so each one lands.
- `brain-habits log` is one message with all checkboxes — no per-habit interrogation.

---

## How the skills chain

```
Morning:    /brain-daily-goals   →  /bizops-daily-brief  (business briefing — separate plugin)
Throughout: /brain-dump          →  /brain-recall
Evening:    /brain-evening       →  /brain-habits log    (optional auto-chain)
Weekly:    /brain-journal --mode free  (Sunday reflection)
Quarterly: /brain-mantras audit
```

---

## Cloud Brain folders this plugin uses

Brain Coach uses existing Cloud Brain folder conventions (`people/`, `projects/`, `daily/`, `ideas/`, `inbox/`) plus three new folders:

| Folder | Purpose | Owner |
|---|---|---|
| `journal/` | Multi-mode personal journals (`free`, `gratitude`, `decision`, `emotional`, `dream`) | `brain-journal` |
| `habits/` | Habits list + monthly logs + archive | `brain-habits` |
| `mindset/` | Mantras list + read-aloud log + retired mantras | `brain-mantras` |

All preferences are stored under `brain/preferences/` per the repo convention.

---

## What this plugin does NOT do (and where to go instead)

| Need | Use this | Not this |
|---|---|---|
| CEO morning briefing (pipeline, projects, calendar) | `bizops-daily-brief` (business-operations) | `brain-daily-goals` is identity, not intelligence |
| 5-min goal alignment check-in against existing hierarchy | `genie-admin/goals-pulse` | `brain-daily-goals` writes daily; goals-pulse reflects |
| Quarterly business goals (Rocks) | `eos-rocks` (eos-operator) | brain-* is personal, not business |
| Annual / quarterly business review | `eos-annual-planning`, `eos-quarterly-pulsing` | brain-* doesn't touch business cycles |
| Tracking commitments to other people | `bizops-follow-up` | `brain-dump` and `brain-habits` are personal; follow-up is interpersonal |
| Business weekly review | `bizops-weekly-review` | brain-* skills are identity/daily |
| Contact CRM | `bizops-people` | brain-* doesn't manage contacts |

---

## Trademark / Attribution

This plugin draws on broadly available coaching and habit-formation practices (gratitude journaling, expressive writing per James Pennebaker, identity-based habit work per Atomic Habits, daily goal reinforcement, 1MIT prioritization, dream-pattern journaling). No proprietary or trademarked framework is being licensed or reproduced.

---

## Installing

This plugin is part of the the provider Library marketplace. Install via the marketplace flow. After install, every skill auto-runs a pre-flight check the first time you invoke it to capture your preferences (stored in Cloud Brain at `brain/preferences/brain-coach-*`).

Requires the `cloud-brain` MCP server connected to your Claude Code session.

---

## Versioning

- **1.0.0** — Initial release with 7 skills.

Planned for future minor versions:
- `brain-weekly` — Sunday Fueling Station (weekly personal planning) — v1.1
- `brain-life-review` — Personal quarterly/annual life review (not business EOS) — v1.2
- `brain-reading` — Reading & book-notes log — v1.3


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
