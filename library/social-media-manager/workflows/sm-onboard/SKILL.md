---
name: sm-onboard
description: >-
  Social Media Suite getting started — run this first. Guides new clients through the complete setup sequence: tool connections, discovery interview, brand snapshot, and strategy build. Use when: 'get started', 'set up my social media', 'onboard me', 'I just installed this plugin', 'social media setup', 'start the social media suite', 'configure my social media agent'.
---

# Social Media Suite — Onboarding Coordinator

## Overview

This skill is the front door to the Social Media Suite. It guides the client through a one-time setup sequence that configures everything needed for the Social Media Manager AI agent to run autonomously. Run this skill once at the beginning — after that, each individual skill runs standalone on a schedule.

**Estimated time:** 30–45 minutes for first-time setup.

**What the client has at the end:**
- Social media tools connected to their AI agent
- Full discovery on goals, platforms, and audience saved to memory
- Brand voice and content pillars defined
- A platform strategy with optimal posting times
- Their Social Media Manager agent ready to run

---

## Pre-Flight — Check for Existing Setup

Before starting, search Cloud Brain for any existing social media preferences:

```
mcp__cloud-brain__search_notes: "social media suite preferences"
```

**If preferences are found:** Confirm with the client — *"It looks like you've already been set up. Would you like to re-run onboarding to update your settings, or would you prefer to jump straight to a specific skill?"*

If they want to update: proceed with the full sequence below. Their existing preferences will be overwritten where they provide new answers.

If they want to skip: point them to the skill they need and stop.

**If no preferences found:** Proceed with the full onboarding sequence below.

---

## Onboarding Sequence

Deliver this as a warm, guided conversation — not a form. Explain what each step does and why before starting it.

---

### Welcome Message

Deliver this at the start:

---

*Welcome to your Social Media Suite — I'm going to walk you through a one-time setup that takes about 30–45 minutes. When we're done, your AI Social Media Manager will be fully configured and ready to run your weekly content workflow.*

*Here's what we'll do together:*

*1. **Tool Setup** — Connect your scheduling tool (Metricool) and inventory any other tools you use (CapCut, Descript, Canva, etc.)*
*2. **Discovery** — Understand your business, platforms, goals, and audience*
*3. **Brand Snapshot** — Define your brand voice and content pillars*
*4. **Strategy** — Build your platform strategy and optimal posting schedule*

*Everything you share will be saved to your private AI brain so you never have to answer these questions again. Let's start.*

---

### Step 1 — Tool Setup

Say: *"First, let's connect your tools. This is the most important step — it's what lets your agent schedule posts automatically rather than just drafting them."*

Then run the **sm-tool-setup** skill in full.

When sm-tool-setup completes, confirm: *"Great — your tools are connected. Moving on to learning about your business."*

---

### Step 2 — Discovery Interview

Say: *"Now I want to understand your business, your goals, and your audience so we can build a strategy that actually fits you."*

Then run the **sm-discovery** skill in full.

When sm-discovery completes, confirm: *"Perfect — I've got a clear picture of your business and what you're trying to accomplish. Next let's make sure your brand voice is defined."*

---

### Step 3 — Brand Snapshot

Say: *"Before we build your content strategy, I need to know how you sound and what your brand stands for. This shapes every post your agent creates."*

Then run the **sm-brand-snapshot** skill in full.

When sm-brand-snapshot completes, confirm: *"Your brand identity is saved. Now let's build your strategy."*

---

### Step 4 — Strategy Build

Say: *"This is where everything comes together. I'm going to build your platform-specific strategy — which platforms to focus on, what content pillars to post around, how often to post, and when."*

Then run the **sm-strategy-builder** skill in full.

---

### Closing Summary

When all four steps are complete, deliver this summary:

---

*Your Social Media Suite is live. Here's what's been set up:*

*✅ Tools connected: [list tools from sm-tool-setup]*
*✅ Platforms configured: [list platforms from sm-discovery]*
*✅ Brand voice defined: [brand voice summary from sm-brand-snapshot]*
*✅ Strategy built: [platform count] platforms, [pillar count] content pillars, [posting frequency] posts/week*

*Here's how your weekly workflow runs from here:*

| Day | What Happens |
|---|---|
| Monday | **sm-trend-watcher** — researches what's trending in your industry |
| Tuesday | **sm-content-creator** — writes this week's posts based on trends + your strategy |
| Wednesday | **sm-visual-creator** — creates graphics in Canva for each post |
| Thursday | **sm-schedule-queue** — presents posts for your approval, then schedules in Metricool |
| 1st of month | **sm-analytics-review** — reviews last month's performance |
| 1st of month | **sm-content-calendar** — plans next month's content themes |

*On-demand anytime: **sm-competitor-watch** (see what's working in your industry), **sm-repurpose** (turn existing content into posts)*

*To activate your AI agent and put this on a schedule, run **system-04 (Agent Activator)** and assign the Social Media Manager agent.*

*You're all set. What would you like to do first?*

---

## Error Handling

**If the client stops partway through:** Save whatever has been completed to Cloud Brain with a note: `[date] — Onboarding incomplete. Completed steps: [list]. Resume with [next step].`

**If the client already has some tools connected but not others:** Don't re-ask — only collect what's missing.

**If the client has an urgent task right now:** Acknowledge it, collect minimum preferences (name, business, platforms), save to Cloud Brain, and let them proceed. Flag: *"You can complete the full setup later by running sm-onboard again — I've saved your basics so we won't start from scratch."*
