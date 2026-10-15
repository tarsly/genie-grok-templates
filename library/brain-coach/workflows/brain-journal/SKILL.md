---
name: brain-journal
description: >-
  Multi-mode personal journal — free-write, gratitude, decision, emotional, or dream entry. Each mode has tailored prompts and a distinct file structure so future search is fast. Use for journaling, gratitude practice, decision capture, expressive writing on a tough event, or dream logging. Distinct from brain-dump (raw capture) and brain-evening (end-of-day reflection ritual) — this is intentional, prompted, reflective writing.
---

# Brain Journal — Five Modes, One Practice

## Overview

Brain Journal is a structured personal journaling system with five distinct modes — each with its own prompts, its own file structure, and its own searchable tags. Free mode is open-ended. Gratitude is a 5-prompt practice. Decision mode captures the texture of a choice you're making (so future-you can see how present-you was thinking). Emotional mode uses Pennebaker-style expressive writing to process a difficult event. Dream mode logs dreams quickly so patterns become searchable over months. All five share one folder (`journal/`) and consistent frontmatter, so `brain-recall` can find anything across them.

## When This Skill Applies

- User says "journal" or "let me journal" or "I want to journal"
- User says "gratitude practice" or "gratitude journal" or "what I'm grateful for"
- User says "I need to think through a decision" or "I'm deciding…"
- User says "I need to process this" or "expressive writing" or "I'm upset about…"
- User says "log a dream" or "I had a weird dream" or "dream journal"
- User triggers `/brain-journal` (with or without `--mode`)
- User asks for journaling prompts
- Distinct from `/brain-dump` (raw capture) and `/brain-evening` (end-of-day ritual)

## Category

Cloud Brain Reflection

## Pre-Flight — Journal Preferences

1. Search Cloud Brain: `search_notes` with query `"brain coach journal preferences"` (folder: `brain/preferences`)
2. **If found:** Apply silently — no banner before journaling (creating friction here defeats the point)
3. **If not found:** Ask in ONE message:
   - Default mode when none specified (default: `free`)
   - Default journaling style: `typed` (you type back-and-forth) or `dictated` (user dumps a long block; AI structures it after)
   - Prompt verbosity: `terse` (one question at a time) or `full` (all prompts visible at once)
   - Default mood-tracking: include a 1-10 mood at the top of each entry? (default: yes)
   - Save to Cloud Brain: `write_note` → title: `brain-coach-journal-preferences`, folder: `brain/preferences`
4. After first run, never show a banner — just open the entry

## How It Works

### Step 1: Determine Mode

If `--mode` is set, use it. Otherwise:
1. Infer from the user's language ("grateful" → gratitude, "deciding" → decision, "processing" → emotional, "dream" → dream)
2. If ambiguous, ask once: "What mode? (free / gratitude / decision / emotional / dream)" — show one-line description for each

### Step 2: Run the Mode

Each mode has its own prompt flow. Match the user's verbosity preference (terse = one prompt at a time; full = all prompts in one message).

### Step 3: Write the Entry

Single `write_note` call at the end of the session. Never write a partial entry mid-conversation — wait until the user signals done (or the prompt flow completes).

- **folder:** `journal`
- **title:** `{YYYY-MM-DD}-{mode}` (e.g., `2026-06-20-gratitude`). If multiple entries in same mode same day, append `-2`, `-3`.
- **tags:** `["journal", "{mode}", "brain-coach"]` + any mode-specific tags (see below)

### Mode 1: Free

**Prompt:**
> "Open page. What's on your mind?"

Wait for the user. They write as much or as little as they want. After they finish, ask ONE follow-up:
> "Anything you want to underline as important?"

Save with whatever they marked as `key_takeaways` in frontmatter.

### Mode 2: Gratitude (5 prompts)

Ask the 5 prompts in sequence (terse) or all at once (full):

1. **One person** you're grateful for today and why
2. **One thing about your body** you're grateful for today
3. **One thing about today** specifically that you're grateful for (not generic — today)
4. **One challenge** you're grateful for and what it's teaching you
5. **One future thing** you're grateful for as if it's already happened

Add the entry to `journal/`. Also append a one-line summary to `mindset/gratitude-streak.md` for streak tracking.

### Mode 3: Decision

Walk through this frame:

1. **What am I deciding?** (one sentence)
2. **What are the options on the table?** (list)
3. **What does my gut say right now?** (no analysis, just the gut)
4. **What's the cost of waiting a week?** (often clarifies urgency)
5. **What would make this an easy yes? An easy no?**
6. **What does the version of me I'm becoming choose?**

Tag with `decision-pending`. After the user makes the decision, they can edit the entry (or run `/brain-journal --mode decision --resolve {date}`) to add the outcome — making decisions a searchable archive of how present-you thinks vs. how it played out.

### Mode 4: Emotional (Pennebaker-style)

For processing a difficult event. Frame:

1. **What happened?** (factual, no editorial)
2. **What did you feel?** (name the emotions specifically — anger? shame? grief? helplessness?)
3. **What does this remind you of?** (often surfaces a pattern)
4. **What part of this is mine to own? What part isn't?**
5. **What's one next action — even if small — that moves you forward?**

Mood tracking: 1-10 before and after (often shifts during writing — that itself is the data).

Tag with `emotional`. Stored in `journal/` like everything else (NOT a separate "secret" folder — secrecy creates avoidance).

### Mode 5: Dream

Fast-capture mode. No prompts — just:

> "Dream content (type or dictate). I'll tag and file it."

User dumps the dream. Auto-detect and tag:
- People mentioned → tag each
- Locations mentioned → tag
- Recurring symbols (water, falling, lost, chase, teeth, flying) → tag if present
- Emotional tone → tag (`peaceful`, `anxious`, `triumphant`, `unsettled`)

Save with tags so monthly recall queries ("what dreams have I logged about water?") work.

## Data Structure

### Free entry

```markdown
---
date: {YYYY-MM-DD}
time: {HH:MM}
mode: free
mood: {1-10}
key_takeaways: [{user-marked}]
tags: [journal, free, brain-coach]
---

# Free journal — {YYYY-MM-DD}

{full text from user}

## Underlined
- {key takeaway}
- {key takeaway}
```

### Gratitude entry

```markdown
---
date: {YYYY-MM-DD}
mode: gratitude
mood: {1-10}
tags: [journal, gratitude, brain-coach]
---

# Gratitude — {YYYY-MM-DD}

**Person:** {response}
**Body:** {response}
**Today:** {response}
**Challenge:** {response}
**Future-as-now:** {response}
```

### Decision entry

```markdown
---
date: {YYYY-MM-DD}
mode: decision
status: pending
mood: {1-10}
resolved_on: null
outcome: null
tags: [journal, decision, decision-pending, brain-coach]
---

# Decision — {one-sentence summary} — {YYYY-MM-DD}

**Deciding:** {response}

**Options:**
- {option A}
- {option B}
- …

**Gut:** {response}
**Cost of waiting a week:** {response}
**Easy yes / easy no:** {response}
**Future-me chooses:** {response}

---

**Decision made on:** _(blank — fill in later)_
**Outcome:** _(blank — fill in later when you can see it)_
```

### Emotional entry

```markdown
---
date: {YYYY-MM-DD}
mode: emotional
mood_before: {1-10}
mood_after: {1-10}
tags: [journal, emotional, brain-coach]
---

# Emotional journal — {YYYY-MM-DD}

**What happened:** {response}
**What I felt:** {response}
**This reminds me of:** {response}
**Mine to own / not mine:** {response}
**Next small action:** {response}
```

### Dream entry

```markdown
---
date: {YYYY-MM-DD}
mode: dream
tone: {peaceful|anxious|triumphant|unsettled|mixed}
people: [{tags}]
symbols: [{tags}]
tags: [journal, dream, brain-coach, {symbols}, {tone}]
---

# Dream — {YYYY-MM-DD}

{dream content verbatim}
```

## Output Format

After saving:

```
📔 {Mode} journal saved → journal/{title}.md
{1-sentence reflection — what stood out to you about what they wrote, NOT a summary}
```

Examples of good closing reflections (one sentence, observation-flavored):
- "You named the gut answer in the first sentence — usually that's the right one."
- "Your mood moved from 4 to 6 just by writing it down. That's the practice working."
- "Three of your five gratitudes today were about Mary. Worth noticing."

NOT a summary ("You wrote about…"). The user knows what they wrote. A reflection is data — a summary is noise.

## Example Usage

**User:** "/brain-journal --mode gratitude"

**AI:** Asks the 5 prompts (terse mode by default), then writes `journal/2026-06-20-gratitude.md`, returns:
```
📔 Gratitude journal saved → journal/2026-06-20-gratitude.md
Two of your five gratitudes today were about the gym. Body's becoming a source of gratitude, not just discipline.
```

---

**User:** "I want to journal about whether to take on this new client. They want a custom AI build but the timeline is brutal."

**AI:** Detects decision mode. Runs the 6-prompt frame. Writes `journal/2026-06-20-decision.md` with `status: pending`. Returns:
```
📔 Decision journal saved → journal/2026-06-20-decision.md — status: pending
You said your gut was "no" in step 3 but kept making the case for "yes" through steps 4-6. Worth re-reading in 24 hours.
```

---

**User:** "/brain-journal --mode dream"

**AI:** Single prompt. User dumps the dream. AI tags it (people: Mary, Tate; symbols: water, falling; tone: anxious). Writes `journal/2026-06-20-dream.md`. Returns:
```
📔 Dream journal saved → journal/2026-06-20-dream.md
Third water-themed dream in two months — might be worth a `brain-recall "water dream"` to see the pattern.
```

---

**User:** "Free journal"

**AI:** "Open page. What's on your mind?" → user writes for as long as they want → AI asks "Anything you want to underline as important?" → captures + saves.

## Error Handling

- **If the user starts journaling without picking a mode and the content is ambiguous:** Ask once. Don't guess silently.
- **If the user gives 1-2 word answers to prompts:** Mirror it. Don't push for more. Brevity is a valid mood signal.
- **If the same mode is journaled twice in a day:** Append `-2` to the filename. Don't overwrite.
- **If the user goes long in free mode (>1500 words):** Save it all. Offer at the end: "Want me to extract 3 key themes for the frontmatter so this is easier to find later?"
- **If the user is dictating and the transcript is messy:** Clean ONLY filler words ("um", "uh", "like") — preserve their phrasing, syntax, and emotional cadence. Do not paraphrase.
- **If the user starts in emotional mode and the content suggests crisis (self-harm, suicidal ideation, etc.):** Save the entry, then say: "I want to make sure you're safe. Do you want to reach out to a person you trust right now? If you're in immediate danger, call 988 (Suicide & Crisis Lifeline) or your local emergency number." Do not skip the entry — saving it is also a form of being heard.
- **If `journal/` folder doesn't exist yet:** Cloud Brain creates it on first write — no action needed, just proceed.
- **If a decision entry has been pending for 14+ days:** When the user opens a new decision entry, surface it: "Heads up — you have a decision from {date} still marked pending: '{summary}'. Want to resolve it before starting a new one?"
- **If the user wants to delete a sensitive entry:** Use `delete_note`. Confirm once. Don't soft-delete or version — when they say delete, it's gone.
