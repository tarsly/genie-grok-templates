---
name: sm-schedule-queue
description: >-
  Presents the week's posts for client approval, then schedules approved posts via the Metricool MCP. Tracks approval patterns over time and surfaces when specific content types are ready for auto-scheduling. Use when: 'schedule my posts', 'approve and schedule', 'review this week's posts', 'schedule queue', 'publish my content', 'schedule posts in Metricool'.
---

# SM Schedule Queue — Approval, Scheduling & Trust Tracker

## Overview

This skill is the final step in the weekly workflow. It loads the week's approved content drafts (written by sm-content-creator and visualized by sm-visual-creator), presents them to the client for review, schedules approved posts via Metricool, and tracks approval patterns over time. As trust builds, it surfaces opportunities to remove the approval gate for specific content types — moving toward full autonomy.

**Metricool MCP required for scheduling.** If not connected, this skill presents the drafts formatted for manual copy-paste into any scheduling tool.

---

## Pre-Flight — Load Drafts & Strategy

1. `mcp__cloud-brain__search_notes` with query `"drafts"` — load current week's content drafts
2. `mcp__cloud-brain__search_notes` with query `"social media strategy"` — load posting schedule and optimal times
3. `mcp__cloud-brain__search_notes` with query `"approval history"` — load past approval patterns

**If no drafts found:** *"I don't have any content drafts for this week. Run sm-content-creator first, then come back."*

---

## Job Inputs (Ask Fresh Each Run)

Ask:
- **Which week are we scheduling?** (e.g., "week of July 7")
- **Are there any posts you know you want to skip or postpone this week?**
- **Any timing changes from the standard schedule?** (e.g., "push Wednesday's post to Thursday")

---

## Step 1 — Build the Approval Queue

Load all PENDING posts from the week's drafts. Present them in a clear, scannable format:

---

**📋 Approval Queue — Week of [date]**
*Review each post below. Reply with: APPROVE ALL / [number] to approve specific posts / EDIT [number]: [your changes]*

---

**Post 1 of [n]** · Instagram · [Pillar] · [date, time]

**Caption:**
[full caption]

**Visual:** [Canva link or "visual brief provided"]

**Hashtags:** [hashtags]

---

**Post 2 of [n]** · LinkedIn · [Pillar] · [date, time]

[full post content]

---

*(Continue for all posts)*

---

**Total this week:** [n] posts across [platforms]

*Reply with: APPROVE ALL, APPROVE [numbers], or EDIT [number]: [your changes]*

---

## Step 2 — Process Approval

**"APPROVE ALL":** Schedule all posts immediately. Go to Step 3.

**"APPROVE [numbers]" (e.g., "APPROVE 1,3,4"):** Schedule only the approved posts. For declined posts, ask: *"Should I save posts [numbers] as drafts for next week, or discard them?"*

**"EDIT [number]: [changes]":** Apply the client's edits to the specified post. Show the revised version and ask for confirmation. Once confirmed, schedule it.

**Partial approval without instruction:** If the client approves some posts but doesn't say what to do with others, ask before scheduling.

---

## Step 3 — Schedule via Metricool

For each approved post, call the Metricool MCP to schedule:

The Metricool MCP scheduling tool requires:
- Platform / network
- Post content (caption, hashtags)
- Scheduled date and time
- Brand ID (from saved preferences or `get_brands`)
- Media attachment link (if visual was created)

Use the optimal posting times from the client's strategy. If `get_best_time_to_post` returns better data for this specific date, use that instead.

After scheduling each post, confirm: *"✅ [Platform] post scheduled for [date] at [time]."*

If Metricool is not connected:
Present all approved posts formatted for manual scheduling, with copy-paste ready captions, exact recommended times, and hashtags separated from captions for easy pasting.

---

## Step 4 — Update Cloud Brain

Update the content drafts note with final scheduling status:

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-drafts-[YYYY-MM-DD].md
```

Mark each post:
- Approved + scheduled: `SCHEDULED — [platform] — [date/time]`
- Approved + not scheduled: `APPROVED — PENDING MANUAL SCHEDULE`
- Declined: `DECLINED — [reason if given]`
- Edited: `EDITED + SCHEDULED — [date/time]`

---

## Step 5 — Update Approval History

This is the trust-building engine. After every schedule session, update the approval history:

```
mcp__cloud-brain__write_note
path: brain/social-media/[business-name-slug]-approval-history.md
```

For each post record:
- Date
- Platform
- Content pillar
- Content type (graphic, video, text, carousel)
- Approval outcome (approved as-is / approved with edits / declined)
- If edited: what changed (tone, length, factual correction, CTA, other)

---

## Step 6 — Trust Escalation Check

After updating approval history, analyze the patterns:

**Trigger for trust escalation suggestion:** Any content type + pillar combination that has been approved **5 consecutive times** with no edits.

When triggered, surface this message:

---

*🤖 Trust Update: Your [pillar] [content type] posts have been approved without changes 5 weeks in a row. Would you like me to auto-schedule those going forward without showing them to you first?*

*You can still see what was scheduled — I'll include them in your weekly summary instead of the approval queue.*

*Options:*
*1. Yes — auto-schedule [content type] posts from now on*
*2. Not yet — keep showing them to me*
*3. Let's talk about it — [I'll explain what changes when you say yes]*

---

**Auto-scheduling is only enabled for specific content type + pillar combinations, one at a time.** A client can have some posts auto-schedule while others still require approval.

When a content type is set to auto-schedule, note it in the approval history file:
`AUTO-APPROVED: [pillar] [content type] — enabled [date]`

**Trigger for full autonomy notification:** When all active content types are set to auto-schedule.

*"🎉 All your content types are now set to auto-schedule. Your Social Media Manager agent is fully autonomous — I'll send you a weekly summary of what was posted instead of an approval queue. You can turn any content type back to manual review anytime."*

---

## Weekly Summary (When All Posts Are Scheduled)

Send a final summary:

---

**✅ Week of [date] — Scheduled & Done**

| Post | Platform | Scheduled For | Status |
|---|---|---|---|
| [topic] | Instagram | Mon Jul 7, 9:15am | ✅ Scheduled |
| [topic] | LinkedIn | Tue Jul 8, 8:30am | ✅ Scheduled |
| [topic] | Facebook | Wed Jul 9, 2:00pm | ✅ Scheduled |
| [topic] | TikTok | Thu Jul 10, 7:30pm | ✅ Scheduled |
| [topic] | Instagram | Fri Jul 11, 10:00am | ✅ Scheduled |

**Total scheduled:** [n] posts
**Platforms:** [list]
**Next step:** Run sm-analytics-review on the 1st to see how last month performed.
