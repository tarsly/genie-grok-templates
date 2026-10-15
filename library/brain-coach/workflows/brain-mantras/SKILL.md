---
name: brain-mantras
description: >-
  Affirmations and identity statements — manage your personal mantras, drive a daily read-aloud session, and audit which mantras still land each quarter. Three sub-commands: manage (add/remove/edit), read (daily session driver), audit (quarterly review). Use for affirmation practice, identity work, declaration ritual, or 'speak my mantras' moments. Distinct from goal-hierarchy (what you're doing) — mantras are who you're being.
---

# Brain Mantras — Speak Who You're Becoming

## Overview

Brain Mantras manages your personal affirmations and identity statements as a first-class brain object. Goals are what you do; mantras are who you are. This skill stores them in one canonical list, drives a daily read-aloud session (paced, one mantra at a time so each one actually lands), and runs a quarterly audit to retire mantras that have stopped landing and add new ones that match who you're becoming. It tracks read-aloud completion as a habit signal so you can see — across months — which mantras you've spoken into reality most often.

## When This Skill Applies

- User says "mantras" or "affirmations" or "identity statements"
- User says "read my mantras" or "speak my affirmations" or "go through my declarations"
- User says "add a mantra" or "update my mantras" or "remove this affirmation"
- User says "audit my mantras" or "review my affirmations" (quarterly cadence)
- User triggers `/brain-mantras`
- Morning routine context where user wants identity reinforcement (NOT the same as `brain-daily-goals` — that anchors action, this anchors identity)

## Category

Cloud Brain Identity Practice

## Pre-Flight — Mantras Preferences

1. Search Cloud Brain: `search_notes` with query `"brain coach mantras preferences"` (folder: `brain/preferences`)
2. **If found:** Apply silently
3. **If not found:** Ask in ONE message:
   - Read-aloud format: `silent` (just display, no speak prompt), `spoken` (prompt user to speak each aloud), `audio-cue` (prompt with a beat between each)
   - Mantra order: `fixed` (same order every time, builds rhythm) or `shuffled` (one random pick per slot, breaks autopilot)
   - Log each read-aloud session? (yes/no, default: yes — feeds streak data)
   - Default reading frequency target: `daily`, `weekly`, or `custom`
   - Save to Cloud Brain: `write_note` → title: `brain-coach-mantras-preferences`, folder: `brain/preferences`
4. Show banner once on first run:
   ```
   🔥 Mantras | Format: {spoken} | Order: {fixed} | Logging: ✓
   ```

## How It Works

### First Run — Seed from User

If `mindset/mantras.md` does not exist:

1. Tell the user: "You don't have a mantras list yet. Want to start from scratch or seed from your CLAUDE.md if you've defined them there?"
2. If they want to seed, look in `/Users/{user}/CLAUDE.md` for mantra-style statements (declarative "I" statements). Surface candidates. Let them confirm/edit before saving.
3. Otherwise prompt: "Type your first mantra. We'll add more later. Format: first-person declarative ('I am…', 'I do…', 'I think…')."
4. Save initial list to `mindset/mantras.md`.

### Sub-Command: `manage`

Three flows:
- **Add:** "What's the mantra?" → append to list → save → confirm
- **Remove:** Show numbered list → "Which to remove?" → confirm → save
- **Edit:** Show numbered list → "Which to edit?" → show current → "New version?" → save

Every mutation logs a line in `mindset/mantras.md` change history (preserving prior phrasings — these often evolve and the evolution is worth keeping).

### Sub-Command: `read` (default if no sub-command given)

The daily read-aloud session:

1. Load `mindset/mantras.md`
2. Order them (fixed = file order; shuffled = random)
3. For each mantra in order:
   - **If `format=silent`:** Display the mantra, wait for user input (anything, including just enter) before showing next
   - **If `format=spoken`:** Display the mantra and prompt: *"Speak it out loud. Press enter when done."*
   - **If `format=audio-cue`:** Same as spoken but with explicit pacing — "Take a breath. Read it slowly. Then next."
4. After all mantras shown, log the session to `mindset/read-aloud-log.md`

### Sub-Command: `audit`

Run quarterly (suggest it if last audit was >90 days ago):

For each mantra:
> "How does this land right now? (a) Still strong — keep, (b) Half-true — refine the language, (c) Doesn't fit anymore — retire"

Mantras marked (c) move to `mindset/mantras-retired.md` with a date and a one-line "what changed" note. Mantras marked (b) get re-edited.

After audit, ask: "What's emerging for who you're becoming next? Want to add a new mantra to capture it?"

## Data Structure

### `mindset/mantras.md`

```markdown
---
type: mantras-list
last_updated: {YYYY-MM-DD}
last_audited: {YYYY-MM-DD}
tags: [mantras, identity, brain-coach]
---

# My Mantras

> *Identity declarations. Who I'm being — not just what I'm doing.*

1. {mantra 1}
2. {mantra 2}
3. {mantra 3}
{…}

---

## Change history

- **{YYYY-MM-DD}:** Added: "{new mantra}"
- **{YYYY-MM-DD}:** Edited #4: "{old}" → "{new}"
- **{YYYY-MM-DD}:** Retired (moved to mantras-retired): "{old}"
```

### `mindset/read-aloud-log.md`

```markdown
---
type: read-aloud-log
tags: [mantras, log, brain-coach]
---

# Read-Aloud Log

| Date | Time | Count | Format | Notes |
|------|------|-------|--------|-------|
| 2026-06-20 | 04:30 | 8 | spoken | full set |
| 2026-06-19 | 04:35 | 5 | spoken | shortened — running late |
| 2026-06-18 | 04:32 | 8 | spoken | full set |
```

### `mindset/mantras-retired.md`

```markdown
---
type: mantras-retired
tags: [mantras, retired, brain-coach]
---

# Retired Mantras

> *Not failures. Evolutions. Kept for memory.*

- **{YYYY-MM-DD}:** "{old mantra}" — retired because {what changed}
```

## Output Format

### After a `read` session

```
🔥 Read-aloud complete — {N} mantras spoken at {HH:MM}
Streak: {N} days
{Optional observation if anything notable: missed a day, hit a milestone, etc.}
```

### After `manage` add

```
🔥 Mantra added — now {N} total
Edit any time with `/brain-mantras manage`.
```

### After `audit`

```
🔥 Mantra audit complete.
- Kept: {N}
- Refined: {N}
- Retired: {N}
- Added: {N}
Next audit suggested: {date in ~90 days}
```

## Example Usage

**User:** "/brain-mantras read"

**AI:** Loads mantras list. Walks through 8 mantras (the user's starter list: "Love BIG, Think BIG!", "I give ONE MORE!", "I am EXTRAORDINARY.", "I MAX OUT My Life.", etc.) one at a time, spoken format. After each, waits for enter. At end:

```
🔥 Read-aloud complete — 8 mantras spoken at 04:32
Streak: 14 days
"I Do It Anyways" landed differently today — the one you paused on the longest. Worth a journal moment.
```

---

**User:** "Add a mantra: 'I do hard things and they make me free.'"

**AI:**
```
🔥 Mantra added — now 9 total
Edit any time with `/brain-mantras manage`.
```

---

**User:** "/brain-mantras audit"

**AI:** Walks through all 9 mantras one at a time, asking keep/refine/retire for each. Saves edits. Returns the summary block.

---

**User:** "Speak my affirmations"

**AI:** Detects "speak my affirmations" → maps to `read` sub-command (spoken format). Runs the session.

## Error Handling

- **If `mindset/mantras.md` doesn't exist on first `read` or `audit`:** Don't error. Run first-run flow: offer to seed from CLAUDE.md or start with the user's first mantra.
- **If `mantras.md` has 0 entries:** Tell the user "You have zero mantras. Want to add one now?" Don't run an empty read session.
- **If user runs `read` 3+ times in one day:** Allow it, but log each session. The pattern is the data — sometimes a mantra needs multiple reads in one day.
- **If user requests `audit` and last audit was <30 days ago:** Confirm: "You audited {date} ({N} days ago). Run another? (audits work best quarterly)"
- **If user tries to add a mantra that already exists (verbatim match):** Tell them: "That mantra already exists at #{N}. Add anyway, or edit existing?"
- **If user tries to retire all mantras at once:** Confirm explicitly: "Retiring all {N} mantras. Are you sure? You'll be starting from scratch."
- **If a mantra is very long (>200 chars):** Suggest: "That's a long one — easier to land if it's shorter. Want to try a tighter version?" Don't force.
- **If Cloud Brain is unreachable:** Don't run `read` (the whole point is logging the practice). Tell user: "Cloud Brain unreachable — can't log this session. Try in a moment, or read from a local copy if you have one."
- **If `mantras.md` has been edited externally (timestamp newer than last in-skill edit):** Read fresh before mutating. Never overwrite an external edit you didn't see.
