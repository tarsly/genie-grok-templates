---
name: comm-zoom-search
description: >-
  Zoom knowledge search — search across all your Zoom meetings, recordings, transcripts, Team Chat messages, and Zoom Docs with a natural language question. Finds decisions, commitments, conversations, and context buried in your Zoom history. Returns specific answers with the meeting date, attendees, and exact context so you can trace where the information came from. Triggers on: 'what did we decide about [topic]', 'find the conversation about [topic]', 'did [person] ever mention [topic]', 'when did we talk about [topic]', 'look up what was said about [topic]', 'search my meetings for [topic]', 'what was discussed about [topic]', 'find in Zoom', 'search Zoom history', 'did we ever discuss [topic]', 'what did [person] say about [topic]', 'find that meeting where we talked about [topic]'.
---

# COMM-ZOOM-SEARCH — Zoom Knowledge Search

> **Requires:** Zoom MCP connector.

---

## Overview

Your Zoom account is a knowledge base. Every meeting summary, chat message, and Zoom Doc contains decisions, commitments, context, and institutional memory — most of it buried and never retrieved. This skill lets you ask natural language questions and get back specific answers pulled from your actual Zoom data.

"What did we decide about pricing?" → Finds the meeting, quotes the decision, tells you who said it and when.

"Did Sarah ever mention budget constraints?" → Searches transcripts and chat for every instance of Sarah discussing budget.

"Find that meeting where we talked about the new hire" → Locates the meeting, gives you the summary and key points.

---

## When This Skill Applies

- User asks "what did we decide about [topic]?"
- User says "find the conversation about [topic]"
- User says "search my meetings for [topic]"
- User asks "when did we talk about [X]?"
- User asks "did [person] ever bring up [topic]?"
- User says "find in Zoom" or "search Zoom history"
- User wants to trace back a decision, commitment, or conversation to its source
- User says "I remember we talked about [X] — when was that?"
- User wants to know what was said about a topic across multiple meetings

---

## Step 1: Understand the Query

Parse the user's natural language question to identify:

1. **Topic/keywords:** What concept, project, decision, or subject are they searching for?
2. **Person filter (optional):** Are they looking for what a specific person said?
3. **Time filter (optional):** Is there a date range implied? ("in Q1", "last month", "before we launched")
4. **Source type:** Meetings/recordings, Team Chat, Zoom Docs, or all?
5. **Answer type:** Are they looking for a decision, a quote, a list of mentions, or a meeting?

Ask for the user's timezone before searching (required by Zoom search API).

---

## Step 2: Search Strategy

Run searches in order from broadest to most specific. Stop when you have enough to answer the question.

### Search A — Meeting Search
Call `mcp__zoom__search_meetings` with:
- `q`: the core keywords from the query
- `from`/`to`: set based on any time context in the query (default: past 12 months)

This returns meeting titles and metadata. Identifies which meetings are relevant candidates.

### Search B — Zoom Docs and Chat Search
Call `mcp__zoom__search_zoom` with:
- `query`: the keywords
- `search_entities`: include both `{"entity_type": "chat"}` and `{"entity_type": "zoom_doc"}`
- For Zoom Docs, set `doc_view` to `my_docs` and `shared_with_me` to maximize coverage
- Apply time range filters if the user specified one

This surfaces relevant chat messages and documents that may contain the answer even if the meeting search doesn't catch it.

### Search C — Transcript Deep Dive (when needed)
If the question requires knowing *what was actually said* (a specific quote, a commitment, a decision with context), or if meeting search found relevant meetings but the summary isn't enough:

For each relevant meeting from Search A (up to 3 most relevant):
- Call `mcp__zoom__get_recording_resource` with `types: "transcript,summary,nextStep"`
- Search the transcript for the keywords
- Extract the specific passages where the topic was discussed, with speaker names and timestamps

---

## Step 3: Synthesize the Answer

Compile results into a clear, sourced response.

**Format the answer based on query type:**

### For "what did we decide about X?" queries:
```
Decision found in [N] meeting(s):

**[Meeting Topic] — [Date]**
Attendees: [Names]
Decision: [What was decided — direct quote or close paraphrase from transcript/summary]
Context: "[Supporting quote showing how the decision was reached]"
— [Speaker Name], [timestamp if available]

[Additional meetings if relevant]

Summary: [1–2 sentence synthesis if multiple meetings found]
```

### For "what did [person] say about X?" queries:
```
[Person Name] mentioned [topic] in [N] instance(s):

**[Meeting Topic] — [Date]**
"[Exact quote from transcript]"
— [Person Name], [timestamp]
Context: [What was being discussed when they said it]

[Additional instances if found]
```

### For "find the meeting about X" queries:
```
Found [N] matching meeting(s):

1. **[Meeting Topic] — [Date]**
   Attendees: [Names]
   Summary: [2–3 sentence summary of what was discussed about the topic]
   Key outcome: [Decision or action item if any]

2. **[Meeting Topic 2] — [Date]**
   ...

[Link or UUID for each meeting if helpful for the user to reference]
```

### For chat/doc results:
```
Also found in Zoom Chat / Zoom Docs:

**[Channel or Doc name] — [Date]**
"[Relevant message or passage]"
— [Person], [time]
```

---

## Step 4: Check Cloud Brain for Additional Context

After searching Zoom, check Cloud Brain for any saved meeting notes or project files that might contain additional context on the topic:

- `mcp__cloud-brain__search_notes` with the same keywords
- If relevant notes found, append: *"Also found in your Brain notes: [brief summary of what was there]"*

This catches any meetings processed with comm-zoom-debrief that were also saved to the brain.

---

## Step 5: Present Results and Offer Follow-Up

Deliver the sourced answer, then offer:
- "Want me to pull the full debrief from any of these meetings?"
- "Want me to check if these action items were completed?"
- "Want me to prep for an upcoming follow-up on this topic?"

---

## Handling No Results

If no relevant content is found across all three search types:

*"I searched your Zoom meetings, chat, and docs from [date range] and didn't find any mentions of [topic]. A few possibilities: the conversation may have happened before the search window (try specifying an earlier date range), Zoom AI Companion may not have been enabled for those meetings, or the discussion happened in a channel I don't have access to. Want me to search a different time range or try different keywords?"*

---

## Error Handling

- **Too many results:** Prioritize by recency and relevance. Show top 5 with a note that more were found. Offer to narrow the search.
- **Transcript not available for a meeting:** Use the AI summary. Flag: "Full transcript not available for this meeting — using Zoom's AI summary."
- **No recording on meeting (transcript unavailable):** Work from meeting title, summary, and any notes. Flag the limitation.
- **Ambiguous query:** If the keywords are too broad and return many unrelated meetings, ask: "I found many results — can you narrow it down? For example, approximately when did this happen, or who was in the meeting?"
- **Person not found in any meetings:** "I didn't find any meetings with [Name] in your Zoom history. They may have joined under a different name or email, or the meetings may predate your search window."
