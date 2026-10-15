---
name: research-person
description: >-
  Individual due diligence — research any person before a sales call, investor meeting, partnership discussion, hiring decision, or collaboration. Builds a profile from web and social sources: background, company, recent activity, credibility signals, conversation starters, and red flags.
---

# Person Research — Individual Due Diligence

## Overview

Researches any individual to prepare you for a high-stakes interaction: a sales call, investor pitch, partnership discussion, hiring decision, or collaboration. Pulls background, career history, company context, recent public activity, credibility signals, and potential red flags from web and social sources. Delivers a concise, actionable brief — who this person is, what matters to them, how to make a strong impression, and anything you should watch out for.

## When This Skill Applies

- User says "research [name]" or "look up [name]" before a meeting
- User says "who is [name]?" in a business context
- User says "prep me for a call with [name]"
- User is about to pitch an investor and wants to know their thesis
- User is meeting a potential partner and wants background
- User is considering hiring someone and wants a quick profile
- User says "what do I know about [name]?" or "find out about [name]"
- User wants conversation starters for a specific person
- User wants to know if a potential business contact is credible

## How It Works

### Step 1: Gather Context

Determine from the user's request:

| Input | Required | Notes |
|-------|----------|-------|
| Full name | Yes | Confirm spelling |
| Company or role | Helpful | Narrows search significantly |
| Context | Yes | Sales / Investor / Partner / Hire / Other |
| Any URL | Optional | LinkedIn, website, or company page |
| What user wants from this interaction | Helpful | "Close a deal" / "Pitch the company" / "Evaluate fit" |

Check cloud brain first — use `mcp__cloud-brain__search_notes` with the person's name. If a prior profile exists, surface it and ask: "I have notes on {name} from {date}. Update the profile with fresh data, or use what's saved?"

### Step 2: Research the Person

Run targeted web searches:

**Core searches:**
1. `"{full name}" "{company}"` — Identity confirmation and current role
2. `"{full name}" LinkedIn` — Professional background
3. `"{full name}" interview OR podcast OR keynote` — Public statements, thought leadership, viewpoints
4. `"{full name}" news 2025 2026` — Recent activity and announcements
5. `"{full name}" "{industry}"` — Industry presence and reputation

**Context-specific searches:**

*Investor context:*
- `"{name}" portfolio investments` — What they've backed
- `"{name}" thesis OR "what I look for"` — Their stated investment criteria
- `"{name}" check size` — Investment range

*Sales context:*
- `"{name}" "{their company}" challenges OR goals 2026` — Pain points to address
- `"{their company}" news 2026` — Company priorities and recent changes

*Partner context:*
- `"{name}" "{their company}" partnerships` — Prior deal patterns
- `"{their company}" competitors` — Shared or competing interests

*Hiring context:*
- `"{name}" skills OR expertise` — Domain credibility
- `"{name}" "{prior companies}"` — Career trajectory and tenure

### Step 3: Build the Profile

Compile findings into a structured brief:

| Field | What to Find |
|-------|-------------|
| Full Name + Title | Confirmed current role and company |
| Background | Career in 2-3 sentences — where they came from, what they've built |
| Current Company | What the company does, size/stage, recent news |
| Notable | What they're known for — awards, media mentions, major deals, thought leadership |
| Online Presence | LinkedIn, Twitter/X, website, podcast, YouTube |
| Recent Activity | Last 60-90 days: posts, news, announcements, public statements |
| Stated Priorities | What they're focused on right now (from recent interviews, posts, company news) |
| Relevance to You | Specific reason this person matters for the user's goal |
| How You Can Help Them | What the user can offer based on the person's apparent priorities |
| Conversation Starters | 3 specific openers grounded in recent activity — not generic |
| Questions to Ask | 3 questions that show genuine interest and advance the user's goal |
| Red Flags | Anything that warrants caution: past disputes, poor reputation signals, competitive conflicts |

### Step 4: Compile the Brief

```markdown
# Person Brief: [Full Name]

> **Prepared:** {date}
> **Context:** {sales / investor / partner / hire}
> **Your Goal:** {what you want from this interaction}

---

## Profile

| | |
|---|---|
| **Name** | [Full Name] |
| **Title** | [Current title] |
| **Company** | [Company — what it does in one sentence] |
| **Location** | [City, if relevant] |
| **LinkedIn** | [URL if found] |

## Background
[2-3 sentences: where they came from, major career milestones, what they've built]

## What They're Known For
[1-2 sentences: their reputation, notable achievements, public presence]

## Recent Activity (Last 90 Days)
- [Notable post, announcement, interview, or news item]
- [Second item]
- [Third item]

## Their Current Priorities
[Based on recent activity and company news — what are they focused on right now?]

---

## For Your Meeting

### How You're Relevant to Them
[Specific connection between what you offer and what they appear to care about]

### Conversation Starters
1. "[Specific opener referencing their recent activity]"
2. "[Opener referencing their company or role]"
3. "[Opener based on a shared interest or mutual context]"

### Questions to Ask
1. [Question that shows you've done your homework and advances your goal]
2. [Question about their priorities — shows you're thinking about their needs]
3. [Question that opens the door to your offer or pitch]

### Your Ask (suggested)
[Based on the context — what is the specific, one-sentence ask for this interaction?]

---

## Red Flags
[Anything to be aware of — competitive conflicts, reputation signals, deal-breaker patterns. "None found" if clean.]

---

## Sources
- [Source](URL) — accessed {date}

---

*Profile compiled by the provider AI. Verify details before the meeting — titles and roles change.*
```

### Step 5: Save to Cloud Brain

Save using `mcp__cloud-brain__write_note`:
- `--title` "person-{name-slug}"
- `--folder` "brain/people"
- `--tags` "person, research, {context}"

This makes the profile retrievable for future interactions with the same person.

### Step 6: Report to User

- Deliver the brief in chat
- Call out the single most useful piece of intel ("The most important thing to know before this meeting is…")
- Flag any red flags prominently if found
- Note where the profile was saved for future reference

## Quality Standards

1. **Only report what was actually found.** Never fabricate claims about a person.
2. **Distinguish public from private.** Only source publicly available information.
3. **Be accurate on current role.** People change jobs. Verify the current title is current.
4. **Flag uncertainty.** If a search result is ambiguous (wrong person with the same name), say so.
5. **Conversation starters must be specific.** "I saw your post about X" beats "I hear you're in real estate."
6. **Red flags are facts, not speculation.** Only flag something if there's a credible source behind it.

## Example Usage

**User:** "Research John Martinez at Acme Capital before my pitch tomorrow"

**AI executes:**
- Searches for John Martinez + Acme Capital to confirm identity and role
- Finds his investment thesis from a recent podcast interview
- Notes Acme Capital recently closed a fund focused on B2B SaaS
- Identifies 3 conversation starters tied to his recent LinkedIn posts
- Flags he has a stated preference for revenue-generating companies (not pre-revenue)
- Saves profile to cloud brain (folder: brain/people)

**User:** "Who is Sarah Kim at TechBridge Partners? I'm meeting her next week."

**AI executes:**
- Researches Sarah Kim — finds she's a founding partner, backgrounds in enterprise software
- Notes she co-authored an article on AI in supply chain last month
- Builds 3 openers referencing that article and her stated focus area
- Delivers brief with suggested ask framed around her stated thesis

## Error Handling

- **If the person cannot be found online:** Report honestly: "I couldn't find reliable public information on {name}. They may have a low online presence. If you have their LinkedIn URL, company, or any other details, share them and I'll look deeper. In the meantime, here are 3 generic but effective openers for a cold meeting."
- **If multiple people with the same name are found:** Surface the ambiguity: "I found {X} people named {name}. Can you confirm which one? Here's what I found: [Option A — title/company] / [Option B — title/company]."
- **If the person has a controversial public reputation:** Report factually with sources. Do not editorialize. Let the user decide how to factor it in.
- **If the context is hiring:** Add a note: "This is a web-based profile only. For formal employment decisions, use this as a starting point only — formal background checks and reference checks are the standard for hiring."
- **If no context is given:** Default to sales/meeting prep. Ask at the end: "I've built this as a general meeting prep brief — want me to reframe it for a specific goal?"
