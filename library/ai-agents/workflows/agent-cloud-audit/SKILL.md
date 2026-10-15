---
name: agent-cloud-audit
description: >-
  Audits a client's Claude scheduled tasks (Routines), local session cron jobs, and Claude-built AI agents/skills to see which run locally vs. in the cloud, scores each on reliability, compute needs, remote-access needs, cost, and data-privacy, and recommends 'keep local,' 'move to cloud,' or 'no change.' For anything worth moving, explains the tradeoff and — with explicit per-item approval — creates the equivalent cloud scheduled task. Use when the client asks things like 'should this be running in the cloud', 'audit my automations', 'check if my scheduled tasks are set up right', 'is this AI agent local or cloud', 'help me move my cron jobs to the cloud', or wants a health check on how their Claude automations are hosted. Also trigger if the client seems confused why a scheduled task didn't fire while their computer was off, or mentions local cron/CronCreate/session-bound jobs that 'disappeared.
---

# Cloud Migration Audit

## Overview

Many Claude setups end up with a mix of automations: some are true cloud scheduled tasks (Routines) that fire on a server regardless of whether the client's computer is on, and some are local — a cron job living inside a single session, or an agent/skill that only works because it's reading files off the client's machine through the device bridge. Clients often don't realize which is which until something silently fails to run.

This skill inspects what's actually visible in the current session, scores each item against a fixed set of criteria, and gives the client a clear verdict per item: keep it local, move it to the cloud, or leave it as-is. For anything worth moving, it explains the tradeoff in plain language, asks for explicit approval, and — only after approval — creates the cloud equivalent.

Run the phases below in order. Don't skip the discovery phase to jump straight to migrating something the client mentions by name — the point of the audit is to catch things the client forgot about too.

## When This Skill Applies

- Client asks "should this be running in the cloud?" or "is this agent local or cloud?"
- Client wants an audit or health check of their automations and scheduled tasks
- Client wants to move local cron jobs to the cloud
- A scheduled task didn't fire while the client's computer was off, and they don't know why
- Client mentions local cron, `CronCreate`, or session-bound jobs that "disappeared"
- After running **Agent Activator**, to verify the agent's tasks were scheduled where they'll actually run
- Periodic check-in — pairs well with the **Genie Admin** Goals Pulse cadence

## Pre-Flight — Tool Availability

This skill is distributed to many different clients with different setups and different tool availability. Never assume a specific client's environment. Before Phase 1, check what's actually reachable:

| Capability | Primary tool | Fallback |
|---|---|---|
| Cloud scheduled tasks | `list_triggers` / `create_trigger` | `mcp__scheduled-tasks__*` equivalents if that's what's connected |
| Local session cron | `CronList` / `CronDelete` (may be deferred — load with `ToolSearch` query `select:CronList,CronDelete`) | State it's unavailable and skip |
| Agent roster | **Agent Viewer** / Cloud Brain `Agent Team` notes | Best-effort context scan |

If `CronList`, the agent roster, or other referenced tools aren't present in a given session, degrade gracefully: state the limitation and keep going with what is available rather than stalling out or guessing. The audit is still useful even if only `list_triggers` is available.

No preference interview is needed — this skill is read-first and asks nothing it can inspect.

## How It Works

### Phase 1 — Discovery (live inspection, no interview)

Gather everything inspectable from the current session. Do not ask the client to describe their setup from memory first — check the tools, then use what you find to ask sharper follow-up questions if needed.

**1. Cloud scheduled tasks (Routines).** Call `list_triggers` (paginate with `cursor` if there are more than one page). For each Routine returned, note: name, `cron_expression` / `run_once_at`, enabled state, `persistent_session_id`, and `ended_reason` / `suspension_reason` if disabled.

- A Routine with no `persistent_session_id` (or one that starts a fresh session each fire) is a genuine, stateless cloud task — the strongest form of "in the cloud."
- A Routine bound to a `persistent_session_id` (self-bound, e.g. created via `send_later`, or explicitly bound to a long-running session) still fires from the cloud, but delivers into a specific session's conversation. Flag this distinction in the summary — it matters if that session ever gets cleaned up or if the client expects it to behave like a fresh-session task.
- Disabled Routines (paused, or with an `ended_reason`) are worth surfacing even though they're not "local vs cloud" issues — the client may not realize something stopped running.

**2. Local session cron jobs.** If `CronList` is available in this runtime, call it and list what's returned. Explain clearly to the client: anything here runs only inside *this* session's process and is lost the moment the session ends, even if `durable: true` was set — this is the single most common "silent local automation" trap. If `CronList` isn't available in this runtime, say so plainly and move on rather than guessing at what might exist.

**3. Claude-built AI agents / skills with local behavior.** Look for signs of local-anchoring:

- Any skill, agent config, or recent conversation mentioning `device_bash`, `mcp__remote-devices__*` tools, local file paths (e.g. `~/Documents`, `C:\Users\...`), or phrases like "runs on my computer" / "my local files."
- Skills whose descriptions or bodies reference locally-installed software, GPU-bound work, or credentials only available on the client's machine.
- If **Agent Viewer** (this plugin) or similar agent-management tooling is available, use it to enumerate configured agents and check each one's dependencies the same way. The `Agent Team: {Company Name}` note in Cloud Brain lists each agent's `active_tasks` and `channels` — a fast way to cross-check that every task an agent claims to run actually has a live Routine behind it.

Treat this phase as best-effort — you can only see what's visible to the current session's tools and recent context, not the client's entire history. State this limitation explicitly in the summary rather than implying full coverage.

**4. State the boundary of what you checked.** Before moving to scoring, tell the client in one or two sentences what you *can't* see: automations built outside Claude entirely (OS task scheduler, Zapier, n8n, standalone scripts) are out of scope for this audit, and anything from a different Claude session or device that isn't reflected in this session's visible state won't show up either.

### Phase 2 — Score each item

For every item found in Phase 1 (cloud Routines included — they act as a baseline showing what "already migrated" looks like), score against these five criteria. Don't turn this into a numeric weighted formula — the goal is a defensible one-line rationale a non-technical client can understand, not a spreadsheet.

| Criterion | What to ask | Which way it pulls |
|---|---|---|
| **Reliability / uptime** | Does this need to fire even when the computer is off or the client isn't watching? | Yes → strong pull to **cloud** |
| **Compute / resources** | Lightweight enough for a cloud session, or does it need licensed local software, a GPU, or device-bridge files? | Local-only resources → **local** |
| **Remote / multi-device** | Does the client want to trigger or check this from a phone or another computer? | Yes → **cloud** (local cron and device-bridge flows only work from one machine) |
| **Cost sensitivity** | Cloud Routines create a session and token usage each time they fire; a manual local run only costs when run. | A consideration for high-frequency or heavy tasks — never the deciding factor on its own |
| **Data sensitivity / privacy** | Does it read local-only sensitive data that shouldn't be pulled into a cloud-run session? | Yes → strong pull to **local**, overriding the other four |

Assign each item one verdict:

- **Move to cloud** — reliability/remote-access needs dominate and nothing blocks it on privacy or local-resource grounds.
- **Keep local** — genuinely needs local resources/data, or privacy makes cloud migration inappropriate.
- **No change needed** — already in the right place (e.g. a cloud Routine that's working as intended), or the item is low-stakes enough that moving it isn't worth the effort.

### Phase 3 — Present the audit summary

Before touching anything, show the client a plain summary — a short table, one row per item. Keep this scannable; save the detailed reasoning for follow-up if asked.

### Phase 4 — Migrate, item by item, with approval gates

For each item verdicted "Move to cloud," don't batch this — walk through one at a time:

1. **Explain the concrete change** in plain language: what will be different once it's a cloud Routine (e.g. "this will fire on Anthropic's servers even when your laptop is closed" or "you'll be able to trigger this from your phone").
2. **Ask for explicit approval before migrating.** Use `AskUserQuestion` if available in this runtime; otherwise ask plainly in text and wait for a clear yes. Never migrate on an implied or assumed yes.
3. **On approval, create the cloud equivalent** with `create_trigger`, mapping over what you found in Phase 1: `cron_expression` or `run_once_at` from the local job's schedule, and a `prompt` written as a complete standalone instruction. Every Routine firing starts a fresh session with no memory of the old local session, so the prompt must be self-contained — don't write something that assumes prior context.
4. **Confirm success**, then ask whether to retire the old local-only version now that it's redundant (e.g. `CronDelete` for a local cron job). Only do this after explicit confirmation — never delete anything automatically, even something you just replaced.
5. **If something can't be safely auto-migrated** — most commonly because it depends on local-only files or credentials reachable only via the device bridge — say so clearly and give a manual checklist instead (e.g. "keep this running locally, but here's how to make sure it doesn't silently die: ..."). Don't fabricate a working cloud migration for something that structurally needs local access; a fake success is worse than an honest "this one has to stay local."

### Phase 5 — Save the audit (optional)

Offer to save the result so the next audit can diff against it:

- `write_note` → title: `Automation Audit — {YYYY-MM-DD}`, folder: `agents`, tags: `["audit", "automations", "cloud-migration"]`

Only write this if the client says yes. If the client declines, the audit still stands — it's a report, not a record-keeping requirement.

## Data Structure

Saved audits use this format:

```markdown
# Automation Audit — {YYYY-MM-DD}

**scope_checked:** cloud Routines / local session cron / agent roster
**tools_unavailable:** {list any that couldn't be inspected, or "none"}

## Items

| Item | Location | Verdict | Rationale |
|---|---|---|---|
| {name} | cloud / local-cron / local-dependent agent | move / keep / no-change | {one line} |

## Migrated This Session

- {item} → cloud Routine `{name}`, schedule `{cron}` — old local job {retired / left in place}

## Left Local (with reason)

- {item} — {local resource or privacy reason}

## Out of Scope

Automations outside Claude (OS scheduler, Zapier, n8n, standalone scripts) and anything not
visible from this session were not checked.
```

## Output Format

```
🔍 Automation Audit | Checked: {N} cloud Routines, {N} local cron jobs, {N} agents

| Item | Where it runs now | Verdict | Why |
|---|---|---|---|
| Monday pipeline report | ☁️ Cloud Routine | ✅ No change | Fires server-side, stateless — already right |
| Nightly file backup | 💻 Local session cron | ⚠️ Keep local | Reads ~/Documents via device bridge |
| Daily market scan | 💻 Local session cron | 🚀 Move to cloud | Needs to run when laptop is closed; no local deps |
| Client digest | ☁️ Cloud (session-bound) | ⚠️ Review | Delivers into one session — breaks if that session is cleaned up |

Not checked: automations built outside Claude (OS scheduler, Zapier, n8n) and anything
from other sessions or devices.

1 item worth moving. Want to walk through it?
```

Then Phase 4, one item at a time.

## Example Usage

**Client:** "My Monday report didn't run this week and my laptop was off — can you check my automations?"

1. Calls `list_triggers` → finds 3 Routines, one disabled with `ended_reason`.
2. Loads `CronList` via `ToolSearch` → finds 2 session cron jobs, including "Monday report."
3. Explains the Monday report was a *local session* cron, not a cloud Routine — it died with the session that created it.
4. Scores it: high reliability need, no local dependencies → **move to cloud**.
5. Shows the summary table, then asks for approval on that one item.
6. On yes, creates the Routine with a self-contained prompt, confirms, asks whether to delete the dead local job.

**Other triggers:** "audit my automations" · "is my agent running in the cloud?" · "why didn't my scheduled task fire?" · "move my cron jobs to the cloud"

## Error Handling

| Situation | Response |
|---|---|
| `list_triggers` unavailable | Say so plainly. Audit local cron and agents only, and flag that cloud Routines couldn't be verified. |
| `CronList` unavailable | State it and move on. Don't speculate about what local jobs might exist. |
| No automations found anywhere | *"Nothing scheduled and nothing local — you're clean. Run **Agent Activator** when you're ready to put an agent on a schedule."* Don't manufacture findings. |
| Nothing worth moving | Say so plainly and stop. Most clients won't have anything badly wrong — this is a health check, not a scary report. |
| `create_trigger` fails | Report the actual error. Do **not** delete or disable the local original — the client is still covered by it. |
| Client declines a migration | Record the "keep local" decision and move to the next item. Don't re-pitch it. |
| Item depends on device-bridge files | Verdict is **keep local**. Give a manual reliability checklist instead of a fake migration. |

Keep the tone practical, not alarmist. The goal is a quick health check.

---

**Related skills:** **Agent Activator** (creates the scheduled tasks this audits) · **Agent Viewer** (agent roster and task status) · **Genie Admin → Goals Pulse** (recurring check-in cadence)
