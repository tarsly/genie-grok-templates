---
name: brain-recall
description: >-
  Natural-language search across Cloud Brain — find that note, that thought, that decision, that person observation, that idea, that journal entry, or any prior capture. Implements the canonical short-keyword + recent_activity + semantic fallback strategy so you never miss content that's actually in your brain. Use whenever you ask 'what did I write about X', 'do I have notes on Y', 'what were my thoughts on Z', or 'when did I last…
---

# Brain Recall — Find Anything You've Ever Captured

## Overview

Brain Recall is your Cloud Brain search interface — done right. Cloud Brain's default search is keyword-based, not natural-language NLP, which means asking it "what did I think about Q3 hiring last month" returns nothing useful. Brain Recall solves this by translating your question into the right kind of query, falling back through multiple strategies until it finds what you're looking for. It tries short keywords first, then `recent_activity` if nothing matches, then semantic search for conceptual queries. You never have to think about how Cloud Brain indexes — you just ask, and it finds.

## When This Skill Applies

- User asks "what did I write about X" or "do I have notes on Y"
- User asks "when did I last X" or "have I captured anything about Z"
- User asks "search my brain for X" or "find notes on X"
- User says "remember when I…" or "I think I saved something about…"
- User asks "what's in my brain about X" or "show me everything on X"
- User asks "did I journal about X" or "did I dump anything about Y"
- User wants to find a specific person, project, idea, or decision they captured before
- User triggers the `/brain-recall` command
- Any question that begins with "where did I…" or "have I ever…"

## Category

Cloud Brain Retrieval

## Pre-Flight — Recall Preferences

1. Search Cloud Brain: `search_notes` with query `"brain coach recall preferences"` (folder: `brain/preferences`)
2. **If found:** Apply silently — no banner (fast path matters for retrieval)
3. **If not found:** Ask in ONE message:
   - Default number of results to show (default: 5)
   - Show full content of top hit automatically? (yes/no, default: yes)
   - Preferred search order (default: keyword → recent → semantic)
   - Default folder scope (any / specific list) — most users say "any"
   - Save to Cloud Brain: `write_note` → title: `brain-coach-recall-preferences`, folder: `brain/preferences`
4. After first run, never show the banner again — retrieval must feel instant

## How It Works

### Step 1: Translate the Question into Keywords

The user asks in natural language. Your job is to extract the 1-3 strongest content keywords. **Do NOT pass full sentences to `search_notes`** — that is the #1 cause of "no results" on Cloud Brain.

Examples:

| User asks | Keywords to try |
|---|---|
| "What did I write about Tate's ops takeover?" | `Tate ops`, `Tate`, `ops takeover` |
| "Have I journaled about my back pain?" | `back pain`, `back hurt`, `cardio` |
| "When did I last capture an idea for Warrior Thoughts?" | `Warrior Thoughts idea`, `Warrior Thoughts` |
| "What were my thoughts on the Acme role?" | `Acme`, `Acme role` |
| "Do I have anything about RV park financing?" | `RV park financing`, `RV financing` |

If the user already provided a single specific keyword (e.g., "find my note on tithing"), use it directly.

### Step 2: Run the Fallback Cascade

Execute searches in this order. **Stop as soon as you have ≥3 hits with title matches.**

1. **`search_notes` with the top keyword** (default `search_type: "hybrid"`)
2. If empty → **`search_notes` with the 2nd keyword variant**
3. If still empty → **`recent_activity`** with `limit: 20` to see if it's a recent capture the keyword search missed
4. If still empty → **`search_notes` with `search_type: "semantic"`** using the user's original phrasing — semantic embeddings handle "concept matches" that keyword search misses
5. If still empty → ask the user to narrow ("Try a person name, project name, or one specific word from the note")

### Step 3: Read the Top Result

If preferences are set to auto-open top hit, `read_note` on the highest-relevance result and present its content inline. Otherwise just list the hits.

### Step 4: Present Results

Format:

```
🧠 Brain Recall — "{user's question}"

Found {N} matches via {keyword | recent | semantic}:

1. [{folder}/{title}] — {created date}
   {one-line excerpt or 80-char content preview}

2. [{folder}/{title}] — {created date}
   {one-line excerpt}

…

Top hit (full content):
─────────────────────────
{full content of top hit, OR "skipped — preferences disable auto-open"}
─────────────────────────
```

If the search fell through every level and found nothing:

```
🧠 Brain Recall — "{user's question}"

No matches. Tried: {keyword variants tried}, recent_activity, semantic.

Try narrowing: a person's name, a project name, or one specific word you remember writing.
```

### Step 5: Offer Next Actions

If results were found, surface 2-3 contextual next actions:
- "Want me to open #2 too?"
- "Want me to search for related notes about {person/project mentioned in top hit}?"
- "Want me to capture a new thought on this?" (→ `brain-dump`)

## Data Structure

This skill is **read-only.** It writes nothing to Cloud Brain except optionally a recall log if the user opts in via preferences:

```markdown
---
date: {YYYY-MM-DD HH:MM}
query: "{original question}"
strategy: keyword|recent|semantic
results_count: N
tags: [recall-log]
---

# Recall log — {date}

**Query:** {original question}
**Top hit:** {folder/title}
```

(Default: logging OFF. Only enabled if user sets `log_recalls: true` in preferences.)

## Output Format

See Step 4 above. Compact, scannable, one-line per result with a single full-content top hit. Never paste 5 full notes — that's noise.

## Example Usage

**User:** "What did I write about Tate wanting more ops responsibility?"

**AI:**
1. Keywords: `Tate ops`, `Tate`, `responsibility`
2. `search_notes("Tate ops")` → hit on `people/Tate.md`
3. `read_note` on the top hit, return:

```
🧠 Brain Recall — "Tate wanting more ops responsibility"

Found 1 match via keyword:

1. [people/Tate.md] — last updated 2026-06-18
   "Tate mentioned he's hungry for more daily ops at Genie. Look at org chart Q3."

Top hit (full content):
─────────────────────────
# Tate
…
## Observations
- **2026-06-18:** Tate was talking about wanting to take on more of the daily ops at Genie. He's hungry. We should look at what that org chart shift would look like next quarter.
─────────────────────────

Want me to also pull `projects/Genie.md` to see if I have related capacity notes?
```

---

**User:** "Did I capture anything about back pain?"

**AI:**
1. Keywords: `back pain`, `back hurt`
2. `search_notes("back pain")` → empty
3. `search_notes("back hurt")` → 1 hit in `daily/2026-06-20.md`
4. Returns formatted result with the hit's body section excerpt.

---

**User:** "Find my journal entries about decision fatigue"

**AI:**
1. Keywords: `decision fatigue`, `fatigue`, `decisions`
2. Keyword search → 1 partial hit
3. Semantic search → 3 more conceptual matches in `journal/`
4. Combined list of 4 hits, top one opened.

---

**User:** "When did I last write daily goals?"

**AI:**
1. Keywords: `daily goals`, `goals`
2. `recent_activity` filtered by `folder: daily` — returns most recent `daily/*-goals.md` file
3. Presents date + excerpt.

## Error Handling

- **If the user's query is a full sentence (>10 words):** Extract keywords first; never pass the raw sentence to `search_notes`. Show the user which keywords you tried in the result header so they can refine if needed.
- **If keyword search returns 50+ noisy hits:** Narrow with a second keyword or folder filter rather than dumping all. Ask: "I got a lot — want to narrow to a specific folder ({list top 3 folders}) or add another keyword?"
- **If a referenced person/project doesn't exist as a note:** Tell the user "{name} doesn't have a note yet — want me to search for mentions of them across other notes?" then run a content-scope search.
- **If Cloud Brain is unreachable:** Tell the user explicitly: "Cloud Brain is unreachable. Try again in a moment, or search local notes with `grep` if you have a local mirror." Do NOT fabricate results from training data.
- **If the search finds something but the user says it's not what they wanted:** Re-run with semantic search and ask one clarifying question — what time period, what context, what surrounding context they remember.
- **If results are in mixed folders and the user asked about a specific topic:** Group results by folder in the output so the user can see "1 in people, 2 in journal, 1 in inbox" at a glance.
- **If the top hit is over 2000 words:** Don't dump the whole thing — show the first 30 lines with a "…(N more lines)" marker and ask "Want me to keep reading?"
- **If the user asks about a date range ("last month", "this quarter"):** Use `recent_activity` with a date filter rather than `search_notes`. Date-range queries are exactly what `recent_activity` exists for.
