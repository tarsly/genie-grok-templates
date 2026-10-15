---
name: pro-youtube-ingest
description: >-
  Ingest any YouTube video into Cloud Brain — pulls the transcript via yt-dlp, cleans the VTT, extracts a summary, quote-able moments, and chapters, and saves the full result as a markdown note. Use for podcasts, masterminds, conference talks, tutorials, or any video you want searchable in your second brain.
---

# YouTube Ingest

## Overview

Takes a YouTube URL and produces a Cloud Brain note with the full transcript, a structured summary, quote-able sentences, and chapter breakdown. Pulls the transcript using `yt-dlp` (which must be installed). Falls back to a transcript-only mode if subtitles aren't available (some videos require auto-generated subs).

Standard destinations:
- `transcripts/` — raw transcript + light cleanup
- `knowledge-base/` — when the video covers domain knowledge worth indexing
- `ideas/` — when watching for inspiration / capture mode

## When This Skill Applies

- User pastes a YouTube URL ("ingest this", "save this to my brain", "transcribe this")
- User mentions: youtube transcript, podcast transcript, save this video, watch later, transcribe
- User listened to a podcast or talk and wants searchable notes
- Plaud sync exists for personal audio — YouTube ingest is for public content

## Pre-Flight — Setup Check

Run this on every call:

```bash
which yt-dlp || echo "MISSING"
```

If missing, offer install instructions:

```
yt-dlp is not installed. Install via:
  brew install yt-dlp     # macOS (recommended)
  pipx install yt-dlp     # cross-platform

After install, re-run the command.
```

Also check `ffmpeg` for any audio-extraction features:

```bash
which ffmpeg || echo "MISSING"
```

Banner:
```
🎯 YouTube Ingest | yt-dlp: {version} | ffmpeg: {version}
```

## How It Works

### Step 1: Validate URL

Accept any YouTube URL form: `youtube.com/watch?v=`, `youtu.be/`, `youtube.com/shorts/`, `youtube.com/live/`. Extract the video ID.

### Step 2: Fetch Metadata

```bash
yt-dlp --skip-download --print-json "{url}"
```

Capture: title, channel, channel_id, upload date, duration, view count, description, chapter markers (if available).

### Step 3: Pull Transcript

Try in this order:

1. Manual subtitles (creator-uploaded): `--write-sub --sub-langs en --skip-download`
2. Auto-generated subtitles: `--write-auto-sub --sub-langs en --skip-download`
3. Fallback to audio download + Whisper if user has `whisper-cpp` or `openai-whisper` installed (`--extract-audio --audio-format mp3`)

Save VTT to a temp location.

### Step 4: Clean the VTT

Convert VTT to clean transcript:
- Remove timing tags
- De-duplicate the line-overlap pattern auto-gen subs produce
- Strip music notation tags `[Music]`
- Preserve paragraph breaks at natural pauses (long silences or speaker shifts in chapters)

### Step 5: Structure the Output

Generate sections:
- **Metadata block** (title, channel, date, duration, URL, tags)
- **Summary** — generated from transcript: 3-5 sentence overview + 5-10 bullet key points
- **Chapters** — if chapter markers exist, use them; otherwise extract topical breaks from transcript
- **Quote-able moments** — pull 3-7 sentences that stand alone and would be tweetable / shareable
- **Action items** — if the video is instructional, extract numbered steps or recommendations
- **Full transcript** — collapsed by default in the note (long content)

### Step 6: Save to Cloud Brain

Default folder logic:
- Educational / how-to → `knowledge-base/`
- Podcast / interview → `transcripts/`
- Inspirational / capture mode → `ideas/`
- Or use `--save-to-folder <folder>` to override

Title pattern: `{Channel} — {Video Title}`

Tags: `["youtube", "{channel slug}", "{topic guesses}"]`

### Step 7: Render Summary

Brief chat output (full content lives in the note).

## Data Structure

```markdown
# {Channel} — {Video Title}

> **URL:** {full URL}
> **Channel:** {channel name} ({channel URL})
> **Uploaded:** {YYYY-MM-DD}
> **Duration:** {HH:MM:SS}
> **Views:** {N}
> **Ingested:** {YYYY-MM-DD}
> **Source:** YouTube

## Summary

{3-5 sentence overview}

### Key Points

- {point}
- {point}
- {point}
- {point}
- {point}

## Chapters

| Timestamp | Topic |
|-----------|-------|
| 00:00 | {chapter title} |
| {ts} | {chapter title} |
| ... |

## Quote-Worthy

> "{sentence — speaker timestamp}" — {speaker if identifiable}, {timestamp}

> "{sentence}" — {timestamp}

> "{sentence}" — {timestamp}

## Action Items / Recommendations (if instructional)

1. {action}
2. {action}
3. {action}

## Full Transcript

<details>
<summary>Click to expand transcript</summary>

[Transcript content here, paragraphed]

</details>

## Original Description

{YouTube description as-is}

## Tags

{tags}
```

## Output Format (Chat)

```
✓ INGESTED — {Title}
{Channel} • {Duration} • {Upload date}

SUMMARY
{2-3 sentence summary}

TOP 3 QUOTES
1. "{quote}" — {timestamp}
2. "{quote}" — {timestamp}
3. "{quote}" — {timestamp}

ACTION ITEMS
1. {action}
2. {action}

Saved: {folder}/{Channel} — {Video Title}
```

## Example Usage

**User:** "Ingest https://youtu.be/abc123 — it's a Sam Lee mastermind"

**AI:** Validates URL. Pulls metadata + transcript. Saves to `knowledge-base/` (educational). Tags include "creative-finance", "real-estate", "mastermind". Renders summary.

**User:** "/pro-youtube-ingest https://youtube.com/watch?v=xyz --save-to-folder ideas"

**AI:** Explicit folder. Saves to `ideas/`. Useful for inspirational videos.

**User:** "This podcast episode — extract just the quotes"

**AI:** Ingests. Returns top 10 quote-worthy moments in chat with timestamps. Full transcript saved.

**User:** "I watched 5 videos today, ingest all of them" [pastes URLs]

**AI:** Loops through 5 URLs. Saves each as separate note. Final summary shows what was saved where.

## Error Handling

- **If `yt-dlp` is not installed:** Show install instructions. Don't attempt fallback — `yt-dlp` is the right tool.
- **If video is age-restricted or member-only:** Note: "Video requires sign-in / age verification. yt-dlp may need a cookie file: `yt-dlp --cookies-from-browser chrome`. Try that and re-run."
- **If no subtitles available (manual or auto-generated):** Offer Whisper transcription if available. Otherwise: "No subtitles and no Whisper installed. Try `brew install openai-whisper` or paste the transcript manually."
- **If video is a Short (<60 seconds):** Still works, but skip chapter extraction. Note shortness in the summary.
- **If video is a livestream (in progress):** Note: "Live streams need to end before transcription. Re-run after the stream is archived."
- **If transcript is very long (>3 hour podcast):** Save full transcript but truncate quote extraction (top 5-10). Add note: "Long transcript — full text in note; running summary on chunks."
- **If video is in a non-English language:** Use `--language` flag. Fall back to YouTube's auto-translate subs if needed. Note translation quality limitations.
- **If summary generation fails (transcript too short / too unclear):** Save transcript without summary. Note: "Summary skipped due to transcript quality. Read directly."
- **If user has `mcp__cloud-brain__write_note` but the file path conflicts with an existing note:** Append `(2)`, `(3)` suffix automatically. Don't overwrite.

## See Also

- `/comm-plaud-sync` — for personal voice recordings (from `communications`)
- `/pro-apple-notes-search` — search across both sources at once (same plugin)
- `/comm-meeting-transcript` — for meeting recordings rather than YouTube (from `communications`)
