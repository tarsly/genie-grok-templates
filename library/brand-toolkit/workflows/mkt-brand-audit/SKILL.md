---
name: mkt-brand-audit
description: >-
  Brand audit — analyze online presence across social media, website, and search. Gap analysis, competitor positioning, content pillar recommendations, bio and tagline suggestions, 30-day improvement plan, or wants a data-backed assessment of their online presence and a 30-day improvement plan.
---

# MKT-brand-audit — Comprehensive Brand Intelligence

## Overview

Performs a full analysis of a personal or business brand across digital channels. Examines social media presence, website, search visibility, content strategy, messaging consistency, and competitive positioning. Output includes a brand snapshot, gap analysis, competitive comparison, 30-day improvement plan, content pillar recommendations, and bio/tagline suggestions.

This is the difference between *hoping* your brand works and *knowing* exactly where it stands and what to fix.

---

## Pre-Flight — Client Preferences

1. Search Cloud Brain: `mcp__cloud-brain__search_notes` with query `"MKT preferences"`
2. **If found:** confirm in ONE line — *"Auditing [Name]'s brand at [Business], correct?"*
   - If yes: proceed with saved identity, target audience, platforms, and goals
   - If no: ask what changed, offer to update brand kit, proceed
3. **If not found:** ask the six context questions in Step 1 below. After the audit, offer to save these as a brand kit: *"Want me to save your brand details so you don't have to re-enter them next time? Just say 'set up my brand kit.'"*
4. **Show preferences banner at top of output:**
   > *Brand Audit for: [Name] | [Business] | [Industry] | Platforms: [list] | Goals: [goals]*

**Identity preferences loaded from brand kit (do not re-ask if found):**
- Full name, business name, industry/niche
- Target audience
- Primary platforms
- Brand goals

---

## When This Skill Applies

- User says "audit my brand" or "brand audit"
- User says "personal brand audit" or "review my online presence"
- User says "how does my brand look?" or "brand health check"
- User says "social media audit" or "audit my profiles"
- User says "brand gap analysis" or "brand consistency check"
- User says "improve my brand" or "brand improvement plan"
- User says "brand positioning" or "how am I perceived online?"
- User says "30-day brand plan" or "brand strategy"
- User says "content pillars" or "content strategy review"
- User says "bio suggestions" or "tagline suggestions"
- User says "brand snapshot" or "digital presence review"
- User mentions wanting to strengthen, refresh, or understand their brand

---

## Step 1: Gather Brand Context

Determine from the user's request or loaded preferences:

1. **Who:** The person or business being audited
2. **Handles/URLs:** Social media handles, website URL, any online profiles
3. **Industry/Niche:** What space are they in?
4. **Target Audience:** Who are they trying to reach?
5. **Current Goals:** What is the brand supposed to accomplish? (lead generation, thought leadership, deal flow, community building, product sales)
6. **Self-perception:** How does the user think their brand is perceived vs. how they *want* it to be perceived?

**Check for prior audit:**
- Search: `mcp__cloud-brain__search_notes` with query `"brand-audit"`
- If a prior audit exists, note the date and overall score
- Flag: *"A prior brand audit exists from [date] with an overall score of [X/100]. I'll compare scores and highlight what has improved or regressed."*

---

## Step 2: Audit the Digital Presence

Research each channel where the subject has a presence.

### Website Audit
- Search for the website and fetch key pages
- Evaluate: messaging clarity, value proposition, design impression, CTA strength
- Note: Is the website doing its job? Does it convert visitors into action-takers?

### Social Media Audit

For each active platform (Instagram, TikTok, YouTube, Twitter/X, Facebook, LinkedIn, Threads), research:

| Audit Point | What to Evaluate |
|---|---|
| **Profile Setup** | Photo, bio, handle consistency, link in bio, highlights/pinned content |
| **Content Quality** | Visual consistency, writing quality, value delivery, professionalism |
| **Posting Frequency** | How often? Is it consistent? |
| **Engagement** | Comments, likes, shares relative to follower count |
| **Content Mix** | Educational vs. promotional vs. personal vs. entertainment ratio |
| **Audience Fit** | Does the content attract the RIGHT audience for their goals? |
| **Calls to Action** | Are they driving followers somewhere? |
| **Messaging Consistency** | Does the brand message match across platforms? |
| **Visual Identity** | Color consistency, font usage, photo style, brand cohesion |
| **Bio/Tagline** | Clear, compelling, tells people what you do and why they should care |

Per platform output:

```markdown
### [Platform Name]

**Handle:** @[handle]
**Followers:** [count if discoverable]
**Posting Frequency:** [X posts per week/month]
**Content Focus:** [What they mostly post about]

**Strengths:**
- [What is working]

**Weaknesses:**
- [What needs improvement]

**Score:** [X/10]
**Priority Fix:** [The single most impactful change for this platform]
```

### Search Presence Audit
- Search the user's name and brand name
- What comes up first? (LinkedIn, website, social, press, nothing?)
- Any negative or outdated content?
- Showing up for relevant keywords?
- Note the "digital first impression" — what does someone see when they Google this person?

---

## Step 3: Build the Brand Snapshot

```markdown
## Current Brand Snapshot

### Who You Are (Based on What the Internet Says)
[2–3 paragraph honest assessment of how the brand currently reads to a stranger. This is not what they WANT to be perceived as — this is what actually comes through.]

### Brand Identity Summary
| Element | Current State | Assessment |
|---|---|---|
| **Core Message** | [What their brand is currently saying] | [Clear / Unclear / Missing] |
| **Value Proposition** | [What value they promise] | [Strong / Weak / Nonexistent] |
| **Target Audience** | [Who their content speaks to] | [Well-defined / Vague / Confused] |
| **Visual Identity** | [Logo, colors, style consistency] | [Cohesive / Inconsistent / Missing] |
| **Tone of Voice** | [How they communicate] | [Consistent / Varies / Undefined] |
| **Credibility Signals** | [Proof, testimonials, results, press] | [Strong / Moderate / Weak] |
| **Differentiation** | [What makes them different] | [Clear / Unclear / Generic] |

### Platform Presence Overview
| Platform | Active? | Score | Top Issue |
|---|---|:---:|---|
| Website | [Yes/No] | [X/10] | [Issue] |
| Instagram | [Yes/No] | [X/10] | [Issue] |
| TikTok | [Yes/No] | [X/10] | [Issue] |
| YouTube | [Yes/No] | [X/10] | [Issue] |
| LinkedIn | [Yes/No] | [X/10] | [Issue] |
| Facebook | [Yes/No] | [X/10] | [Issue] |
| Twitter/X | [Yes/No] | [X/10] | [Issue] |
| Threads | [Yes/No] | [X/10] | [Issue] |
| Google Search | — | [X/10] | [Issue] |

### Overall Brand Score: [X/100]
```

**If a prior audit exists:** Add a comparison row:

```markdown
### Progress Since Last Audit ([Prior Date])
| Dimension | Prior Score | Current Score | Change |
|---|:---:|:---:|:---:|
| Overall | [X/100] | [X/100] | [▲/▼ N points] |
| [Platform] | [X/10] | [X/10] | [▲/▼ N] |
```

---

## Step 4: Gap Analysis

```markdown
## Gap Analysis

### Perception vs. Reality

| Dimension | Where You Are | Where You Need to Be | Gap Size |
|---|---|---|:---:|
| **Authority** | [Current level of perceived expertise] | [Where your goals require you to be] | [Small/Medium/Large] |
| **Visibility** | [Current reach and discovery] | [Required reach for goals] | [Small/Medium/Large] |
| **Trust** | [Current credibility signals] | [What your audience needs to see] | [Small/Medium/Large] |
| **Clarity** | [How clear your message is now] | [Crystal clear positioning] | [Small/Medium/Large] |
| **Consistency** | [How consistent across channels] | [Unified brand experience] | [Small/Medium/Large] |
| **Engagement** | [Current audience interaction] | [Active community interaction] | [Small/Medium/Large] |
| **Conversion** | [Brand → business results] | [Brand drives revenue] | [Small/Medium/Large] |

### Top 5 Gaps (Ranked by Impact)
1. **[Gap 1]** — [Why it matters for business goals] — [What to do about it]
2. **[Gap 2]** — [Why it matters] — [What to do]
3. **[Gap 3]** — [Why it matters] — [What to do]
4. **[Gap 4]** — [Why it matters] — [What to do]
5. **[Gap 5]** — [Why it matters] — [What to do]
```

---

## Step 5: Competitor Positioning Comparison

Identify 3–5 people or brands in the same space and compare positioning:

```markdown
## Competitive Positioning

### Competitor Landscape

| Competitor | Platform Focus | Positioning | Audience Size | What They Do Well | Your Edge |
|---|---|---|:---:|---|---|
| [Name 1] | [Platform] | [How they position] | [Rough size] | [Strength] | [Your edge] |
| [Name 2] | [Platform] | [How they position] | [Rough size] | [Strength] | [Your edge] |
| [Name 3] | [Platform] | [How they position] | [Rough size] | [Strength] | [Your edge] |

### Positioning Map
[Text-based positioning map showing where the user sits relative to competitors on two key dimensions relevant to their industry]

### Positioning Recommendation
[2–3 paragraphs on how the user should position relative to competitors. What space is open? What angle is underserved? What makes them genuinely different?]
```

---

## Step 6: 30-Day Brand Improvement Plan

```markdown
## 30-Day Brand Improvement Plan

### Week 1: Foundation (Fix the Basics)
| Day | Task | Platform | Time |
|:---:|---|---|:---:|
| 1 | Update all bios to match new positioning | All | 1 hr |
| 2 | Fix profile photos for consistency | All | 30 min |
| 3 | Update website messaging / hero section | Website | 1 hr |
| 4 | Create or update link-in-bio page | All | 30 min |
| 5 | Pin/highlight best content on each platform | All | 30 min |
| 6–7 | Content batch: create 5 posts following content pillars below | Primary platforms | 2 hrs |

### Week 2: Content Machine (Start Posting Consistently)
[Specific posts tied to the content pillars identified in Step 7]

### Week 3: Authority Building (Get Visible)
[Collaboration, long-form content, engagement strategy]

### Week 4: Optimize and Scale
[Repurposing top performers, adding email opt-in, month-end review]

### Total Time Investment: ~20–25 hours over 30 days (~45 min/day)
```

---

## Step 7: Content Pillar Recommendations

**If a social calendar exists in Cloud Brain** (`search_notes: "social-calendar"`), load the existing content pillars and note: *"These pillars align with your active content calendar."*

**If no calendar exists**, build fresh:

```markdown
## Content Pillar Recommendations

Content pillars are the 3–5 core topics your brand consistently creates around. Every post should tie back to one of these.

**Pillar 1: [Topic]** — "[One-line description]"
- Why this serves your goals: [explanation]
- Content types: [post formats that work here]
- Example posts: [3 specific ideas]

**Pillar 2: [Topic]** — "[One-line description]"
[same structure]

**Pillar 3: [Topic]** — "[One-line description]"
[same structure]

**Pillar 4: [Topic]** — (optional)
[same structure]

**Pillar 5: Personal/Lifestyle** — (optional)
- Why: People buy from people — humanize the brand

### Recommended Content Mix
- [X]% Pillar 1 (Educational / Value)
- [X]% Pillar 2 (Personal / Storytelling)
- [X]% Pillar 3 (Proof / Authority)
- [X]% Pillar 4 (Industry / Commentary)
- [X]% Pillar 5 (Lifestyle / Human)
```

---

## Step 8: Bio and Tagline Suggestions

```markdown
## Bio & Tagline Suggestions

### Universal Bio (Adapt per platform)

**Option 1 (Authority-led):**
[Title/Role] | [Key achievement or credential] | [What you help people do] | [CTA]

**Option 2 (Value-led):**
[What you do for people] | [Proof point] | [Brand slogan or philosophy] | [CTA]

**Option 3 (Story-led):**
[From X to Y] | [What you teach/share/build now] | [CTA]

### Platform-Specific Bios
- **Instagram:** [150 chars max — punchy, emoji-light]
- **TikTok:** [80 chars max — casual, direct]
- **LinkedIn:** [Professional headline — title + what you do + who you help]
- **Threads / Twitter/X:** [160 chars max — personality-forward]

### Tagline Options (5-8 words max)
1. "[Tagline 1]"
2. "[Tagline 2]"
3. "[Tagline 3]"
4. "[Tagline 4]"
5. "[Tagline 5]"

A great tagline answers "what do you do?" in under 10 words, speaks to the audience's desire (not your resume), and is memorable enough to repeat.
```

---

## Step 9: Save and Present

Save the full audit to Cloud Brain:

```
mcp__cloud-brain__write_note
path: brain/mkt/brand-audit-[slug]-[YYYY-MM-DD].md
```

Where `slug` is the brand's slug from the brand kit (or a slugified version of the brand/person name if no kit exists).

**Present to user:**
1. Preferences banner (who this audit is for)
2. Overall brand score (X/100) with one-line verdict
3. Top 3 gaps ranked by impact
4. Positioning recommendation summary
5. Week 1 priorities from the 30-day plan
6. Top bio/tagline suggestion
7. Where the full audit was saved

If a prior audit exists: lead with the comparison — improvement or regression — before diving into the new findings.

---

## Quality Standards

1. **Honest, not flattering.** Only useful if it tells the truth. If the brand is weak, say so with specifics and solutions.
2. **Evidence-based.** Scores and assessments based on what was actually found, not assumptions. If a platform could not be assessed, say so explicitly.
3. **Actionable over analytical.** Every finding comes with a recommendation. "Your bio is unclear" is useless without "Here's a better bio."
4. **Goal-aligned.** Recommendations tie directly back to stated business goals.
5. **Platform-current.** Recommendations reflect current platform algorithms, formats, and best practices.
6. **Respectful of time.** The 30-day plan must be achievable — 30–60 minutes per day max.

---

## Error Handling

- **No handles or website provided:** Ask — "What are your social media handles and/or website URL? I need at least one channel to audit. If you have no online presence yet, I can build a brand launch plan from scratch instead."
- **Private or inaccessible profiles:** Note: "Your [Platform] profile appears to be private or could not be accessed. Auditing based on what's publicly visible. For a deeper audit, share screenshots or export your analytics."
- **No social presence found:** Pivot to a brand-launch plan: "I could not find an existing online presence for [name/brand]. Instead of an audit, I've built a brand launch plan — how to set up your profiles, what to post first, and how to build from zero."
- **Limited public data:** Note which channels were and were not assessed. Flag: "Limited public data was available. Scores reflect what I could find — actual brand strength may be higher if engagement or reputation isn't publicly visible."
- **Competitor brand audit (not their own):** Proceed, but adjust framing — it becomes competitive intelligence rather than self-improvement. Note: "This is a brand audit of [competitor]. Want me to audit your brand too and compare?"
- **Overwrite existing audit for same date:** Overwrite and note: "Updated your existing brand audit for today."
