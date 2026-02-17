---
tags: [obsidian, knowledge-management, workflows, tools, cli]
created: 2026-02-15
difficulty: intermediate
status: complete
---

# Obsidian

Obsidian is a note-taking app that stores everything as plain text files on your computer, so you own your notes forever and can open them with any text editor. It connects your notes together like a personal Wikipedia, making it perfect for building a knowledge base that grows with you.

## Core Philosophy

**File over app:**
> "If you want to create digital artifacts that last, they must be files you can control, in formats that are easy to retrieve and read."

**Why Obsidian:**
- Vault = just a folder of files
- Plain text Markdown
- You control the files
- Works with any editor
- Future-proof
- Local-first with optional sync

## Obsidian CLI

### Official CLI (Released Feb 10, 2026)

Obsidian v1.12.0 introduced an official CLI with 100+ commands.

**Key benefits:**
- Terminal-based vault operations without GUI
- Automation and scripting support
- AI tool integration (e.g., Claude Code can manipulate vault directly)
- Template application, task management via command line
- TUI mode for remote SSH access

**Current status:** Early Access (requires Catalyst license $25), planned free release later.

## Why It Benefits This Repo

**Direct manipulation:**
- Create/edit notes from terminal/scripts
- Query vault contents programmatically
- Automate note organization and linking

**Integration with workflows:**
- Add programming notes during CLI sessions
- Script-based note generation from code analysis
- Automated knowledge base updates

**Remote access:**
- Edit vault via SSH without GUI
- Terminal-only environments (servers, remote dev boxes)

## Community Tools

Alternative/supplemental CLI tools:
- **[notesmd-cli](https://github.com/Yakitrak/notesmd-cli)**: Works without Obsidian running, good for pure automation
- **[obsidian-export](https://github.com/zoni/obsidian-export)**: Export vault to CommonMark
- **[obsidian-cli](https://github.com/mcky/obsidian-cli)**: Query and interact with vault data

## Vault Organization Patterns

### Minimal Folder Approach

**Problem with deep folder hierarchies:**
- Many notes belong to multiple categories
- Creates overhead deciding where things go
- Reduces capture speed
- Hard to maintain over time

**Solution: Flat structure with links and properties**

```
vault/
├── [root]/             # Most notes here (journal, essays, evergreen)
├── References/         # External things (books, movies, places)
├── Clippings/          # Articles from others
├── Attachments/        # Media files
├── Daily/              # Daily notes (YYYY-MM-DD.md)
├── Templates/          # Note templates
└── .obsidian/          # Config (auto-generated)
```

**Key principle: Location indicates type, not topic**
- Root → you wrote it
- References → external references
- Clippings → saved articles

**Organize with properties, not folders:**
```yaml
---
categories: [Books, Sci-fi, Philosophy]
rating: 7
author: [[Author Name]]
---
```

### Personal Style Guide

**Consistency collapses future decisions.**

Example rules to consider:
- Always pluralize tags/categories (`books` not `book`)
- Use YYYY-MM-DD for all dates
- Link first mention of everything
- Keep single to-do list per week
- Use consistent rating scale (e.g., 1-7)

**Write down your rules. Review quarterly. Adjust as needed.**

### Heavy Linking Strategy

**Link the first mention of everything, even if target doesn't exist.**

```markdown
I went to see [[Perfect Days]] with [[Aisha]] at [[Vidiots]].

Quote: [[Next time is next time, now is now]]

Reminds me of [[Mindfulness in daily practice]].
```

**Unresolved links = breadcrumbs for future connections.**

Don't worry about creating the target note yet. The link is valuable on its own.

**Benefits over time:**
- Trace how ideas emerged
- Find unexpected connections
- Navigate by concept, not folder
- Network becomes more valuable as it grows

### Fractal Journaling

**Multi-scale review system that bubbles up insights.**

```
Yearly review
  ↑ bubbles up from
Monthly review
  ↑ bubbles up from
Weekly review  (every few days)
  ↑ bubbles up from
Daily fragments  (throughout day)
  ↑ captured from
Individual thoughts
```

**Daily capture:**
- Use "unique note" hotkey throughout day
- Creates: `YYYY-MM-DD HHmm Optional title.md`
- Don't worry about organization yet

**Reviews:**
- Weekly: Compile salient thoughts, create connections
- Monthly: Extract themes, identify patterns
- Yearly: Big picture, major themes

**Result:** Can zoom in/out of your thinking at varying detail levels.

### Random Revisit

**Quarterly maintenance for taming wilderness:**

1. Set aside dedicated time
2. Use "random note" hotkey
3. Travel randomly through vault
4. Use local graph at shallow depth
5. Create missing links
6. Fix formatting
7. Apply style guide updates

**Why manual (not automated):**
> "Don't delegate understanding. Doing this maintenance helps me understand my own patterns."

Keep human in loop for:
- Understanding patterns
- Making connections
- Applying judgment
- Learning about yourself

### Properties and Templates

**Almost every note starts from a template.**

Common properties:
- `created`, `start`, `end`, `published` (dates)
- `author`, `director`, `artist`, `host` (people)
- `genre`, `type`, `topic`, `categories` (themes)
- `rating` (consistent scale)
- `location`, `city`, `coordinates` (places)

**Property rules:**
1. **Reusable across categories** - `genre` works for books, movies, shows
2. **Composable templates** - Mix "Person" + "Author" in same note
3. **Short names** - `start` not `start-date`
4. **Default to list type** - Easier to add multiple values later

### Navigation Patterns

**Primary methods (by frequency):**

1. **Quick switcher** - Type note name, jump directly (Cmd/Ctrl+O)
2. **Links within notes** - Click to related notes
3. **Backlinks** - See what links to current note
4. **Search** - Full-text when needed
5. **Random note** - Serendipitous discovery
6. **Local graph** - Visualize nearby connections

**Rarely used:**
- File explorer (only for admin)
- Folder navigation (most notes in root anyway)

## AI-Assisted Patterns

For AI-assisted workflows with Claude Code, see [[agentic-coding/obsidian-claude-system]]:
- Progressive disclosure (4-layer context selection)
- CLAUDE.md as system instructions
- Vault index for orientation
- Agent-assisted note-taking

## Related Topics

- [[agentic-coding/obsidian-claude-system]] - AI-assisted knowledge system (Heinrich's approach)
- [[knowledge-graphs]] - Graph structures for knowledge
- [[zettelkasten]] - Related note-taking methodology
- [[personal-knowledge-management]] - PKM principles

## Sources

### CLI
- [Obsidian CLI - Official Help](https://help.obsidian.md/cli)
- [Obsidian 1.12.0 Release Notes](https://obsidian.md/changelog/2026-02-10-desktop-v1.12.0/)
- [Obsidian CLI Overview](https://deepakness.com/raw/obsidian-cli/)
- [Community CLI Tools](https://github.com/Yakitrak/notesmd-cli)

### Vault Organization
- Kepano's vault template and methodology
- Minimal theme: https://minimal.guide
- File over app philosophy
