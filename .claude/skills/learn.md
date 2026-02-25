---
skill: learn
description: Research a topic and create a well-formed vault note
args: topic to research (e.g., "CRDT", "WebTransport", "Raft consensus")
---

# Learn Skill

Research a topic using web search and create a complete vault note following this vault's conventions.

## Instructions

When this skill is invoked with a topic:

### 1. Research Phase

- Search the web for the topic (2-4 queries covering: overview, practical usage, comparisons, recent developments)
- Prioritize official docs, reputable blogs, and recent sources (2025-2026)
- Identify related concepts that already exist in the vault

### 2. Check for Existing Notes

- Search the vault for existing notes on this topic using the Obsidian CLI or Grep
- If a note already exists, ask the user whether to update it or create a new related note
- Check for related notes to link to

### 3. Create the Note

Create a markdown file following these rules:

**Filename:** lowercase-with-hyphens.md (e.g., `web-transport.md`, `raft-consensus.md`)

**Location:** Root of vault unless it clearly belongs in an existing folder (cloudflare/, agentic-coding/, openclaw/)

**Required structure:**

```markdown
---
tags: [tag1, tag2, tag3]
created: YYYY-MM-DD
difficulty: beginner|intermediate|advanced
status: complete
---

# Title

[1-2 sentence accessible intro. Plain language. Explain what it is and why it matters to someone who's never heard of it.]

## How It Works

[Core concepts with code examples where relevant]

## [Topic-Specific Sections]

[Use ## for main sections, ### for subsections. Include:]
- Code examples in fenced blocks with language identifiers
- Tables for comparisons
- Mermaid diagrams for architecture, flows, or state machines

## Related

- [[existing-note]] - brief description of relationship
- [[another-note]] - brief description of relationship

## References

- [Source Title](URL) - brief description
```

### 4. Conventions to Follow

- **Tags:** 2-5 tags. Use existing tag vocabulary from the vault (check other notes). Categories: language tags (rust, typescript), domain tags (architecture, databases, concurrency), type tags (pattern, concept, tool, best-practice)
- **Intro:** Must be super accessible — no jargon, explain like the reader has never heard of it
- **Diagrams:** Use Mermaid liberally where they add clarity. Split complex diagrams into multiple smaller ones. Prefer vertical (TD) layout for readability
- **Links:** Only link to notes that exist in the vault. Don't create empty links
- **Code:** Use triple backticks with language identifiers. Keep examples practical and runnable
- **Length:** Aim for 100-300 lines. If longer, consider splitting into sub-topics

### 5. After Creating

- Report what was created: filename, tags, links to existing notes, key sections
- Suggest 2-3 follow-up topics that would complement the new note

## Examples

```
/learn "CRDT"
/learn WebTransport
/learn "Raft consensus algorithm"
/learn cloudflare queues
```
