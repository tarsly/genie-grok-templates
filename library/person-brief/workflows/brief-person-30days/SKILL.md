---
name: brief-person-30days
description: >-
  Synthesize the last 30 days of public activity on a person across Reddit, X/Twitter, Hacker News, YouTube, GitHub, and the open web. Produces a one-page brief covering what they posted, what they engaged with, recurring themes, and notable mentions. Use before sales calls, networking meetings, podcast guests, or any conversation where being current matters.
---

# Person Brief — Last 30 Days

## Overview

A cross-platform research skill that produces a current, signal-dense brief on a single person. Unlike static "research-person" reports, this skill focuses on **what they've been doing recently** — posts, comments, code, talks, mentions. Output is a one-page Markdown brief written to Cloud Brain under `research/people/`, optionally cross-linked to a contact note in `people/`.

This skill is the building block for **brief-pre-meeting** and **brief-pre-call**, which orchestrate it inside larger pre-conversation workflows.

## When This Skill Applies

- User asks to research a person ("what's [name] been up to lately?", "give me a brief on [name]")
- User has a meeting / call / podcast / interview coming up with a specific person
- User mentions a person by name and the context implies they want recent context, not a biography
- User says: "brief me on", "pre-meeting research", "current intel", "last 30 days on", "what is [name] talking about"
- User is preparing for outbound (cold email, LinkedIn DM, pitch) to a named individual

## Pre-Flight — Preferences

1. Search Cloud Brain: `search_notes` with query `"person brief preferences"` (folder: `brain/preferences`)
2. **If found:** Load preferences, render banner, proceed
3. **If not found:** Ask in ONE message:
   - Default research depth (quick = 5 min / standard = 10 min / deep = 20+ min)
   - Default sources priority (rank: X, Reddit, HN, YouTube, GitHub, podcasts, news, LinkedIn)
   - Save briefs to Cloud Brain automatically? (yes / ask each time)
   - Default time window (7 / 14 / 30 / 90 days)
   - Save to Cloud Brain: `write_note` → title: `person-brief-preferences`, folder: `brain/preferences`
4. Render banner at top of every output:
   ```
   🎯 Person Brief | Depth: {depth} | Window: last {N} days | Auto-save: {on/off}
   ```
   Add: *"Say 'update my brief preferences' to change settings."*

## How It Works

### Step 1: Identify the Person Uniquely

Common names cause false positives. Before searching, gather identifiers:

- Full name
- Optional: company / role
- Optional: LinkedIn URL, GitHub username, X handle
- If only a name is given, ask once: *"To narrow this down — what company are they at, or do you have their LinkedIn / X / GitHub?"*
- Skip the ask if user provided enough context (e.g., a meeting invite with an email domain).

Check Cloud Brain first via `search_notes` for an existing contact note in `people/`. If found, pull known handles and use them.

### Step 2: Parallel Source Sweep

Run searches across the configured sources. Default window: last 30 days. Suggested queries:

| Source | Query pattern |
|--------|--------------|
| X / Twitter | `site:x.com "{name}" OR @{handle}` (newest first) |
| Reddit | `site:reddit.com "{name}"` plus self-posts from u/{handle} if known |
| Hacker News | `site:news.ycombinator.com "{name}"` |
| YouTube | `"{name}" {company}` filter: last month |
| GitHub | API: `users/{username}/events/public` (PRs, issues, repos created) |
| Podcasts | `"{name}" podcast` site:podcasts.apple.com OR site:spotify.com OR site:rss.com |
| News | `"{name}" {company}` filter: past month |
| Personal site / blog | If LinkedIn or X has a link, WebFetch latest posts |

For each source, capture: post date, link, headline/summary, engagement signal (upvotes, likes, replies, view count).

### Step 3: Synthesize

Reduce raw hits into the brief. Drop anything older than the time window. De-duplicate cross-postings. Group by theme.

For each theme:
- What they're saying / building
- Who they're engaging with (mentions, replies, collabs)
- Evidence (3-5 links max with date + 1-line excerpt)

Add a "What This Means" section: 1-3 bullets connecting their activity to the user's context (meeting topic, pitch, role).

### Step 4: Write the Brief

Use `write_note` to save the brief.

- **title:** `Brief — {Full Name} — {YYYY-MM-DD}`
- **folder:** `research/people`
- **tags:** `["person-brief", "{last-name}", "{company-or-domain}"]`

Use the template in the Data Structure section.

### Step 5: Cross-Link to Contact Note (Optional)

If a contact note exists in `people/` for this person, read it, append a line under a "Recent Briefs" section:

```markdown
## Recent Briefs

- [2026-06-20] Brief — {Name} — 2026-06-20 (last 30 days)
```

If no contact note exists, offer: *"Want me to create a contact note for {name} in your people folder?"* — don't auto-create.

## Data Structure

### Brief Template

```markdown
# Brief — {Full Name}

> **Generated:** {YYYY-MM-DD}
> **Window:** Last {N} days
> **Depth:** {quick / standard / deep}
> **Confidence:** {high / medium / low — based on signal volume}

## Identity Snapshot

| Field | Value |
|-------|-------|
| Name | {full name} |
| Role / Company | {title at company} |
| Location | {city / region if known} |
| LinkedIn | {url or "—"} |
| X / Twitter | {handle or "—"} |
| GitHub | {handle or "—"} |
| Personal Site | {url or "—"} |

## What They've Been Doing

### Theme 1: {short label}
- {1-sentence summary}
- Evidence:
  - [{YYYY-MM-DD}] {source} — "{excerpt}" → {url}
  - [{YYYY-MM-DD}] {source} — "{excerpt}" → {url}

### Theme 2: {short label}
...

## Who They're Engaging With

- {Name / handle} — {context, e.g., "replied to 3 posts about [topic]"}
- {Name / handle} — {context}

## Signal Strength

| Source | Activity | Notes |
|--------|----------|-------|
| X | {N posts, M likes} | {trend: rising / steady / quiet} |
| Reddit | {N comments / posts} | {top sub / topic} |
| GitHub | {N PRs, M repos} | {primary repo / language} |
| YouTube | {N appearances} | {channel / topic} |
| HN | {N submissions / comments} | {threads of note} |
| Podcasts | {N appearances} | {show names} |
| News | {N mentions} | {publication / topic} |

## What This Means for {context}

- {bullet — connect activity to user's reason for the brief}
- {bullet}
- {bullet — suggested opener / question / common ground}

## Sources

1. {url} — {date} — {1-line description}
2. ...
```

## Output Format

In the chat (concise), and full brief written to Cloud Brain:

```
Brief generated: {Full Name} | Window: last {N} days | Confidence: {level}
Saved to Cloud Brain: research/people/Brief — {Name} — {date}

TOP 3 SIGNALS
1. {theme} — {1-line summary}
2. {theme} — {1-line summary}
3. {theme} — {1-line summary}

SUGGESTED OPENER
"{1-2 sentence opener that references something specific from the brief}"

Full brief: {cloud brain link or note title}
```

## Example Usage

**User:** "Give me a brief on Sarah Chen at Acme AI before my call tomorrow"

**AI:** Pulls handles from people/ if Sarah Chen exists, otherwise asks for LinkedIn. Searches X, Reddit, HN, YouTube, GitHub. Synthesizes 30-day brief. Saves to research/people/. Shows top 3 signals + opener.

**User:** "/brief-person-30days Peter Steinberger --x steipete --github steipete"

**AI:** Pulls last 30 days of @steipete X posts, GitHub PRs from steipete, Reddit mentions, HN submissions. Themes detected: ClaudeCode tooling, screen capture experiments, AI agent benchmarks. Brief saved.

**User:** "What's Sam Lee been up to lately?"

**AI:** Searches name across YouTube (his channel), Instagram (link in YouTube), podcasts, real estate news. 30-day window. Brief covers latest masterminds, new deal types he's discussing, partnerships announced.

**User:** "/brief-person-30days Alex Smith --days 7 --save"

**AI:** Tight 7-day window. Saves to Cloud Brain. Adds "Recent Briefs" entry to Mary's contact note in people/ if it exists.

## Error Handling

- **If multiple people match the name:** List the top 3 candidates with disambiguating info (company, location, role) and ask which one. Don't guess.
- **If person has zero recent public activity:** Report honestly — "I found {name} but they have no public activity in the last {N} days across the sources I checked. Want me to widen the window to 90 days, or pull historical context instead?" Don't pad with low-signal noise.
- **If GitHub username is supplied but private/no public events:** Note "GitHub: private or no public events in window" and skip.
- **If a source query fails (rate limit, fetch error):** Mark that source as `unavailable` in the Signal Strength table, continue with the rest. Never fail the whole brief because one source is down.
- **If the person is a public figure with overwhelming volume:** Cap each source at top 10 by engagement, note the truncation in the brief: "Signal volume was high — showing top 10 per source. Widen scope with --deep."
- **If the user asks for a brief on themselves:** Run it — useful for personal brand audits. Add a note: "This is a brief on yourself. Want this as a personal brand check-in instead? Run `/personal-brand-audit`."
- **If Cloud Brain save fails:** Report the error, present the brief inline in chat so it's not lost. Suggest retry.
- **If the brief is for a sales call and the user has a CRM lead for this person:** If `business-operations` is installed (detected via Cloud Brain dashboard note `Lead Pipeline`), read the lead via `mcp__cloud-brain__search_notes` and include deal stage, value, and last interaction in the "What This Means" section. Otherwise skip this enrichment.

## See Also

- `/brief-pre-meeting` — full pre-meeting prep that includes this brief plus Cloud Brain history (same plugin)
- `/brief-pre-call` — sales-call-specific game plan that uses this brief (same plugin)
- `/research-person` — static profile / biography (from `market-intelligence`)
- `/bizops-lead-tracker` — track this prospect in your pipeline (from `business-operations`)
