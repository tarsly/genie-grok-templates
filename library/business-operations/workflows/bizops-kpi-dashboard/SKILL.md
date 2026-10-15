---
name: bizops-kpi-dashboard
description: >-
  KPI tracking — define, update, and report on key performance indicators. Business scorecard, revenue tracking, conversion rates, deal metrics, content output, trend analysis, target setting, or needs to define a KPI, log a new number, or pull a trend report across revenue, pipeline, or content metrics.
---

# KPI Dashboard

## Overview

Define, track, and report on Key Performance Indicators across all your businesses and revenue streams. You set up your KPIs once — revenue targets, deal counts, content output, lead conversion rates, customer metrics, whatever matters — and then `/kpi-dashboard` pulls the current status, shows trend arrows (up/down/flat), highlights what is on track vs. off track, and delivers specific recommendations for any metric that is underperforming. This is how a $180K/year executive assistant keeps you informed: you never have to ask "how are we doing?" — you already know.

## When This Skill Applies

- User wants to set up KPIs for the first time
- User asks "how are my numbers?" or "check my KPIs"
- User wants a business performance dashboard
- User says "update my KPIs" with new data
- User asks if they are on track for their goals
- User wants to define new metrics to track
- User asks for a weekly or monthly metrics report
- User wants to know which areas of the business are underperforming
- User says "scorecard" or "business health check"
- User wants to change their targets or add a new KPI
- User asks "what should I be tracking?"


## Pre-Flight — KPI Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops kpi preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Business name and industry
   - Primary KPI categories to track (revenue / pipeline / operations / marketing / financial / custom)
   - Reporting cadence (weekly / monthly / quarterly)
   - Fiscal period start month
   - Current annual revenue target (used for benchmarking)
   - Save to Cloud Brain: `write_note` → title: `bizops-kpi-preferences`, folder: `brain/preferences`
4. Apply throughout: filter dashboard to saved KPI categories, use reporting cadence for all trend windows
5. Show banner at top of every output:
   ```
   📈 KPI Dashboard | {Business Name} | {Reporting Cadence} | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my KPI preferences' to change settings."*

## How It Works

### Step 0: Determine the Action

Parse the user's request into one of three modes:

- **`setup`** — First-time KPI definition or adding new KPIs
- **`update`** — Logging new data points for existing KPIs
- **`report`** — Generating the current dashboard (default if no action specified)

If no arguments are provided, default to `report`. Use `search_notes` with query "KPI dashboard" to check if a KPI note exists in Cloud Brain. If not found, automatically switch to `setup`.

### Step 1: Setup — Define KPIs (first run or adding new ones)

If this is the first time, guide the user through KPI definition. Start by using `search_notes` with query "goals" to find goals in Cloud Brain and understand their objectives, then suggest KPIs aligned to those goals.

**Default KPI categories (propose these, let user customize):**

| Category | Example KPIs |
|----------|-------------|
| Revenue | Monthly revenue, MRR/ARR, average deal size, revenue per client |
| Pipeline | Total leads, qualified leads, deals in negotiation, close rate |
| Sales Activity | Outreach calls/emails per week, proposals sent, follow-ups completed |
| Content & Marketing | Posts per week, followers gained, engagement rate, email list size, CTR |
| Client/Customer | Active clients, churn rate, NPS/satisfaction, referrals received |
| Operations | Tasks completed per week, SOP compliance, response time |
| Financial Health | Cash runway, profit margin, accounts receivable, burn rate |
| Personal/Growth | Hours worked, gym sessions, learning hours, networking events |

For each KPI the user wants to track, capture:

| Field | Description | Example |
|-------|-------------|---------|
| Name | What the metric is called | "Monthly Revenue" |
| Category | Which category it falls under | "Revenue" |
| Current Value | Where the metric stands today | "$12,500" |
| Target | What they are aiming for | "$25,000" |
| Frequency | How often it gets updated | Weekly / Monthly / Daily |
| Source | Where the data comes from | "Stripe dashboard", "manual count", "analytics" |
| Priority | How critical this KPI is | High / Medium / Low |

Save the KPI definitions to Cloud Brain using `write_note` with:
- **title:** `KPI Dashboard`
- **folder:** `pipeline`
- **tags:** `["kpi", "dashboard", "metrics"]`

Using this format:

```markdown
# KPI Dashboard

> **Last Updated:** {YYYY-MM-DD}
> **Review Cadence:** {Weekly / Bi-weekly / Monthly}
> **Total KPIs Tracked:** {count}

---

## Active KPIs

### Revenue

| KPI | Target | Current | Trend | Status | Last Updated |
|-----|--------|---------|-------|--------|-------------|
| Monthly Revenue | $25,000 | $12,500 | -- | Off Track | {date} |
| Average Deal Size | $5,000 | $4,200 | -- | Approaching | {date} |

### Pipeline

| KPI | Target | Current | Trend | Status | Last Updated |
|-----|--------|---------|-------|--------|-------------|
| Active Leads | 50 | 23 | -- | Off Track | {date} |
| Close Rate | 25% | 18% | -- | Off Track | {date} |

### {Category}...

---

## History Log

| Date | KPI | Previous | New Value | Change | Notes |
|------|-----|----------|-----------|--------|-------|
| {date} | {name} | {old} | {new} | {+/-/%} | {context} |
```

### Step 2: Update — Log New Data

When the user provides new numbers:

1. Use `search_notes` with query "KPI dashboard" to find the KPI note, then `read_note` to get the content
2. Parse which KPIs the user is updating (match by name, fuzzy if needed)
3. For each KPI being updated:
   - Record the previous value
   - Set the new current value
   - Calculate the change (absolute and percentage)
   - Determine the trend arrow:
     - **up arrow** (moving toward target and improving)
     - **down arrow** (moving away from target or declining)
     - **right arrow** (flat, less than 2% change)
   - Update the status:
     - **On Track** — current value is at or above target, or trending to hit target by deadline
     - **Approaching** — within 80% of target and trending positively
     - **Off Track** — below 60% of target or trending negatively
     - **Critical** — below 40% of target or declining for 3+ consecutive periods
     - **Exceeded** — above target
4. Append each change to the History Log with timestamp
5. Update `> **Last Updated:**` to today
6. Save using `write_note` with title `KPI Dashboard`, folder `pipeline`, tags `["kpi", "dashboard", "metrics"]`
7. Report what changed and flag any KPIs that flipped status (e.g., went from Approaching to Off Track)

**Accepting bulk updates:**

The user can say things like:
- "Revenue hit $18K this month, closed 4 deals, posted 12 times this week"
- Parse all numbers and match them to the closest KPIs

### Step 3: Report — Generate the Dashboard

1. Use `search_notes` with query "KPI dashboard" to find and `read_note` to load the KPI data
2. Use `search_notes` with query "goals" to find goals for context on what matters most right now
3. Use `recent_activity` to check if there is a daily log for today

Generate the dashboard in this format:

```markdown
# KPI Dashboard — {Date}

> **Period:** {Week of / Month of} {date range}
> **Overall Health:** {STRONG / SOLID / MIXED / CRITICAL}
> **KPIs On Track:** {X} of {Y} ({Z}%)

---

## Executive Summary

{2-3 sentences: What is going well, what needs attention, and the single most important thing to focus on this week to move the needle.}

---

## Scorecard

### Revenue & Financial
| KPI | Target | Current | Trend | Status | Gap |
|-----|--------|---------|-------|--------|-----|
| {name} | {target} | {value} | {arrow} | {status} | {how far from target} |

### Pipeline & Sales
| KPI | Target | Current | Trend | Status | Gap |
|-----|--------|---------|-------|--------|-----|

### Content & Marketing
| KPI | Target | Current | Trend | Status | Gap |
|-----|--------|---------|-------|--------|-----|

### Operations & Growth
| KPI | Target | Current | Trend | Status | Gap |
|-----|--------|---------|-------|--------|-----|

---

## Highlights

### What is Working (On Track or Exceeded)
- {KPI}: {value} vs {target} — {why it is working, what to keep doing}

### What Needs Attention (Off Track or Critical)
- {KPI}: {value} vs {target} — {specific diagnosis of why and what to do about it}

---

## Recommendations

1. **{Highest priority recommendation}** — {specific action, tied to the most impactful off-track KPI}
2. **{Second recommendation}** — {specific action}
3. **{Third recommendation}** — {specific action}

---

## Week-over-Week Trend (last 4 periods)

| KPI | 4 wks ago | 3 wks ago | 2 wks ago | Last wk | This wk | Direction |
|-----|-----------|-----------|-----------|---------|---------|-----------|
| {name} | {val} | {val} | {val} | {val} | {val} | {arrow} |

---

## Goal Alignment Check

| Goal (from goals) | Related KPIs | Combined Status | Risk |
|-------------------|-------------|-----------------|------|
| {quarterly goal} | {KPI names} | {On Track / At Risk / Off Track} | {notes} |

---

*Dashboard generated {date}. Next recommended update: {date based on cadence}.*
```

4. Display the full dashboard in the terminal
5. Append a summary to today's daily log using `write_note` with title `Daily Log {YYYY-MM-DD}`, folder `daily`, tags `["daily", "log"]`
6. If the daily log does not exist, create it with the KPI summary as the first entry

### Step 4: Intelligent Recommendations

For every Off Track or Critical KPI, generate a specific recommendation. Not generic advice — specific to the user's numbers and context.

**Recommendation framework:**

1. **Diagnose:** Why is this metric underperforming? (Look at related KPIs, recent history, patterns)
2. **Prescribe:** What specific action would move this number? (One action, not five)
3. **Quantify:** If they do X, the expected impact is Y
4. **Timeline:** When should they see improvement if they start now?

Example:
- "Monthly revenue is at $12,500 vs. target of $25,000 (50%). Your close rate dropped from 22% to 18% while lead volume stayed flat. The fastest lever is follow-up speed — your pipeline shows 8 leads in 'contacted' stage with no activity in 7+ days. Work those 8 leads this week. If you close 2, that adds approximately $8,400 at your current average deal size."

## Output Format

The dashboard is displayed directly in the terminal. A condensed version is also saved to the daily log in Cloud Brain.

**Terminal output:** Full dashboard as shown in Step 3 above.

**Daily log entry:**

```markdown
## KPI Check — {time}

**Overall:** {STRONG/SOLID/MIXED/CRITICAL} — {X}/{Y} KPIs on track
**Top Win:** {best performing KPI and value}
**Top Risk:** {worst performing KPI and gap}
**Action:** {#1 recommendation}
```

## Example Usage

**User:** "Set up my KPIs"

**AI:** Searches Cloud Brain for goals, proposes KPIs aligned to their quarterly objectives. Asks the user to confirm, modify, or add. Saves the initial KPI definitions to Cloud Brain as `KPI Dashboard` in the `pipeline` folder with current values and targets.

**User:** "Update my numbers — revenue was $18K this month, I closed 6 deals, and I posted 15 times on social media"

**AI:** Matches "revenue" to Monthly Revenue, "6 deals" to Deals Closed, "15 posts" to Content Output. Updates each, calculates trends and status changes, saves to Cloud Brain with history log, and reports what moved.

**User:** "/kpi-dashboard"

**AI:** Generates the full dashboard, shows trend arrows, highlights 2 off-track KPIs with specific recommendations, notes goal alignment, and appends summary to today's daily log in Cloud Brain.

**User:** "Am I on track for Q1?"

**AI:** Cross-references KPI actuals against goals from Cloud Brain. Shows which goals are supported by on-track KPIs and which are at risk based on current trajectory. Provides a projected end-of-quarter estimate for each goal.

**User:** "Add a new KPI for email list subscribers"

**AI:** Adds the KPI to the Content & Marketing category in the KPI Dashboard note in Cloud Brain, asks for current value and target, sets it up with the appropriate tracking cadence.

## Error Handling

- **If the KPI Dashboard note does not exist in Cloud Brain:** Automatically switch to setup mode. Tell the user: "No KPIs defined yet. Let me help you set up your dashboard. I'll start by searching your brain for goals to suggest relevant metrics."
- **If the user provides numbers but no KPIs match:** List the defined KPIs and ask: "I couldn't match '{input}' to an existing KPI. Here are your current KPIs: {list}. Which one should I update? Or should I create a new KPI for this?"
- **If no goals note exists in Cloud Brain during setup:** Proceed without goal alignment. Note: "No goals found in your brain. I'm setting up KPIs based on your input only. Add goals to enable automatic goal-to-KPI alignment."
- **If the user asks for a trend report but there is only one data point:** Show the current values without trends. Note: "This is your first data point — trends will appear after your next update. I recommend updating {cadence} to build a useful trend line."
- **If KPI data is stale (last updated more than 2x the stated cadence):** Flag it prominently: "WARNING: {X} KPIs haven't been updated in {Y} days. Stale data means blind spots. Here's what needs refreshing: {list}."
- **If a KPI has been Critical for 3+ consecutive updates:** Escalate: "ESCALATION: {KPI name} has been in Critical status for {X} consecutive periods. This is no longer a dip — it is a pattern. Recommend: {specific structural change, not just 'try harder'}."
