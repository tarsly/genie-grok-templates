# Social Media Suite

**Version 1.0.0 · By the provider**

A complete AI-powered social media management system for solo entrepreneurs and small teams. From strategy and brand setup through weekly content creation, scheduling, analytics, and competitor monitoring — everything your Social Media Manager agent needs to run your social presence autonomously.

---

## What This Plugin Does

Your Social Media Manager agent handles your complete weekly social media workflow:

- **Learns your business** — goals, audience, platforms, brand voice, content pillars
- **Connects your tools** — Metricool for scheduling, Canva for graphics, plus any tools you already use
- **Plans your content** — monthly calendar tied to your strategy and business goals
- **Writes your posts** — platform-optimized captions for Instagram, TikTok, LinkedIn, Facebook, and YouTube
- **Creates your graphics** — Canva designs at the right size for every platform
- **Schedules everything** — posts directly to Metricool with your approval (working toward full autonomy)
- **Tracks performance** — monthly analytics review with clear recommendations
- **Watches your industry** — weekly trend research and competitor monitoring

---

## Getting Started — Run This First

**Say:** *"Get started with Social Media Suite"* or *"Run sm-onboard"*

`sm-onboard` guides you through a one-time setup (~30–45 minutes):
1. Tool connections (Metricool + inventory your existing tools)
2. Discovery interview (business, platforms, goals, audience)
3. Brand snapshot (voice, pillars, visual style)
4. Strategy build (platform priority, posting frequency, optimal timing)

After setup, your agent runs weekly automatically.

---

## Skills

| Skill | When It Runs | What It Does |
|---|---|---|
| `sm-onboard` | **Once** — start here | Complete setup sequence |
| `sm-tool-setup` | Once / on-demand | Connect Metricool, inventory tools, cost breakdown |
| `sm-discovery` | Once / when business changes | Business, goals, audience, platform interview |
| `sm-brand-snapshot` | Once / when brand evolves | Brand voice, content pillars, visual identity |
| `sm-strategy-builder` | Once / quarterly | Platform strategy, posting cadence, agent config |
| `sm-content-calendar` | **Monthly** | Plan next month's content themes and schedule |
| `sm-analytics-review` | **Monthly** | Performance review, top posts, recommendations |
| `sm-trend-watcher` | **Weekly** (Monday) | Research industry trends and content ideas |
| `sm-content-creator` | **Weekly** (Tuesday) | Write this week's posts for all platforms |
| `sm-visual-creator` | **Weekly** (Wednesday) | Create Canva graphics for each post |
| `sm-schedule-queue` | **Weekly** (Thursday) | Approve posts and schedule via Metricool |
| `sm-competitor-watch` | Weekly / on-demand | Monitor what competitors are posting |
| `sm-repurpose` | **On-demand** | Turn blog posts, podcasts, or videos into social posts |

---

## Trigger Phrases

| Say this... | To do this... |
|---|---|
| "Get started" / "Set up my social media" | Run full onboarding |
| "Connect my tools" / "Set up Metricool" | Tool setup and connections |
| "Plan next month's content" | Generate content calendar |
| "Write this week's posts" | Create weekly content drafts |
| "Create my graphics" / "Make the visuals" | Generate Canva graphics |
| "Schedule my posts" / "Approve and schedule" | Review and publish to Metricool |
| "Review my analytics" / "How did I do this month?" | Monthly performance report |
| "What's trending this week?" | Weekly trend research |
| "What are my competitors posting?" | Competitor monitoring |
| "Repurpose this blog post/podcast/video" | Content repurposing |
| "Update my strategy" / "Rebuild my social media plan" | Refresh strategy |

---

## Tool Requirements

### Metricool (Required for scheduling)
Your agent schedules posts through Metricool's native MCP connection.

**Connection URL:** `https://ai.metricool.com/mcp`

**How to connect:** Claude Settings → Connections → Add MCP → paste the URL → authorize with your Metricool account.

| Plan | Cost | Best for |
|---|---|---|
| Free | $0/month | Getting started (20 posts/month limit) |
| Starter | ~$22/month | Most solo entrepreneurs |
| Advanced | ~$59/month | Multiple brands or team access |

*Verify current pricing at metricool.com/pricing.*

**Platforms supported:** Instagram, TikTok, LinkedIn, Facebook, YouTube, Pinterest, Threads, Bluesky, X (Twitter), plus Meta Ads, Google Ads, TikTok Ads.

### Canva (Required for automated graphics)
Already connected to your agent. Canva creates platform-sized graphics automatically when you run `sm-visual-creator`.

| Plan | Cost | Notes |
|---|---|---|
| Free | $0 | Good starting point |
| Pro | ~$15/month | Brand kit, background remover, premium templates |

*If Canva is not connected, sm-visual-creator provides detailed manual design briefs.*

### Other Tools (Optional)
During setup, your agent will ask about tools you already use and find the best integration for each:

- **CapCut** — video editing for TikTok/Reels
- **Descript** — podcast/video editing with transcription (enables automatic repurposing)
- **Google Drive** — asset storage with optional direct agent connection
- **Buffer / Later / SocialPilot** — if migrating from another scheduler

---

## How to Upload Your Own Videos and Images

Your agent creates graphics automatically via Canva. For videos you film yourself:

**For videos (Reels, TikToks, Shorts):**
1. Edit in your video tool (CapCut, Descript, iMovie, etc.)
2. Export as MP4 at correct size:
   - TikTok / Instagram Reels / Facebook Reels: 1080×1920 (vertical)
   - YouTube Shorts: 1080×1920 (vertical)
   - YouTube long-form: 1920×1080 (landscape)
3. Upload to Metricool directly (Planner → New Post → attach video)
4. Tell your agent: *"Video for [topic] is uploaded and ready"*

**For photos you took yourself:**
1. Upload directly to Metricool, or drop in your Google Drive social media folder (if connected)
2. Tell your agent: *"I have photos ready for [post topic]"*

**For images you made in Canva manually:**
Your agent can also create them automatically — just run `sm-visual-creator`.

---

## The Path to Full Autonomy

Your agent starts supervised and earns trust over time:

**Phase 1 — Supervised:** Every post goes through approval in sm-schedule-queue before scheduling. (This is where everyone starts.)

**Phase 2 — Selective trust:** After 5+ consecutive approvals of a content type with no edits, your agent asks: *"Would you like me to auto-schedule [content type] going forward?"* You approve autonomy one content type at a time.

**Phase 3 — Full autonomy:** All content types are trusted. Your agent runs the full weekly workflow and sends you a summary instead of an approval queue.

You stay in control of this timeline — you decide when each content type is ready for auto-scheduling.

---

## Setting Up Your Social Media Manager Agent

After running `sm-onboard`, activate your agent:

1. Run **system-03 (Agent Designer)** — this plugin's `AGENTS.md` will automatically suggest the right agent configuration for your situation
2. Run **system-04 (Agent Activator)** — sets up your preferences and schedules the weekly workflow
3. Run **system-05 (Agent Reporter)** — configures your weekly summary report

For most solo entrepreneurs: one Social Media Manager agent handles everything.
For YouTube-heavy clients: a Content Creator agent + Social Media Manager agent is recommended.

---

## Version History

### 1.0.0 — 2026-06-12
Initial release. 13 skills covering full social media management lifecycle: onboarding, tool setup, discovery, brand capture, strategy, content calendar, content creation, Canva graphics, approval/scheduling via Metricool, analytics, trend research, competitor monitoring, and content repurposing. Includes trust-building path to full agent autonomy.


## Grok Bot import
See the root README. Replace `{{BOT_NAME}}` in `system-prompt.md` and `workflows/getting-started/SKILL.md`.
