---
name: brain-dump
description: >-
  Quick-capture brain dump — speak or type any thought, idea, observation, person, or project mention, and AI routes it to the right Cloud Brain folder. Use for raw idea capture, mind sweeps, voice memos pasted in, get-it-out-of-my-head moments, or any 'just save this somewhere' request. Distinct from follow-up tracking — this captures thoughts, not commitments to others.
---

# Brain Dump — Get It Out of Your Head, Into Your Brain

## Overview

Brain Dump is the lowest-friction capture point in your Cloud Brain. Speak it, paste it, type it — the AI reads what you said, decides where it belongs, and files it. No menus. No tags to pick. No folders to choose. If you mention a person, it appends to that person's note. If you mention a project, it appends to that project's note. If it's a raw idea, it goes to `ideas/`. Everything else lands in `inbox/` for a future sweep. The goal: zero excuses to keep something in your head when it could be in your brain.

## When This Skill Applies

- User says "brain dump" or "braindump" or "dump this"
- User says "remember this" or "save this thought" or "capture this"
- User says "quick note" or "just a thought" or "get this out of my head"
- User pastes voice-transcribed text and says "save it"
- User says "I just realized…" or "thought about…" or "interesting that…"
- User mentions a person + an observation without an explicit ask ("Tate mentioned…")
- User mentions a project + an idea without explicit instruction ("for Warrior Thoughts…")
- User triggers the `/brain-dump` command
- Any low-context capture request that is NOT a commitment to another person (those route to `bizops-follow-up`)

## Category

Cloud Brain Capture

## Pre-Flight — Brain Dump Preferences

1. Search Cloud Brain: `search_notes` with query `"brain coach dump preferences"` (folder: `brain/preferences`)
2. **If found:** Apply silently — no banner (fast path matters for capture)
3. **If not found:** Ask in ONE message:
   - Default inbox folder (default: `inbox`)
   - Auto-route to people notes when a known person is mentioned? (yes/no, default: yes)
   - Auto-route to project notes when a known project is mentioned? (yes/no, default: yes)
   - Suggest a follow-up when capture sounds like a commitment? (yes/no, default: yes)
   - Save to Cloud Brain: `write_note` → title: `brain-coach-dump-preferences`, folder: `brain/preferences`
4. After first run, never show the banner again — capture must be fast

## How It Works

### Step 1: Read the Dump

Take the entire raw text the user provided. Do not edit, summarize, or restructure it before deciding where it goes. The user's words go in verbatim; only the routing decision is yours.

### Step 2: Detect Routing Signals

Walk the text and identify the strongest signal:

| Signal | Routing | Example |
|---|---|---|
| Names a person (first + last, or known first name) | `people/{name}.md` (append) | "Tate mentioned he's blocked on…" |
| Names a known project | `projects/{project}.md` (append) | "For Warrior Thoughts, we should…" |
| Starts with "idea for X" or "what if X" | `ideas/{topic-slug}.md` (new) | "Idea for the app: streak freezes" |
| Contains a future-tense commitment to another person ("I'll send", "I told them I would") | Recommend `bizops-follow-up` and stop | "I told Sarah I'd send the deck Friday" |
| Health/body observation ("slept 5 hours", "back hurt today") | `daily/YYYY-MM-DD.md` (append under `## Body`) | "Energy crashed at 2pm again" |
| Decision in progress ("deciding whether to…") | Recommend `brain-journal --mode decision` | "Deciding whether to take on this client" |
| Default | `inbox/YYYY-MM-DD-{slug}.md` (new) | Anything that doesn't match |

**Use `search_notes` to verify a known person/project exists** before routing there. If the name is ambiguous, ask one question: *"Is this about {Person A} or {Person B}? Or someone else?"*

### Step 3: Write or Append

**For new files** (`inbox/`, `ideas/`):

`write_note`:
- **folder:** `inbox` (or `ideas`)
- **title:** `{YYYY-MM-DD} — {first-5-words-of-thought}` (e.g., `2026-06-20 — back hurt during cardio today`)
- **tags:** `["brain-dump", "{routing-reason}"]`
- **content:** see Data Structure below

**For appends** (`people/`, `projects/`, `daily/`):

1. `search_notes` to locate the existing note
2. `read_note` to fetch it
3. `edit_note` to append a new dated entry under the appropriate section (create the section if missing)

### Step 4: Confirm

Reply with ONE LINE — no fluff:

```
📥 Captured to {folder}/{title} — {routing reason in 4 words or less}
```

That's it. No summary of what they said. No suggestions. No follow-up questions unless routing was ambiguous.

## Data Structure

### New `inbox/` entry

```markdown
---
captured: {YYYY-MM-DD HH:MM}
source: brain-dump
routed: false
tags: [brain-dump, inbox]
---

# {first-5-words}

{raw verbatim user text}

---

**Routing notes:** {one line — why this landed in inbox vs. routed elsewhere}
```

### New `ideas/` entry

```markdown
---
captured: {YYYY-MM-DD}
status: seed
tags: [idea, brain-dump]
---

# {idea topic}

{raw verbatim user text}

## Why it matters
{one line if obvious from the dump, otherwise blank}

## Next step
{one line if obvious, otherwise blank — leave for later sweep}
```

### Append to `people/{name}.md`

Append under a `## Observations` section (create if missing):

```markdown
## Observations

- **{YYYY-MM-DD}:** {raw verbatim user text}
```

### Append to `projects/{project}.md`

Append under a `## Notes` section:

```markdown
## Notes

- **{YYYY-MM-DD}:** {raw verbatim user text}
```

### Append to `daily/YYYY-MM-DD.md`

Append under the section that fits (`## Body`, `## Energy`, `## Random`, etc. — create if missing):

```markdown
## {Section}

- **{HH:MM}:** {raw verbatim user text}
```

## Output Format

Single-line confirmation. No extras unless an ambiguity needed resolving.

```
📥 Captured to ideas/streak-freezes-warrior-thoughts.md — new idea
```

```
📥 Captured to people/Tate.md — observation about Tate
```

```
📥 Captured to inbox/2026-06-20-tax-question-for-kevin.md — unrouted
```

If the dump sounded like a commitment, intercept:

```
That sounds like a commitment to {person}. Want me to add it to your follow-ups instead?
(yes → uses bizops-follow-up | no → captures as a regular dump)
```

## Example Usage

**User:** "/brain-dump Tate was talking about wanting to take on more of the daily ops at Genie. He's hungry. We should look at what that org chart shift would look like next quarter."

**AI:** Searches for an existing `people/Tate.md` note. Found. Appends an observation. Returns:
```
📥 Captured to people/Tate.md — observation about Tate
```

---

**User:** "/brain-dump Idea — what if Warrior Thoughts had a streak freeze you could earn by completing a longer reflection? Like Duolingo but the freeze costs more than just showing up."

**AI:** Detects "Idea —" prefix. Routes to ideas/. Returns:
```
📥 Captured to ideas/streak-freezes-warrior-thoughts.md — new idea
```

---

**User:** "/brain-dump back hurt during cardio this morning, third day in a row. need to check form on deadlifts."

**AI:** Detects body/health observation. Appends to today's daily note under `## Body`. Returns:
```
📥 Captured to daily/2026-06-20.md — body observation
```

---

**User:** "/brain-dump told Annika I'd review her social media calendar by Friday"

**AI:** Detects commitment pattern. Intercepts:
```
That sounds like a commitment to Annika. Want me to add it to your follow-ups instead?
(yes → uses bizops-follow-up | no → captures as a regular dump)
```

---

**User:** "/brain-dump just feeling really grateful for Mary today. The way she handled the kids this morning was such a gift."

**AI:** Detects gratitude content. Suggests, but defaults to dump if user doesn't redirect:
```
📥 Captured to people/Mary.md — observation about Mary
```
*(If user prefers, this could route to `brain-journal --mode gratitude` — preference set in pre-flight.)*

## Error Handling

- **If the user provides no text after `/brain-dump`:** Ask: "What do you want to capture? Speak or paste it — I'll figure out where it goes."
- **If a referenced person/project doesn't exist in Cloud Brain:** Default to creating the note. Confirm: "I don't have a note for {name} yet — creating one and capturing this as the first entry. Confirm?"
- **If multiple people/projects match the same name:** Ask once: "Is this about {A} or {B}?" Do not guess.
- **If the dump is very long (>2000 words):** Capture it whole but warn: "That's a long dump — captured verbatim. Want me to also produce a 3-sentence summary at the top?"
- **If Cloud Brain is unreachable:** Save to a local fallback at `~/.claude/brain-coach-pending/{YYYY-MM-DD-HH-MM}.md` and tell the user: "Cloud Brain is unreachable. Captured locally — I'll re-sync when it's back. Run `/brain-dump --sync` to retry."
- **If the dump is clearly a follow-up commitment to another person:** Recommend `bizops-follow-up` but DO NOT auto-create — the user must confirm to avoid duplicating between systems.
- **If the dump contains multiple distinct thoughts:** Capture the whole thing once to the strongest-signal folder. Do not split — splitting loses context and creates orphaned shards.
- **If routing is genuinely uncertain after one clarifying question:** Default to `inbox/` with `routed: false` and a routing note explaining why. The user can sweep inbox later.
