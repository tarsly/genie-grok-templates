---
name: eos-health-check
description: >-
  Run the EOS Organizational Checkup — the canonical 20-question self-assessment that scores your company 0-100% across the Six Key Components (Vision, People, Data, Issues, Process, Traction). Tracks score over time, surfaces the lowest-scoring questions, and tells you exactly where to focus next. Target: 80%+ = 'Strong on EOS'. Use for taking the checkup, viewing your score history, or seeing the component breakdown.
---

# EOS Organizational Checkup — How Strong Are You On EOS?

## Overview

The EOS Organizational Checkup is the canonical 20-question self-assessment. Each question maps to one of the Six Key Components (Vision, People, Data, Issues, Process, Traction). Each is rated 1-5. The composite score is normalized to 0-100%. The EOS benchmark: **80%+ is "Strong on EOS"**; below 80% means there's a specific component to strengthen next. This skill runs the assessment, tracks score history over time, and surfaces the lowest-scoring questions so the team knows exactly where to focus.

## When This Skill Applies

- User says "EOS checkup", "health check", "how strong are we on EOS"
- User asks "what's our weakest area", "what should we strengthen next"
- `eos-quarterly-pulsing` calls this before each quarterly
- `eos-annual-planning` references the current score
- User invokes `/eos-health-check`

## Pre-Flight — Onboarding + Preferences

In parallel:

1. `search_notes "business blueprint"`, `"eos company preferences"`
2. `search_notes "EOS Health Check"` — prior assessments for trend

**Banner:**
```
🩺 EOS Health Check | {Company} | Last Check: {date or "never"} | Last Score: {N}%
```

## How It Works

### Step 0: Determine Action

Parse to: `take`, `view`, `history`, `focus`. Default: `view` if a recent check exists, `take` otherwise.

### Step 1: Take — 20 Questions Across 6 Components

Walk all 20 questions. Each: rate 1-5 (1 = strong disagree, 5 = strong agree). Ask in groups of 3-4 per message to keep momentum.

**The 20 questions (canonical):**

#### Vision (3 questions)
1. We have a clear vision in writing that has been properly communicated and is shared by everyone.
2. Our core values are clear, and we are hiring, reviewing, rewarding, and firing around them.
3. Our Core Focus (Purpose/Cause/Passion + Niche) is clear, and our business activities are aligned with it.

#### People (3 questions)
4. Our leadership team is open and honest, demonstrating a high level of trust.
5. Everyone is the right person in the right seat (Right People, Right Seats).
6. Our Accountability Chart is clear, complete, and constantly updated.

#### Data (3 questions)
7. We have a scorecard for tracking weekly metrics/measurables.
8. Everyone has at least one number they're accountable for.
9. We have effective measurables that give us a true pulse on the business.

#### Issues (3 questions)
10. Our Issues List is visible, and we are good at solving issues as they arise.
11. We use IDS (Identify, Discuss, Solve) to truly resolve issues.
12. We have a high-trust, open, and honest culture (people speak up).

#### Process (3 questions)
13. Our core processes are documented, simplified, and followed by all (FBA).
14. We've simplified all of our processes to be Followed By All.
15. We've systemized and packaged "our way" of doing business.

#### Traction (5 questions)
16. We have a meeting pulse — annual, quarterly, weekly, and where applicable, daily.
17. Each meeting (Annual, Quarterly, Weekly, Daily) is on the same day and time, has the same agenda, starts and ends on time.
18. All meetings are well-run and result in clarity, decisions, and action.
19. We set, track, and accomplish Quarterly Rocks.
20. Everyone in our organization is "All In" — they like working here, they understand the vision, and they're contributing.

For each, capture rating + (optional) one-sentence note.

### Step 2: Score

- Sum all 20 ratings (max 100)
- Convert to percentage: `(sum / 100) * 100`
- Per-component score: `(component sum / (5 * questions in component)) * 100`

### Step 3: Surface Findings

- Overall % vs 80% benchmark
- Per-component % — render as horizontal bar chart
- Lowest-scoring 3 questions
- Recommended focus: weakest component → which `eos-*` skill or Rock could move it

### Step 4: Save

`mcp__cloud-brain__write_note`:
- **title:** `EOS Health Check — {YYYY-MM-DD}`
- **folder:** `eos`
- **tags:** `["eos","health-check"]`

### Step 5: History / Trend

When user says "history" or "trend":
- Read all prior `EOS Health Check` notes
- Render trend: date → overall % → per-component %
- Sparkline of overall score

### Step 6: Publish (optional)

On `--publish`:
- `taskName: "eos-health-check"`
- `category: "research"`
- `content`: rendered checkup with the six-component bar chart, overall %, lowest 3 questions, focus recommendation
- `summary`: *"EOS Health: {pct}% (target ≥80%). Weakest: {Component} at {pct}%. Strongest: {Component} at {pct}%."*

## Data Structure

`EOS Health Check — {YYYY-MM-DD}` note:

```markdown
# EOS Health Check — {YYYY-MM-DD}

> **Date:** {YYYY-MM-DD}
> **Overall:** {pct}%  ({rating} of 100)
> **Trend (vs last):** {↑/↓/→} {delta points}

## Component Breakdown

| Component | Score | Bar | Notes |
|-----------|-------|-----|-------|
| Vision    | 80%   | ████████░░ | Core Focus clear; vision shared |
| People    | 60%   | ██████░░░░ | GWC gaps on 2 leaders |
| Data      | 55%   | █████▌░░░░ | Scorecard not yet weekly |
| Issues    | 75%   | ███████▌░░ | IDS use is solid |
| Process   | 40%   | ████░░░░░░ | Only 3 of 6 documented |
| Traction  | 70%   | ███████░░░ | Rocks set; L10 still <8 |

## Lowest-Scoring 3 Questions
1. Q13 (Process — FBA): 2/5
2. Q15 (Process — packaged): 2/5
3. Q9 (Data — effective measurables): 2/5

## Focus Recommendation
Strengthen **Process**. Highest-leverage next step:
  • Run `/eos-process-docs document --process HR` (highest-leverage missing core process)
  • Schedule annual review of existing process docs

## Q-by-Q Ratings
1. Q1 Vision (in writing): 4/5 — "V/TO clear but cascading message missed last 2 weeks"
2. Q2 Core Values: 5/5 — "Used in hiring + reviews"
…
20. Q20 All In: 4/5 — "1 leader's engagement is shaky"
```

## Output Format

### Take

```
🩺 EOS Health Check | {Company} | {YYYY-MM-DD}

OVERALL: 63%  ⚠️  (target ≥80%)   Trend: ↓ 4 points vs Q2

COMPONENT BREAKDOWN
  Vision    ████████░░  80% ✅
  People    ██████░░░░  60% ⚠️
  Data      █████▌░░░░  55% ⚠️
  Issues    ███████▌░░  75% ✅
  Process   ████░░░░░░  40% ❌  ← weakest
  Traction  ███████░░░  70% ⚠️

LOWEST 3 QUESTIONS:
  Q13 — Process (FBA): 2/5
  Q15 — Process (packaged): 2/5
  Q9  — Data (effective measurables): 2/5

NEXT FOCUS: Process. Highest-leverage actions:
  • /eos-process-docs document --process HR
  • Annual review of Marketing and Accounting process docs (both overdue)

History trend:
  2026-01-15: 58%
  2026-04-20: 65%
  2026-07-10: 67%
  2026-10-12: 63%  ← today (regression on Process)
```

## Example Usage

**User:** "Run the EOS checkup."
**AI:** Walks all 20 questions (in groups of 3-4). Computes scores. Saves. Renders.

**User:** "What's our weakest area?"
**AI:** Reads most recent. Returns the weakest component + the 3 specific questions driving it.

**User:** "Health check history."
**AI:** Renders trend line of overall % across all prior checkups.

**User:** "/eos-health-check take --publish"
**AI:** Runs the assessment, saves, publishes to dashboard.

**User:** "What component is strongest?"
**AI:** Reads most recent, returns highest-scoring component with its driving questions.

## Error Handling

- **No prior checkup:** Take fresh. Note that trend comparison isn't available yet.
- **All ratings 5/5:** Push back: *"All-5s are rare — most teams have at least one area where they know they could be sharper. Want to revisit a few?"*
- **All ratings 1-2/5:** Acknowledge but reframe: *"Tough self-assessment — that's good. EOS is a multi-year build. Focus on one component."*
- **User wants to skip a question:** Allow; mark as `N/A` and exclude from denominator.
- **Score drops >15 points from prior:** Surface: *"Big drop from last checkup. Want to IDS the regression at next leadership L10?"*
- **No prior to compare to** but user asks for trend: Show only this check; explain history begins.
- **Dashboard publish fails:** Save the checkup, warn about dashboard.
