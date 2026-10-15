# Social Media Suite — Agent Capabilities Manifest

*For use by system-03 (Agent Designer) and system-04 (Agent Activator)*
*Last updated: 2026-06-12*

---

## About This Plugin

`social-media-manager` is a complete AI-powered social media management system for solo entrepreneurs and small teams. It covers the full weekly workflow: strategy, brand identity, content planning, post writing, graphic creation (Canva), scheduling (Metricool MCP), performance analytics, trend research, and competitor monitoring.

**Target users:** Solo entrepreneurs, creators, coaches, consultants, and small business owners managing their own social media. Clients who want to delegate their social media workflow to an AI agent that starts supervised and progressively earns full autonomy.

**Key integrations:** Metricool MCP (`https://ai.metricool.com/mcp`) for scheduling and analytics; Canva MCP for automated graphic creation; web search for trend research.

**Plugin version:** 1.0.0 (13 skills)

---

## Available Skills Catalog

---

### 1. sm-onboard

**What it does:** One-time setup coordinator. Guides the client through tool connections, discovery interview, brand snapshot, and strategy build in a single guided session. The entry point for all new clients.

**Preferences collected at activation:** All — this skill coordinates the full setup sequence.

**Suggested schedule:** Once only (re-run if the client wants to update their full setup).

**Natural pairings:** Calls sm-tool-setup → sm-discovery → sm-brand-snapshot → sm-strategy-builder in sequence.

---

### 2. sm-tool-setup

**What it does:** Inventories the client's tools (scheduling, graphics, video, storage), connects Metricool MCP, provides transparent cost breakdowns for all tools, and delivers content upload instructions for video and image assets.

**Preferences collected at activation:**
- Metricool account status and plan tier
- Graphic design tool (Canva, Adobe, other) and plan
- Video creation tool (CapCut, Descript, iMovie, etc.)
- Audio/podcast tool (if applicable)
- Content asset storage location (Google Drive, local, etc.)
- Existing scheduling tool (if migrating)
- Content upload workflow preference

**Suggested schedule:** Once only. Re-run when adding new tools or when Metricool connection needs updating.

**Natural pairings:** Runs inside sm-onboard. Can run standalone when the client adds a new tool.

---

### 3. sm-discovery

**What it does:** Structured discovery interview capturing business details, active social media platforms and handles, goals, target audience, pain points, content production capacity, and competitors to monitor.

**Preferences collected at activation:**
- Business name and description
- Industry and niche
- Target audience (demographics, pain points)
- Active platforms and handles
- Primary and secondary goals
- Pain points with social media
- Content capacity (video, writing, time available)
- Competitor / inspiration accounts (up to 5)
- Current and desired posting frequency

**Suggested schedule:** Once only. Re-run when the business, goals, or platforms change significantly.

**Natural pairings:** Runs inside sm-onboard. Feeds sm-strategy-builder and sm-content-creator.

---

### 4. sm-brand-snapshot

**What it does:** Captures brand voice, content pillars, visual identity, and posting persona. Checks for existing brand audit data first. Offers the full MKT-brand-audit skill if no brand data exists.

**Preferences collected at activation:**
- Brand voice (3 adjectives + description)
- Voice to avoid
- Content pillars (3–5 named themes)
- Brand colors (primary and secondary)
- Visual style
- Logo availability in Canva
- Photo/image style
- Posting persona (personal brand vs. business brand)
- Off-limits topics

**Suggested schedule:** Once only. Re-run when brand identity evolves.

**Natural pairings:** Runs inside sm-onboard. Feeds sm-content-creator (voice) and sm-visual-creator (visual style).

---

### 5. sm-strategy-builder

**What it does:** Builds a complete platform-specific social media strategy from discovery and brand data. Outputs platform tiers, content pillar allocation, posting frequency, optimal timing (via Metricool), content type mix, and an agent configuration recommendation.

**Preferences collected at activation:**
- Platform priority tiers (Tier 1 / Tier 2 / Tier 3)
- Content pillar allocation percentages
- Posting frequency per platform
- Optimal posting times per platform
- Content type mix (% video / graphics / text)
- Agent configuration recommendation (single / two-agent / specialist)

**Suggested schedule:** Once only, then quarterly or when business direction shifts.

**Natural pairings:** Runs inside sm-onboard. Powers sm-content-calendar and sm-content-creator.

---

### 6. sm-content-calendar

**What it does:** Generates a monthly content calendar mapping weekly themes, content pillars, post types, and topic angles to a posting schedule. Accounts for upcoming events, launches, and promotions.

**Preferences collected at activation:** None — reads strategy from Cloud Brain. Job inputs only (which month, any events or promotions).

**Suggested schedule:** Monthly — 1st of each month.

**Natural pairings:** Follows sm-analytics-review (incorporates performance insights). Feeds sm-content-creator.

---

### 7. sm-trend-watcher

**What it does:** Researches what's trending in the client's industry and on their active platforms. Produces a weekly trend brief with 5–7 specific content ideas, a refreshed hashtag set, platform-specific format trends, and a competitor activity snapshot.

**Preferences collected at activation:** None — reads industry and platform data from Cloud Brain. Job inputs only.

**Suggested schedule:** Weekly — Monday morning.

**Natural pairings:** Feeds sm-content-creator. Complements sm-competitor-watch.

---

### 8. sm-content-creator

**What it does:** Writes a full week of platform-optimized social media posts — hooks, captions, CTAs, hashtags, and TikTok/YouTube scripts. Platform-specific formatting for Instagram, TikTok, LinkedIn, Facebook, and YouTube. Includes visual briefs for sm-visual-creator.

**Preferences collected at activation:** None — reads strategy, brand, and trend brief from Cloud Brain. Job inputs only (which week, any specific topics).

**Suggested schedule:** Weekly — Tuesday.

**Natural pairings:** Follows sm-trend-watcher. Feeds sm-visual-creator → sm-schedule-queue.

---

### 9. sm-visual-creator

**What it does:** Creates platform-optimized graphics for each post using the Canva MCP. Reads visual briefs from content drafts. If Canva is not connected, provides detailed manual briefs.

**Preferences collected at activation:** None — reads brand profile and post drafts from Cloud Brain. Job inputs only.

**Suggested schedule:** Weekly — Wednesday.

**Natural pairings:** Follows sm-content-creator. Feeds sm-schedule-queue.

---

### 10. sm-schedule-queue

**What it does:** Presents the week's content drafts for client approval, schedules approved posts via Metricool MCP, and tracks approval patterns over time. Surfaces trust escalation opportunities — when a content type has been approved consistently, offers to auto-schedule it. Manages the client's path from supervised to fully autonomous.

**Preferences collected at activation:**
- Current autonomy level per content type (approval required / auto-scheduled)
- Approval preferences (approve all at once / review individually)

**Suggested schedule:** Weekly — Thursday.

**Natural pairings:** Final step in weekly chain (follows sm-visual-creator). Connects directly to Metricool for scheduling.

---

### 11. sm-analytics-review

**What it does:** Pulls performance data from Metricool, identifies top and underperforming content, compares against strategy goals, benchmarks against competitors, and produces 3–5 specific recommendations for next month.

**Preferences collected at activation:** None — reads strategy and goals from Cloud Brain. Job inputs only (which month).

**Suggested schedule:** Monthly — 1st of each month (pair with sm-content-calendar).

**Natural pairings:** Feeds sm-content-calendar (performance insights shape next month's plan) and sm-strategy-builder (when major strategy shifts are warranted).

---

### 12. sm-competitor-watch

**What it does:** Monitors competitor and industry accounts using Metricool's competitor tracking and web research. Identifies what's getting traction, surfaces content gaps, and produces specific opportunity recommendations.

**Preferences collected at activation:** None — reads competitor list from Cloud Brain. Job inputs only.

**Suggested schedule:** Weekly (alongside sm-trend-watcher) or on-demand.

**Natural pairings:** Feeds sm-trend-watcher and sm-content-creator with competitive intelligence.

---

### 13. sm-repurpose

**What it does:** Takes any existing long-form content (blog post, podcast episode, video transcript, newsletter, webinar) and transforms it into a full set of platform-optimized posts across all active platforms. One piece of content becomes 5–10 ready-to-schedule posts.

**Preferences collected at activation:** None — reads brand and strategy from Cloud Brain. All inputs are job-specific.

**Suggested schedule:** On-demand — whenever the client has existing content to repurpose.

**Natural pairings:** Outputs feed sm-schedule-queue. Particularly powerful when paired with sm-visual-creator.

---

## Preferences Registry

All preferences that can be collected at activation, organized by category. system-04 collects all fields in one consolidated interview (via sm-onboard — which runs the full setup sequence).

**Core Identity:**
- Business name
- Industry and niche
- Business stage
- Geographic focus

**Platforms:**
- Active platforms (Instagram, TikTok, LinkedIn, Facebook, YouTube, Pinterest, Threads, X, Bluesky)
- Handle / username / URL per platform
- Current follower count per platform

**Goals:**
- Primary goal (brand awareness / lead gen / sales / community / thought leadership / retention)
- Secondary goal
- 90-day specific outcome

**Audience:**
- Target audience demographics
- Psychographics and pain points
- Geographic market

**Brand Identity:**
- Brand voice (3 adjectives)
- Voice to avoid
- Content pillars (3–5 named themes with descriptions)
- Brand colors (primary and secondary)
- Visual style
- Posting persona (personal brand / business / blend)
- Off-limits topics

**Content Capacity:**
- Video production capability (yes/no/type)
- Writing comfort level
- Time available per week (hours)
- Photo/image assets available

**Tools:**
- Metricool account status and plan tier
- Graphic design tool
- Video editing tool
- Audio/podcast tool
- Content storage location
- Other scheduling tools in use

**Strategy:**
- Platform tiers (Tier 1 / Tier 2 / Tier 3)
- Posting frequency per platform
- Content pillar allocation percentages
- Content type mix
- Optimal posting times per platform

**Autonomy:**
- Current approval mode (all supervised / selective auto-scheduling / full autonomy)
- Which content types are set to auto-schedule

**Competitors:**
- Up to 5 competitor or industry accounts to monitor (name + handle + platform)

---

## Suggested Agent Configurations

---

### Option A: Single Social Media Manager Agent (Recommended for Most Clients)

One agent handles the full weekly workflow. Best for solo entrepreneurs, creators, and small business owners on 1–3 platforms.

**Agent name:** Social Media Manager (or a persona name the client chooses)

**All 13 skills assigned to this agent.**

**Activation interview:** Full preferences registry (via sm-onboard). Takes 30–45 minutes once.

**Weekly automated tasks (separate scheduled tasks):**
1. Monday: sm-trend-watcher
2. Tuesday: sm-content-creator
3. Wednesday: sm-visual-creator
4. Thursday: sm-schedule-queue (approval queue → Metricool scheduling)

**Monthly automated tasks:**
5. 1st of month: sm-analytics-review
6. 1st of month: sm-content-calendar

**On-demand tasks (client triggers manually):**
- sm-competitor-watch
- sm-repurpose
- sm-onboard (re-run to update setup)
- sm-strategy-builder (re-run to refresh strategy)

---

### Option B: Content Creator + Social Media Manager (Two-Agent Setup)

Two agents with clear division of responsibility. Best for clients who are active on 4+ platforms, publish daily, have a YouTube channel as a primary platform, or work with a small team where content creation and publishing are separate functions.

**Agent 1: Content Creator**

Responsibilities: research, ideation, writing, and visual direction.

Skills: sm-trend-watcher, sm-content-creator, sm-visual-creator, sm-competitor-watch, sm-repurpose

Weekly tasks:
- Monday: sm-trend-watcher + sm-competitor-watch
- Tuesday–Wednesday: sm-content-creator + sm-visual-creator

**Agent 2: Social Media Manager**

Responsibilities: strategy, scheduling, analytics, and client relationship.

Skills: sm-onboard, sm-tool-setup, sm-discovery, sm-brand-snapshot, sm-strategy-builder, sm-content-calendar, sm-schedule-queue, sm-analytics-review

Weekly tasks:
- Thursday: sm-schedule-queue

Monthly tasks:
- 1st: sm-analytics-review + sm-content-calendar

**Handoff:** Content Creator saves drafts to Cloud Brain → Social Media Manager reads them in sm-schedule-queue for approval and scheduling.

---

## Recommended Schedules

| Skill | Schedule | Notes |
|---|---|---|
| sm-trend-watcher | Weekly — Monday | Research before content is written |
| sm-content-creator | Weekly — Tuesday | Writes from trend brief |
| sm-visual-creator | Weekly — Wednesday | Graphics from content drafts |
| sm-schedule-queue | Weekly — Thursday | Approval → Metricool scheduling |
| sm-competitor-watch | Weekly — Monday (alongside trend-watcher) | Or on-demand |
| sm-analytics-review | Monthly — 1st of month | Review prior month |
| sm-content-calendar | Monthly — 1st of month | Plan next month (after analytics) |
| sm-repurpose | On-demand | Triggered by client when they have content to repurpose |
| sm-onboard | Once only | Re-run to update full setup |
| sm-strategy-builder | Quarterly or on-demand | Re-run when business direction changes |
| sm-brand-snapshot | Once / on-demand | Re-run when brand evolves |
| sm-discovery | Once / on-demand | Re-run when business changes significantly |
| sm-tool-setup | Once / on-demand | Re-run when adding new tools |

---

## How system-03 Should Use This Document

1. **Read the Skills Catalog** to understand capabilities before proposing agent configurations.
2. **Present Option A as the default** for solo entrepreneurs and clients on 1–3 platforms. Present Option B when the client mentions YouTube as a primary channel plus 3+ other platforms, or when they have team support for content production.
3. **Use the Suggested Configurations as conversation starters** — clients are free to customize freely.
4. **Note that sm-onboard coordinates discovery, brand, and strategy** — system-04 does not need to run these separately; sm-onboard handles it.
5. **Flag the Metricool connection as a requirement** for scheduling features. If the client hasn't connected Metricool, sm-tool-setup guides them through it.

---

## How system-04 Should Use This Document

1. **When activating the Social Media Manager agent:** Check Cloud Brain first — `search_notes` with query `"social media suite preferences"`. If sm-onboard has already been run, preferences are complete and activation is just a confirmation + schedule setup. If not, run sm-onboard now.
2. **Do not run a separate preferences interview** — sm-onboard conducts it. system-04's role here is to confirm preferences, set up scheduled tasks, and update agent status.
3. **Use the Recommended Schedules table** to present the weekly and monthly task schedule. Confirm days/times with the client.
4. **For Option B (two-agent):** Activate Content Creator first (skill subset sm-trend-watcher, sm-content-creator, sm-visual-creator, sm-competitor-watch, sm-repurpose). Then activate Social Media Manager (remaining skills). Both agents read from and write to the same Cloud Brain paths.
5. **Write all preferences to:**
   ```
   brain/preferences/social-media-manager-preferences.md
   ```
6. **After activation, confirm the trust level** — whether the client wants full approval mode (all posts reviewed) or if any content types are already set to auto-schedule.
