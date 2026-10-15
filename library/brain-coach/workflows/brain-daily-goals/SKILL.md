---
name: brain-daily-goals
description: >-
  Daily goal-writing ritual — reads your goal hierarchy from Cloud Brain, displays your top quarterly priorities, prompts you to re-write today's expression of those goals in first-person present-tense, and captures today's 1MIT (One Most Important Thing). The 'write your goals every day' non-negotiable, automated. Distinct from goals-pulse (5-min check-in) and bizops-daily-brief (CEO briefing) — this is the daily reinforcement ritual.
---

# Brain Daily Goals — Write Them Down Every Day

## Overview

Brain Daily Goals automates the "write your goals every day" non-negotiable. It reads your goal hierarchy from Cloud Brain, shows you your active quarterly priorities, and prompts you to write today's expression of those goals — in first-person present-tense by default ("I am building Warrior Thoughts to launch by summer 2026"), then captures the one most important thing you'll move forward today. Writing your goals daily is not redundant — it's the act of re-deciding. This skill removes every friction point between the intention and the act.

## When This Skill Applies

- User says "write my goals" or "daily goals" or "write goals for today"
- User says "goal writing" or "goals ritual" or "morning goals"
- User says "1MIT" or "one most important thing" or "what's the one thing today"
- User says "remind me of my goals" + the time is morning (this is the writing version; for a 5-min check-in use `goals-pulse`)
- User triggers `/brain-daily-goals`
- Morning routine context where user wants to anchor the day in their goals (NOT a CEO briefing — that's `bizops-daily-brief`)

## Category

Cloud Brain Daily Rhythm

## Pre-Flight — Daily Goals Preferences

1. Search Cloud Brain: `search_notes` with query `"brain coach daily-goals preferences"` (folder: `brain/preferences`)
2. **If found:** Apply silently and proceed
3. **If not found:** Ask in ONE message:
   - Voice: `first-person-present` ("I am…") or `raw` (write them in your own words however) — default: `first-person-present`
   - How many goals to re-write each day (default: 3 — quarterly priorities only)
   - Ladder depth: `quarterly` (only Q priorities) or `full` (yearly → quarterly → weekly) — default: `quarterly`
   - Capture a 1MIT (One Most Important Thing for today)? (yes/no, default: yes)
   - Capture an identity declaration ("Today I am the kind of person who…")? (yes/no, default: yes)
   - Save to Cloud Brain: `write_note` → title: `brain-coach-daily-goals-preferences`, folder: `brain/preferences`
4. Show banner on first run only:
   ```
   ✍️  Daily Goals | Voice: {first-person-present} | Ladder: {quarterly} | Preferences: ✓ loaded
   ```

## How It Works

### Step 1: Load the Goal Hierarchy

Read these from Cloud Brain (matches the pattern in `genie-admin/goals-pulse`):

1. `search_notes` for `goal-hierarchy` → `read_note` for the canonical hierarchy
2. `search_notes` for `quarterly-priorities` → current quarter's commitments
3. If `--ladder full`: also load `yearly-vision` and `weekly-priorities` if they exist

**If `goals/goal-hierarchy` does not exist:**

Say:
> "You haven't set up your goal hierarchy yet — that's the foundation this daily ritual reinforces. Set it up first with `genie-admin/goals-pulse` (or its setup flow), then come back. This skill won't write daily goals against a vacuum."

Then stop. Do not write placeholders or fake goals — the integrity of this ritual depends on the hierarchy being real.

### Step 2: Display the Source

Show the user their quarterly priorities as a clean read:

```
🎯 Your Q{N} priorities:

1. {priority 1 title}
2. {priority 2 title}
3. {priority 3 title}
{etc.}
```

If `--ladder full`, also show the parent yearly vision and the current week's focus.

### Step 3: Prompt the Writing

In the chosen voice, ask the user to write today's expression. If `voice=first-person-present`, default phrasing:

> "Write today's version of each priority. First-person, present-tense. Like it's already happening.
>
> Priority 1 — '{title}'. Your daily expression of this:"

Wait for the user. Then move to priority 2, 3, etc.

If `voice=raw`, just ask:
> "Write priority 1 in your own words for today:"

### Step 4: 1MIT (One Most Important Thing)

> "Of everything you could do today, what is the ONE thing that — if you only got that done — today would be a win? Be specific. Don't list two."

Capture the answer. If the user lists multiple, push back once: *"Pick one. The whole point is choosing."*

### Step 5: Identity Declaration (optional)

> "Today I am the kind of person who ____. Fill in the blank."

This is the identity reinforcement step — small but powerful.

### Step 6: Write the Entry

`write_note`:
- **folder:** `daily`
- **title:** `{YYYY-MM-DD}-goals`
- **tags:** `["daily-goals", "ritual", "brain-coach"]`
- **content:** see Data Structure below

### Step 7: Closing

End with ONE line — observational, specific:
- "Your 1MIT today is the same as Tuesday's. It's not getting done. Worth asking why before you start."
- "First-person present-tense across all three priorities — strong write today."
- "Identity line names a person you want to be, not just an action. That's the work."

Then:
> "Day anchored. Go."

## Data Structure

```markdown
---
date: {YYYY-MM-DD}
type: daily-goals
voice: {first-person-present|raw}
ladder: {quarterly|full}
quarterly_source: {filename or reference}
tags: [daily-goals, ritual, brain-coach]
---

# Daily Goals — {YYYY-MM-DD}

> *Written {HH:MM}. Reinforcement of Q{N} priorities.*

## Today's expression of my priorities

### 1. {priority title}
{user's daily expression — first-person present-tense or raw}

### 2. {priority title}
{user's daily expression}

### 3. {priority title}
{user's daily expression}

{… etc per quarterly priority …}

## 1MIT — One Most Important Thing

{the one thing that makes today a win if it's the only thing done}

## Identity declaration

**Today I am the kind of person who** {user response}.

## Optional ladder context _(only if --ladder full)_

**Yearly vision:** {one-line from yearly-vision}
**This week:** {one-line from weekly-priorities}
```

## Output Format

After saving:

```
✍️  Daily goals written → daily/{YYYY-MM-DD}-goals.md
{One-line observation specific to what they wrote.}

Day anchored. Go.
```

If the user already wrote daily goals today, surface that on the next invocation:

```
You already wrote goals today at {time}. Want to overwrite, append a second pass, or read back what you wrote?
```

## Example Usage

**User:** "/brain-daily-goals"

**AI:**
1. Loads `goal-hierarchy` + `quarterly-priorities` from Cloud Brain
2. Displays the owner's Q2 priorities (e.g., Warrior Thoughts MVP shipped, RV park deal closed, Acme W2 fortified)
3. Walks through prompts in first-person present-tense voice
4. Captures 1MIT and identity line
5. Writes `daily/2026-06-20-goals.md`
6. Returns:

```
✍️  Daily goals written → daily/2026-06-20-goals.md
Your 1MIT today (finish Warrior Thoughts onboarding wireframes) directly serves your keystone. That's clean alignment.

Day anchored. Go.
```

---

**User:** "Write my goals"

**AI:** Same flow as above. Detects "write my goals" as the daily-goals trigger (NOT goals-pulse — that's "check on" goals).

---

**User:** "/brain-daily-goals --ladder full"

**AI:** Shows yearly vision + quarterly + this week, then runs the prompts. Resulting entry includes the full ladder in frontmatter for future hierarchy traversal.

---

**User opens the skill, but their goal hierarchy doesn't exist:**

**AI:**
```
You haven't set up your goal hierarchy yet — that's the foundation this daily ritual reinforces. Set it up first with `genie-admin/goals-pulse` (or its setup flow), then come back. This skill won't write daily goals against a vacuum.
```

Stops. Does not create placeholders.

## Error Handling

- **If `goals/goal-hierarchy` is missing:** STOP. Do not create fake goals. Redirect to the hierarchy-setup flow (`genie-admin/goals-pulse` setup or whatever is canonical).
- **If `quarterly-priorities` is missing but hierarchy exists:** Tell the user: "Hierarchy is set, but I don't see this quarter's priorities. Run `eos-rocks` (if EOS) or `goals-pulse` to set them, then come back."
- **If the user already wrote goals today:** Ask whether to overwrite, append a second pass, or just read back. Default to "read back" — re-writing twice is fine but should be deliberate.
- **If the user's daily expression of a priority drifts far from the priority title:** Flag it gently in the closing observation, not mid-flow: "Your daily expression of #2 was about something different than the priority title — worth a check whether the priority needs updating."
- **If the user gives 1-word responses ("yes" for each priority):** Mirror it. Save what they gave. Add a frontmatter flag `low_engagement: true` for future pattern analysis.
- **If the user wants to skip 1MIT:** Allow it, but log: "No 1MIT chosen today" in the entry. The pattern over weeks tells a story.
- **If the user tries to list more than one 1MIT:** Push back once. If they insist on multiple, save all but tag the entry `1mit_violated: true`.
- **If Cloud Brain is unreachable:** Save the entry locally to `~/.claude/brain-coach-pending/daily-goals-{YYYY-MM-DD}.md`. Sync on retry.
- **If the user is on a streak (e.g., 7 consecutive days written):** Surface it in the closing line: "Day 8 in a row. The pattern is the practice."
