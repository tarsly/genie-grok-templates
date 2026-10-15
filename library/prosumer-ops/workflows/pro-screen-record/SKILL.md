---
name: pro-screen-record
description: >-
  Record the macOS screen with system audio (the thing QuickTime alone can't do without a hack). Uses ScreenCaptureKit via a small helper binary (or the screencapture CLI for static captures). Supports full-screen, window-specific, and region recordings, with optional mic + system audio mix and configurable resolution / frame rate. Saves output ready for pro-screen-polish.
---

# macOS Screen Recorder

## Overview

ScreenCaptureKit (macOS 12.3+) is the modern API for high-quality recording with system audio. This skill orchestrates it via small helper binaries — either:

1. A user-installed Swift CLI (e.g., `aperture-cli`, `screencapturekit-cli`, or custom build)
2. `ffmpeg` with the AVFoundation backend (more flexible, less native)
3. `screencapture` macOS CLI for static screenshots / short clips

The skill checks what's installed, picks the best available, and orchestrates the recording. Output paths are predictable so `pro-screen-polish` can pick up the file directly.

## When This Skill Applies

- User asks to record their screen
- User mentions: screen recording, capture, demo recording, walkthrough video, system audio capture, screencast
- User wants to demo a workflow / pitch a feature / record a tutorial
- User says: "record this", "capture my screen", "record with sound"

## Pre-Flight — Setup Check

Run capability detection on every call:

```bash
# Check for native helpers
which aperture-cli 2>/dev/null || echo no-aperture
which screencapturekit-cli 2>/dev/null || echo no-sck-cli

# Fallback
which ffmpeg && ffmpeg -hide_banner -devices 2>&1 | grep -i avfoundation || echo no-ffmpeg-av
```

Choose the best available:
1. Preferred: `aperture-cli` or `screencapturekit-cli` (native ScreenCaptureKit)
2. Fallback: `ffmpeg` with AVFoundation (no system audio without virtual audio driver like BlackHole)
3. Static-only: `screencapture` (built-in, screenshots / short MOV without system audio)

If none of the helpers exist, offer setup:

```
For best quality (screen + system audio + mic):
  brew install aperture-cli           # or screencapturekit-cli

For ffmpeg fallback:
  brew install ffmpeg
  brew install blackhole-2ch          # virtual audio for system-audio capture

For static / short clips:
  screencapture is built into macOS.
```

Banner:
```
🎯 Screen Record | Tool: {tool detected} | System audio: {available/needs BlackHole}
```

## How It Works

### Step 1: Choose Recording Mode

Modes:
- `--mode full` — full display (or `--display 1/2` for multi-monitor)
- `--mode window` — specific window (interactive picker)
- `--mode region` — drag a region (interactive)

If `--mode` not specified, default to `full` and confirm.

### Step 2: Choose Audio Sources

- `--mic on` — capture default mic
- `--system-audio on` — capture system audio (requires ScreenCaptureKit or BlackHole)
- Both — mix mic + system audio into one track or keep separate tracks

If user wants system audio and tool can't do it natively, explain BlackHole setup.

### Step 3: Set Output Path

Default: `~/Movies/Recordings/{YYYY-MM-DD_HH-MM-SS}_{mode}.mov`

Override with `--output`.

Ensure parent directory exists.

### Step 4: Build the Command

Examples (the actual command depends on which tool was detected):

**Aperture CLI:**
```bash
aperture-cli record \
  --display 1 \
  --audio-device "Default" \
  --capture-cursor \
  --fps 60 \
  --output "{output_path}"
```

**ScreenCaptureKit CLI:**
```bash
screencapturekit-cli \
  --mode display \
  --audio system,mic \
  --fps 60 \
  --output "{output_path}"
```

**FFmpeg fallback:**
```bash
ffmpeg \
  -f avfoundation \
  -framerate 30 \
  -capture_cursor 1 \
  -i "1:0" \                      # display 1, default audio
  -c:v h264_videotoolbox \
  -b:v 8M \
  "{output_path}"
```

For region mode, capture screen coords from the user via osascript or skip to full mode if not interactive.

### Step 5: Run

Start the recording. Show running status with elapsed time.

If `--duration` set, stop automatically. If not, wait for user to send a stop command.

Best practice: warn user before starting that screen-recording permission must be granted in System Settings → Privacy → Screen Recording.

### Step 6: Save and Notify

After recording stops:
- Verify file exists and has nonzero size
- Run `ffprobe` (if available) to extract duration, resolution, audio channels
- Save a Cloud Brain note in `knowledge-base/recordings/` with metadata + path

### Step 7: Hand-Off

Suggest next step:
- `/pro-screen-polish "{path}" --vertical` to auto-zoom + caption + vertical export
- Or open in QuickTime / Descript / DaVinci

## Data Structure

```markdown
# Recording — {timestamp}

> **Captured:** {YYYY-MM-DD HH:MM:SS}
> **Tool:** {tool}
> **Path:** `{path}`

## Specs

| Field | Value |
|-------|-------|
| Duration | {HH:MM:SS} |
| Resolution | {WxH} |
| FPS | {N} |
| Mode | {full / window / region} |
| Audio tracks | {mic / system / both / none} |
| File size | {MB} |

## Use For

- {what user said the recording is for, if mentioned}

## Next Steps

- Polish: `/pro-screen-polish "{path}" --vertical`
- Trim: open in QuickTime
- Transcribe + edit: Descript
- YouTube upload: `/zoom-content-pipeline` or direct
```

## Output Format (Chat)

```
🎬 RECORDING — {timestamp}
Tool: {tool} • Mode: {mode} • Duration: {duration} • Audio: {tracks}

FILE
{path}
{size} • {WxH}@{fps}fps

NEXT
- Polish (auto-zoom + captions + vertical): /pro-screen-polish "{path}"
- Open in QuickTime: open -a "QuickTime Player" "{path}"

Saved metadata: knowledge-base/recordings/Recording — {timestamp}
```

## Example Usage

**User:** "Record my screen for 60 seconds with system audio"

**AI:** Detects aperture-cli. Confirms permissions. Records full screen + system audio for 60s. Saves to `~/Movies/Recordings/`. Suggests polish step.

**User:** "/pro-screen-record --mode window --mic on"

**AI:** Interactive window picker. Records with mic only. Waits for stop command.

**User:** "Record a demo of the new Genie plugin — I'll narrate"

**AI:** Full screen, mic on, system audio on (so any sounds the demo makes are captured), 1080p, 30fps. Confirms ready. Starts on user signal.

**User:** "Quick screenshot of my whole screen"

**AI:** Uses built-in `screencapture` — no need for helper. Saves PNG.

## Error Handling

- **If no screen-recording permission:** Direct: "System Settings → Privacy & Security → Screen Recording → enable Claude Code (or your Terminal). Restart Claude Code after."
- **If no recording tool is installed:** Show install options. Recommend `aperture-cli` as easiest.
- **If user wants system audio but no native helper + no BlackHole:** Two-step setup — install BlackHole, create Multi-Output Device in Audio MIDI Setup. Provide link to setup guide.
- **If disk is low on space:** Check before starting. Recordings are large (~100MB/min at 1080p60).
- **If user wants 4K but the display is 1080p:** Recording can't exceed source resolution.
- **If multiple monitors:** Default to primary; show how to switch with `--display 2`.
- **If recording fails partway (crash, killed):** Some tools leave a partial file. Check size + try playback. If unplayable, suggest re-recording with shorter duration / lower bitrate.
- **If user requested `--duration` but command exits early:** Check process; may be a permission issue.
- **If on Apple Silicon vs. Intel:** `h264_videotoolbox` is faster on Apple Silicon. ffmpeg fallback should pick that automatically. Note in performance hints.

## See Also

- `/pro-screen-polish` — auto-zoom, captions, vertical export on the resulting file (same plugin)
- `/comm-meeting-transcript` — for already-recorded Zoom / meeting files (from `communications`)
