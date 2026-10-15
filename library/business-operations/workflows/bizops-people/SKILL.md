---
name: bizops-people
description: >-
  Use when the user wants to manage a contact — 'add [name] to my contacts', 'update [person]'s info', 'who is [name]?', or 'what's [name]'s email?'. Stores and retrieves contact details, relationship notes, and interaction history. Includes duplicate detection and contradiction flagging so contact records stay accurate over time.
---

# People — Contact Management Skill

## Overview

Manage contacts and people in the brain. Create new people, look up existing contacts, update details, and browse the full contact list. All operations use Cloud Brain MCP tools — no filesystem access.

## When This Skill Applies

- User says "who is [name]?" or "tell me about [name]"
- User says "add [name] to contacts" or "remember [name]'s info"
- User says "update [name]'s email/phone/role"
- User says "list all contacts" or "show my contacts"
- User says "what's [name]'s email/phone?"
- User mentions a new person with contact details
- User says "create a contact for [name]"
- User says "look up [name]" or "find [name]"

## Person Note Template

When creating a person via `write_note`, use this content structure:

```markdown
---
type: person
tags: [person]
role: [role or relationship]
email: [if known]
phone: [if known]
company: [if known]
priority: [high/medium/low]
last_contact: [YYYY-MM-DD]
---

# [Full Name]

## Role & Relationship
[How you know them, what they do]

## Contact Info
- Email: [email]
- Phone: [phone]
- Company: [company]

## Context & Notes
[Relevant context, meeting notes, relationship history]

## Related
[[Project Name]] [[Other Person]] [[Goal Name]]
```


## Contact Defaults

No pre-flight setup required — this is a lookup and CRUD tool. Use these defaults when creating contacts:

- **Priority:** medium (override per-contact if the user specifies)
- **Categories:** client / vendor / partner / prospect / personal / team (accept any label the user provides)

Jump directly to the requested operation.

## Operations

### 1. Create Person

Create a new contact in the brain.

**Before creating:** Run duplicate detection (see below). If a match is found, ask the user before proceeding.

**Steps:**
1. Call `mcp__cloud-brain__search_notes` with the person's name and `tags: ["person"]` to check for duplicates
2. If no duplicate found, call `mcp__cloud-brain__write_note` with:
   - `title`: `firstname-lastname` (kebab-case)
   - `directory`: `"people"`
   - `tags`: `["person"]`
   - `content`: Populated person template (see above)
3. Include `[[Entity Name]]` wiki-links in the content for any mentioned projects, companies, or other people

**Example:**
```
User: "Add John Smith to contacts — he's the CPA at TaxPro Inc, email john@taxpro.com"

Action: write_note(
  title="john-smith",
  directory="people",
  tags=["person"],
  content="---\ntype: person\ntags: [person]\nrole: CPA\nemail: john@taxpro.com\ncompany: TaxPro Inc\npriority: medium\nlast_contact: 2026-04-01\n---\n\n# John Smith\n\n## Role & Relationship\nCPA at [[TaxPro Inc]]. Handles tax filings.\n\n## Contact Info\n- Email: john@taxpro.com\n- Company: TaxPro Inc\n\n## Context & Notes\nAdded as contact.\n\n## Related\n[[TaxPro Inc]]"
)
```

### 2. Lookup Person ("who is X?")

Find and display a person's full profile.

**Steps:**
1. Call `mcp__cloud-brain__search_notes` with the person's name and `tags: ["person"]`
2. If found, call `mcp__cloud-brain__read_note` with the note identifier to get full details
3. Return a formatted summary of the person's info

**Output format:**
```
**John Smith** — CPA at TaxPro Inc
- Email: john@taxpro.com
- Priority: Medium
- Last Contact: 2026-04-01
- Context: Handles all tax filings and entity structuring
- Related: [[TaxPro Inc]]
```

If not found: "I don't have anyone named [name] in the brain. Would you like me to create a contact for them?"

### 3. Update Person

Modify an existing person's information.

**Steps:**
1. Call `mcp__cloud-brain__read_note` to get the current content
2. **Contradiction flagging:** If the new info conflicts with existing info, flag it:
   - "The brain currently says John's email is john@old.com. You said john@new.com. Which is correct? I'll update once you confirm."
3. Once confirmed (or if no contradiction), call `mcp__cloud-brain__edit_note` to update the specific fields
4. Update `last_contact` date to today

**Never silently overwrite conflicting data.** Always flag contradictions and ask for confirmation.

### 4. List Contacts

Show all people in the brain.

**Steps:**
1. Call `mcp__cloud-brain__search_notes` with `tags: ["person"]`
2. Format results as a table:

```
| Name | Role | Company | Priority |
|------|------|---------|----------|
| John Smith | CPA | TaxPro Inc | Medium |
| Sarah Chen | Partner | — | High |
```

If no contacts found: "No contacts in the brain yet. Say 'add [name] to contacts' to create one."

### 5. Quick Contact Lookup

For specific field queries like "what's John's email?"

**Steps:**
1. Call `mcp__cloud-brain__search_notes` with the person's name and `tags: ["person"]`
2. Call `mcp__cloud-brain__read_note` to get full details
3. Extract and return just the requested field

**Example:**
```
User: "What's John Smith's email?"
Response: "john@taxpro.com"
```

## Entity Linking

Person notes MUST include `[[Entity Name]]` wiki-links for:
- **Projects** they are associated with (e.g., `[[Warrior Thoughts]]`)
- **Companies** they work for (e.g., `[[TaxPro Inc]]`)
- **Other people** they are connected to (e.g., `[[Sarah Chen]]`)
- **Goals** they are related to (e.g., `[[Q2 Revenue Target]]`)

These links allow Cloud Brain to build a knowledge graph connecting people to the rest of the brain.

## Duplicate Detection

Before creating any new person:

1. Call `mcp__cloud-brain__search_notes` with the person's name and `tags: ["person"]`
2. Also search with common variations (e.g., "John" for "John Smith", "J. Smith")
3. If a match is found, inform the user:
   - "This person already exists in the brain. Would you like to update their record instead?"
4. Show the existing record summary so the user can decide

## Contradiction Flagging

When updating a person's information:

1. Read the current note content first
2. Compare each field being updated against the existing value
3. If any field conflicts (e.g., different email, different company), flag it:
   - "The brain says [name]'s [field] is [old value]. You said [new value]. Which is correct?"
4. Wait for user confirmation before overwriting
5. Never silently replace existing data with conflicting new data

## Error Handling

- **Person not found:** Offer to create a new contact
- **Ambiguous name match:** Show all matches and ask user to clarify
- **Missing required info:** Create the contact with what's available, note missing fields
- **Sensitive data (SSN, passwords):** Do NOT store. Warn: "This looks like sensitive data. I won't store credentials in the brain."
