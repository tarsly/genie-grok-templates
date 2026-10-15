---
name: sm-tool-setup
description: >-
  Social media tool setup and connection — connects Metricool MCP, inventories existing tools (CapCut, Descript, Canva, Buffer, etc.), provides transparent cost breakdowns, and gives clear content upload instructions. Use when: 'connect my tools', 'set up Metricool', 'tool setup', 'what tools do I need', 'how do I connect my scheduling tool', or as part of sm-onboard.
---

# SM Tool Setup — Tool Inventory, Connections & Cost Transparency

## Overview

This skill inventories the client's existing social media tools, connects the recommended scheduling tool (Metricool) to their AI agent, checks for other integrations, and provides honest cost information so the client can make informed decisions. It also provides clear instructions for uploading their own video and image content for the agent to use.

---

## Pre-Flight — Check for Existing Tool Inventory

Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `"social media tools"`.

**If found:** Display the saved tool inventory and ask: *"Here are the tools I have on file for you — anything to update?"*

**If not found:** Proceed with the full tool inventory below.

---

## Step 1 — Metricool (Scheduling & Analytics)

Metricool is the recommended scheduling tool because it has a native MCP connection — meaning your AI agent can schedule posts, read analytics, and monitor competitors directly without you opening a dashboard.

**Present the cost options clearly:**

| Plan | Cost | What's included | Best for |
|---|---|---|---|
| Free | $0/month | 20 scheduled posts/mo, 3 months of analytics history, 1 brand | Testing, very light use |
| Starter | ~$22/month | Unlimited posts, 12 months history, 1 brand | Most solo entrepreneurs |
| Advanced | ~$59/month | Multiple brands, advanced analytics, team members | Small teams, multi-brand |

*Note: Prices reflect Metricool's published pricing as of mid-2026. Verify current pricing at metricool.com/pricing before subscribing.*

Ask: *"Do you already have a Metricool account?"*

**If yes:** Proceed to connection setup.

**If no:** *"The free plan works to get started. You can create an account at metricool.com — it takes about 2 minutes. Once you have an account, come back and I'll walk you through connecting it."*

Wait for them to confirm they have an account, then proceed.

### Connecting Metricool to Your Agent

Walk the client through these steps:

1. Open your Claude settings (Claude Desktop → Settings → Connections or MCP)
2. Add a new MCP connection
3. Paste this URL: **`https://ai.metricool.com/mcp`**
4. You'll be redirected to Metricool to log in and authorize the connection
5. Once authorized, return to Claude and confirm the connection is active

*"Once connected, your agent can schedule posts across all your connected platforms, check when your audience is most active, review what your competitors are posting, and pull your analytics — all without you opening Metricool."*

**Platforms Metricool supports:** Instagram, TikTok, LinkedIn, Facebook, YouTube, Pinterest, Threads, Bluesky, X (Twitter), plus Meta Ads, Google Ads, TikTok Ads.

Ask the client to confirm once the Metricool MCP is connected before moving on.

---

## Step 2 — Tool Inventory

Ask each question in sequence. Record answers to save to Cloud Brain.

### Graphic Design

*"Do you create graphics or visual content for your posts?"*

Options to mention:
- **Canva** — Free tier available; Canva Pro ~$15/month (brand kit, background remover, premium templates). **Already connected to your agent** — can create platform-sized graphics automatically.
- **Adobe Express** — Free tier; Premium ~$9.99/month. No direct agent connection; produces exported files.
- **Figma** — Free personal; Professional $12/editor/month. Design tool, not optimized for social posts.
- **Other / None** — Record what they use.

If they use Canva: *"Canva is already connected to your agent. When you run sm-visual-creator, your agent can create platform-optimized graphics automatically — no manual export needed."*

If they use something else: *"Your agent will create detailed visual briefs specifying the design, dimensions, and style. You'll use [their tool] to create the graphic, then upload it as described in the content upload instructions below."*

### Video Creation & Editing

*"Do you create video content — Reels, TikToks, YouTube videos, or Shorts?"*

Options to mention with costs:

| Tool | Cost | Best for | Agent integration |
|---|---|---|---|
| **CapCut** | Free; Pro ~$7.99/mo | TikTok/Reels, trending effects | Script + brief → you create → upload to Metricool |
| **Descript** | Free (1hr/mo); Creator $12/mo; Pro $24/mo | Podcast clips, screen recordings, AI voice editing | Transcript → repurpose via sm-repurpose |
| **iMovie** | Free (Mac) | Basic editing | Script + brief → you create → upload to Metricool |
| **Adobe Premiere** | $54.99/mo (Creative Cloud) | Professional production | Script + brief → you create → upload to Metricool |
| **Riverside.fm** | Free (720p); Standard $15/mo | Podcast/interview recording | Transcript → repurpose via sm-repurpose |

Record which tool(s) they use. Note: if they use Descript, the sm-repurpose skill can pull transcripts to generate multi-platform posts automatically.

### Audio / Podcasting

*"Do you record a podcast or audio content?"*

If yes: ask which platform/tool (Descript, Riverside, Buzzsprout, Anchor, Spotify for Podcasters). Record it — sm-repurpose can turn episode summaries into cross-platform posts.

### Content Storage

*"Where do you currently store your content assets — finished videos, graphics, photos?"*

Options:
- **Google Drive** — agent can read from connected Drive folders if Google Drive MCP is connected
- **Dropbox** — no direct agent connection; manual upload workflow
- **Local computer** — manual upload workflow
- **Other** — record

If they use Google Drive: *"If you connect Google Drive to your agent (via the Google Drive MCP in Claude settings), you can drop finished assets in a shared folder and your agent can pull them directly. This is the most hands-off option for video content."*

### Existing Scheduling Tool

*"Are you currently using another scheduling tool — Buffer, Later, Hootsuite, SocialPilot, or anything else?"*

| Tool | Cost | Notes |
|---|---|---|
| Buffer | Free (3 channels); $6/channel/mo | No direct agent MCP — manual workflow |
| Later | Free (limited); $16.67/mo | No direct agent MCP |
| Hootsuite | $99/mo+ | API available; enterprise-focused |
| SocialPilot | $30/mo+ | API available; good for multi-account |

If they're on another tool: *"Your agent can still draft content for any platform, but the automatic scheduling feature works only with Metricool (which has a native connection). You have two options: (1) migrate your accounts to Metricool — most clients find this takes about 15 minutes and the free tier covers typical solo use, or (2) continue using [their tool] and have your agent draft posts for you to manually schedule."*

Record their decision.

---

## Step 3 — Content Upload Instructions

Deliver these instructions and save them to Cloud Brain so the agent can reference them whenever the client needs to upload content.

### How to Get Your Own Videos and Images to Your Agent

Your agent can write scripts, create post captions, and schedule everything — but for video content you've filmed yourself, here's the workflow:

**For Videos (Reels, TikToks, YouTube Shorts):**

1. Film and edit your video in [their tool — CapCut, iMovie, etc.]
2. Export settings by platform:
   - TikTok / Instagram Reels / Facebook Reels: MP4, vertical (1080×1920), under 60 seconds for Reels
   - YouTube Shorts: MP4, vertical (1080×1920), under 60 seconds
   - YouTube long-form: MP4, landscape (1920×1080), any length
3. Upload the finished video directly to Metricool:
   - Log in to metricool.com → Planner → New Post → attach your video
   - Your agent can then access it via the Metricool connection
4. Or, if you have Google Drive connected: drop the video in your designated social media folder and tell your agent: *"I have a new video ready — [filename or topic]"*
5. Your agent will match the video to the correct scheduled post and confirm

**For Photos and Images:**

1. Export from [their tool — Canva, Adobe, etc.] at the correct size:
   - Instagram feed: 1080×1080 (square) or 1080×1350 (portrait)
   - Instagram/Facebook Story: 1080×1920
   - LinkedIn: 1200×627
   - Facebook post: 1200×630
   - YouTube thumbnail: 1280×720
2. Upload directly to Metricool when scheduling, or share via Google Drive if connected
3. Or ask your agent to create the graphic in Canva — this is fully automatic if Canva is connected

**The fastest workflow:** Let your agent create Canva graphics for most posts. Film video yourself → export → upload to Metricool → tell your agent it's ready.

---

## Step 4 — Save Tool Inventory to Cloud Brain

Save everything collected to Cloud Brain:

```
mcp__cloud-brain__write_note
path: brain/preferences/social-media-manager-preferences.md
```

Include:
- Metricool connection status (connected / pending / not using)
- Graphic design tool(s) and plan tier
- Video creation tool(s)
- Audio/podcast tool(s)
- Content storage location
- Existing scheduling tool (if any) and migration decision
- Content upload workflow preference

---

## Output Summary

Present a clean tool inventory summary:

---

**Your Tool Stack:**

| Category | Tool | Connection | Cost |
|---|---|---|---|
| Scheduling & Analytics | Metricool | [status] | [plan] |
| Graphics | [tool] | [status] | [plan] |
| Video | [tool] | Manual upload | [plan] |
| Storage | [tool] | [status] | — |

**Content Upload Workflow:** [summarize their specific workflow]

**Recommended upgrade to consider:** [only if there's a clear gap — e.g., "Metricool paid plan for unlimited scheduling" or "Google Drive connection for hands-off video uploads"]

---

*Costs listed are estimates. Always verify current pricing at the tool's website before subscribing.*
