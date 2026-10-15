---
name: pro-screen-polish
description: >-
  Screen-Studio-style auto-polish for screen recordings — auto-zoom on cursor clicks, idle-time speed-up, keystroke overlay chips, auto-captions from transcript, intro/outro cards, background blur for window mode, and 9:16 vertical export for Shorts/Reels/TikTok. Uses ffmpeg + a transcript source (Whisper or YouTube-style auto-sub) to produce a polished MP4.
---

# Screen Polish

## Overview

Mimics the Screen Studio (and similar paid tools) feature set using `ffmpeg` + a transcript engine + a small Python or shell coordinator. The skill focuses on the highest-leverage polish steps:

1. **Auto-zoom on click clusters** — when the cursor clicks rapidly in one area, zoom in for emphasis
2. **Idle speed-up** — long static segments (no cursor movement, no audio) play faster
3. **Keystroke overlay chips** — visualize key presses as floating chips in the corner
4. **Auto-captions** — burnt-in or sidecar SRT/VTT from Whisper transcription
5. **Vertical export (9:16)** — crop to mobile-first aspect, keeping cursor focus in frame
6. **Intro/outro cards** — branded text cards (uses brand-toolkit if installed)
7. **Background blur** — when recording a single window, blur surrounding desktop

Output is an MP4 ready for upload.

## When This Skill Applies

- User has a screen recording (from `pro-screen-record` or any source) and wants it polished
- User wants vertical / Shorts / Reels / TikTok export
- User mentions: Screen Studio, polish my recording, auto-zoom, vertical crop, captions, screen capture editing
- User says: "polish this", "make it pretty", "ready for upload"

## Pre-Flight — Setup Check

Required:
- `ffmpeg` (with libx264 + libfreetype)
- Either `whisper` (openai-whisper or whisper-cpp) OR an existing `.srt` / `.vtt` sidecar OR `--no-captions`

Optional:
- `imagemagick` for fancy intro/outro card rendering
- Python with `numpy` + `opencv-python` for click-cluster detection (or pure-ffmpeg if not available)

If `ffmpeg` is missing: `brew install ffmpeg` and re-run.

If captions requested but no transcription tool: `brew install openai-whisper` or supply sidecar.

Banner:
```
🎯 Screen Polish | ffmpeg: {version} | Whisper: {available/missing} | OpenCV: {y/n}
```

## How It Works

### Step 1: Analyze Input

Run `ffprobe` on the input to get:
- Resolution
- Frame rate
- Duration
- Has audio? How many channels?

Reject if file doesn't exist or has zero duration.

### Step 2: Detect Click Clusters (if --auto-zoom on)

Two options:
- **Heuristic via OpenCV** (if installed) — detect cursor position frame-by-frame, find clusters of nearby clicks (proxy via fast movement + brief stillness)
- **Brute-force ffmpeg motion analysis** — `ffmpeg -i input -vf "select=gt(scene,0.1)"` to detect significant frame changes
- **User-supplied click markers** — `--clicks "10,12,45,46"` (timestamps in seconds)

Output: list of zoom moments with timestamp + bounding box.

### Step 3: Detect Idle Segments (if --speed-up-idle)

- Audio analysis via `ffmpeg -af silencedetect` for silent ranges
- Motion analysis for static visual ranges
- Intersection = idle
- Apply speed-up (default 4x) to idle segments > 3s

### Step 4: Transcribe (if --captions on)

- If `.srt` exists alongside input → use it
- Else if Whisper available → run transcription
- Else: skip captions, note in output

### Step 5: Build the ffmpeg Pipeline

A single (or piped) ffmpeg invocation that:
- Applies zoom-in segments (with smooth zoom transitions via `zoompan` filter)
- Applies tempo changes for idle segments (`atempo` for audio, `setpts` for video)
- Burns in captions (`subtitles=filename.srt:force_style='...'`) OR uses sidecar VTT
- Crops + scales to 9:16 (1080x1920) if `--vertical` — keep cursor/click center in frame
- Adds intro / outro cards (generated as PNGs, concatenated)
- Background blur for window-mode (if metadata flags window mode)

Output codec: H.264 (libx264) or HEVC (libx265) per user preference, `-crf 18` for high quality.

### Step 6: Save

Output path:
- Default: same dir as input, suffix `_polished.mp4` (or `_vertical.mp4` if vertical)
- Override with `--output`

Save Cloud Brain note in `knowledge-base/recordings/` with input + output + edit summary.

### Step 7: Render Summary

Show the user what was applied.

## Data Structure

```markdown
# Polished — {input filename} — {YYYY-MM-DD}

> **Polished:** {YYYY-MM-DD HH:MM}
> **Input:** `{input path}`
> **Output:** `{output path}`

## Polish Applied

| Feature | Status | Details |
|---------|--------|---------|
| Auto-zoom on click clusters | {on/off} | {N zoom moments detected} |
| Speed-up idle | {on/off} | {N segments, ratio {X}x} |
| Captions | {burnt-in / sidecar / off} | {language} |
| Vertical (9:16) | {yes/no} | {1080x1920} |
| Intro card | {yes/no} | "{intro text}" |
| Outro card | {yes/no} | "{outro text}" |
| Background blur | {yes/no} | {window-mode source} |

## Render Details

- Codec: {h264 / hevc}
- CRF: {N}
- Output size: {MB}
- Output duration: {HH:MM:SS} ({reduction from {input duration}})

## Next Steps

- Upload to YouTube: `gh youtube upload "{output}"` (if `gh-youtube` extension installed)
- Upload to TikTok / Reels: open Finder → drag to app
- Further edit: open in Descript / DaVinci / CapCut
```

## Output Format (Chat)

```
🎬 POLISH — {input filename}

APPLIED
{checklist}

OUTPUT
{path}
{duration} • {size} • {resolution}

TIME SAVED
Input duration: {X} • Output duration: {Y} • Saved: {Z}

NEXT
- Upload: drag to YouTube / TikTok / etc.
- Further edit: open in Descript

Metadata: knowledge-base/recordings/Polished — {input} — {date}
```

## Example Usage

**User:** "Polish my latest recording — make it vertical for TikTok"

**AI:** Identifies most recent file in `~/Movies/Recordings/`. Applies all defaults (auto-zoom, speed-up idle, captions, vertical). Outputs `_vertical.mp4`. Saves metadata.

**User:** "/pro-screen-polish ~/Movies/Recordings/demo.mov --captions off --intro 'Genie Demo' --outro 'the provider'"

**AI:** Specific input. No captions. Branded intro/outro from `brand-toolkit` if installed (detected via `brain/brand/brand-kit` Cloud Brain note), else neutral text cards. Saves.

**User:** "Just speed up the idle parts — don't crop or caption"

**AI:** Detects idle ranges. Applies 4x speed-up only. Keeps original aspect. Faster ffmpeg run.

**User:** "Polish for YouTube Shorts — 60 seconds max"

**AI:** Vertical + caps duration at 60s (trim from the input, default keep first 60s; warn if input is longer and ask). Saves.

## Error Handling

- **If input file doesn't exist:** Error. Ask for the correct path.
- **If input has no audio and captions requested:** Skip captions, note: "No audio track — captions skipped."
- **If Whisper isn't installed and captions requested without sidecar:** Offer install or fallback to `--captions off`.
- **If ffmpeg is missing:** Hard requirement. Install instructions.
- **If output already exists:** Append `(2)`, `(3)` suffix. Don't overwrite.
- **If auto-zoom over-zooms (jumpy, choppy):** Reduce sensitivity automatically OR offer manual control via `--clicks` markers.
- **If vertical crop loses cursor / important UI:** Use cursor tracking to recenter. Note in summary: "Vertical crop centered on cursor — manually edit if UI was clipped."
- **If render is very slow:** Note duration estimate before starting. Recommend HEVC for smaller / slower or H.264 for faster / larger.
- **If brand-toolkit isn't installed and user wants branded intro/outro:** Use neutral text cards. Note: "Install `brand-toolkit` for branded intro/outro with your colors and logo."
- **If user wants 4K output from 1080p input:** Refuse upscale — note quality loss. Use original resolution.

## See Also

- `/pro-screen-record` — record the screen first (same plugin)
- `/brandtoolkit-brand-kit` — provides the colors and logo for intro/outro cards (from `brand-toolkit`)
- `/sm-content-batch` — schedule the polished video for social distribution (from `social-media-manager`)
