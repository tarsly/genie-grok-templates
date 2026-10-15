---
name: pro-apple-notes-search
description: >-
  Search your Apple Notes library by keyword and semantically — bridges your iCloud Notes into Claude conversations so notes you wrote on iPhone, iPad, or Mac are findable alongside your Cloud Brain. Requires the apple-notes MCP server. Surfaces note title, folder, last-modified date, and a snippet.
---

# Apple Notes Search

## Overview

Apple Notes is where most macOS users actually capture — quick thoughts on the phone, voice-to-text snippets, photos with annotations. But Notes search inside the app is brittle and the data is siloed from Cloud Brain. This skill bridges them.

Requires the `apple-notes` MCP server installed and authenticated against the user's Notes database. If not installed, the skill guides through setup.

## When This Skill Applies

- User asks "do I have a note about {topic}?"
- User mentions: Apple Notes, iCloud Notes, my notes on iPhone, what did I write down
- User wants to promote an Apple Note into Cloud Brain
- User wants to search across both Apple Notes AND Cloud Brain at once
- User says: "search my notes", "find my note on...", "I wrote about this somewhere"

## Pre-Flight — Setup Check

Check that the apple-notes MCP is available:

If `mcp__apple_notes__search` exists → proceed.

If not, show setup instructions:

```
The apple-notes MCP isn't installed. Setup:

  1. Install: npm install -g apple-notes-mcp  (or follow latest install docs)
  2. Add to your MCP config (~/.config/claude/mcp.json or equivalent):
     {
       "apple-notes": {
         "command": "apple-notes-mcp"
       }
     }
  3. Grant access to Notes via macOS System Settings → Privacy → Automation
  4. Restart Claude Code

Once installed, re-run this command.
```

Banner:
```
🎯 Apple Notes Search | MCP: {available/missing} | Account: {primary iCloud}
```

## How It Works

### Step 1: Parse Query

Identify:
- Search terms (keywords)
- Folder filter (`--folder` or "in my {folder name} folder")
- Date filter (`--since 2026-01-01` or "from last month")
- Semantic vs. keyword mode (`--semantic` flag)
- Limit (default 20)

### Step 2: Search

Use `mcp__apple_notes__search` with the parsed query.

Cross-search Cloud Brain in parallel via `mcp__cloud-brain__search_notes` if user didn't explicitly limit to Apple. Merge results, dedup on title similarity, show source per result.

### Step 3: Rank and Filter

For each result:
- Score by keyword match relevance
- Boost recently-modified notes
- Filter by folder / date if specified
- Cap at `--limit` (default 20)

### Step 4: Render Results

For each result:
- Title
- Source (Apple Notes / Cloud Brain)
- Folder
- Last modified
- 1-line snippet (first non-trivial sentence or matched phrase)

If user wants to read one, use `mcp__apple_notes__read` and display.

### Step 5: Optional — Promote to Cloud Brain

If `--save-as-brain-note` or user says "save that to my brain":
1. `mcp__apple_notes__read` to get full content
2. `mcp__cloud-brain__write_note` to save:
   - **title:** Original Apple Note title
   - **folder:** based on Apple Notes folder mapping (e.g., Personal → `journal/`, Work → `projects/`)
   - **tags:** include `from-apple-notes` + any detected topics
3. Append a backreference in the Apple Note ("Saved to Cloud Brain {date}") if write permission exists

Don't delete the original Apple Note — keep it as the mobile capture surface.

## Data Structure

When promoting an Apple Note to Cloud Brain:

```markdown
# {Original Apple Note Title}

> **Source:** Apple Notes ({folder})
> **Originally Captured:** {YYYY-MM-DD}
> **Promoted to Cloud Brain:** {YYYY-MM-DD}

{Original note content, lightly cleaned — markdown formatting preserved if present}

---

> Captured on Apple Notes ({device hint if available}). Promoted via `/pro-apple-notes-search --save-as-brain-note`.
```

## Output Format (Chat)

```
🎯 APPLE NOTES SEARCH — "{query}"
{N} results ({A} from Apple Notes, {B} from Cloud Brain)

APPLE NOTES
1. {Title} — {Folder} — {date} — "{snippet}"
2. {Title} — {Folder} — {date} — "{snippet}"
...

CLOUD BRAIN
1. {Title} — {folder} — {date} — "{snippet}"
...

NEXT
- Read one: /pro-apple-notes-search --read "{title}"
- Promote to brain: /pro-apple-notes-search "{query}" --save-as-brain-note
```

## Example Usage

**User:** "Find my Apple Note about car wash deal underwriting"

**AI:** Searches both Apple Notes and Cloud Brain. Returns 3 Apple Notes + 2 Cloud Brain notes. Renders each with snippet.

**User:** "Promote that car wash note to my brain"

**AI:** Reads the chosen Apple Note. Writes it to Cloud Brain `knowledge-base/` with tags. Adds backreference.

**User:** "/pro-apple-notes-search 'mary birthday' --folder Personal --limit 5"

**AI:** Filters by folder + 5 results. Returns matching notes.

**User:** "What did I write down about Sam Lee this week?"

**AI:** Searches both stores with date filter (--since 7-days-ago). Returns recent captures.

## Error Handling

- **If `apple-notes-mcp` isn't installed:** Show install instructions. Offer Cloud Brain-only search as fallback: "I can search Cloud Brain only for now."
- **If macOS permission for Notes hasn't been granted:** Direct user to System Settings → Privacy → Automation, grant Claude Code (or Terminal) access to Notes.
- **If a note is encrypted (locked note):** Skip with note: "{N} encrypted notes matched but can't be read without unlock."
- **If query returns 0 results in both stores:** Suggest alternative terms. Note search heuristics: "Apple Notes does substring match — try a different keyword. Cloud Brain supports semantic search — try a more conceptual phrasing."
- **If user has thousands of notes and search is slow:** Note the volume. Suggest folder-scoped search.
- **If user wants to bulk-promote multiple notes:** Confirm count, then loop. Don't auto-promote large batches without confirmation — Cloud Brain will get noisy.
- **If a note has attachments (photos, sketches):** Note: "Note has {N} attachments — Apple Notes API can't transfer them. Promoted note will reference the original."
- **If user is on iCloud sync delay:** Notes created in last few minutes may not be searchable. Suggest waiting or pulling specifically.

## See Also

- `/pro-youtube-ingest` — pull YouTube content into Cloud Brain (same plugin)
- `/brain-recall` — Cloud-Brain-only search with smart fallbacks (from `brain-coach`)
- `/comm-plaud-sync` — sync PLAUD voice notes into Cloud Brain (from `communications`)
