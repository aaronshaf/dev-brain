---
tags: [obsidian, knowledge-management, ai-agents, claude-code, workflows]
created: 2026-02-15
difficulty: intermediate
status: complete
---

# Obsidian + Claude Code: Knowledge System Architecture

Building an AI-assisted knowledge system using Obsidian vaults as operating systems for thinking. Based on Heinrich's production patterns.

## Core Philosophy

**Shift from note-taking to system operation:**
- You don't take notes anymore
- You operate a system that takes notes
- Your role becomes **judgment** (deciding what matters)
- Human evolves from writer → editor, creator → curator

**Knowledge = Code:**
- Knowledge bases and codebases are structurally similar
- Both are folders of text files with relationships
- Both benefit from agents that navigate and operate them
- Vibe coding for knowledge work

**Depth over breadth:**
> "Depth over breadth. Quality over speed. Tokens are free. This is not about efficiency. This is about excellence."

## Vault Structure

### Basic Architecture

```
vault/
├── 00_inbox/           # Capture zone, zero friction
├── 01_thinking/        # Your notes and synthesis
│   ├── notes/          # Individual thinking notes
│   └── topic-pages.md  # MOCs (Maps of Content)
├── 02_reference/       # External knowledge
│   ├── tools/          # Tool documentation
│   ├── approaches/     # Methods and patterns
│   └── sources/        # External knowledge
├── 03_creating/        # Content in progress
│   └── drafts/
├── 04_published/       # Finished work archive
├── 05_archive/         # Inactive content
├── 06_system/          # Templates and scripts
├── CLAUDE.md           # Teaches AI your system (~2000 lines)
├── vault-index.md      # List of all notes with descriptions
└── attachments/        # Images and files
```

**Key principle:** Structure is **purpose-dependent**
- Thinking vault ≠ work vault ≠ research vault ≠ creative vault
- Same pattern (markdown + links + CLAUDE.md), different rules
- Folder location tells you what something is

### Vault Type Examples

**Work vault emphasis:**
- Capture first, structure later
- Project folders with meetings/outputs
- Client context for AI consumption

**Research vault emphasis:**
- Source tracking and citations
- Literature notes
- Claim verification

**Creative vault emphasis:**
- Idea capture and incubation
- Draft progression
- Reference organization

**Thinking vault emphasis (Heinrich's example):**
- Developing understanding
- Depth over breadth
- Quality over speed
- Connection discovery

## Writing Effective Notes

### 1. Composability

**Can this note be linked from elsewhere and still make sense?**

Bad: Note requires reading 5 other notes first
Good: Note stands alone but connects to build bigger structures

Think of notes like **Lego blocks:**
- Each complete on its own
- They connect to build bigger structures
- Network becomes valuable

### 2. Title as Claim

**Don't name notes like topics. Name them like claims.**

Bad: "Thoughts on AI slop"
Good: "Quality is the hard part"

**Why:** When you link `[[quality is the hard part]]`, the title becomes part of your sentence naturally.

Forces Claude to think differently when building sentences because it requires understanding.

### 3. Links as Thought

**Weave links into sentences, not as footnotes.**

Bad:
```markdown
This relates to quality. See: quality-note.md
```

Good:
```markdown
Because [[quality is the hard part]] we need to focus on curation.
```

The link becomes part of your thought. Agent follows your reasoning by following links.

### 4. Network Value

**Individual notes matter less than their relationships.**

- A note with many incoming links is more valuable than isolated note
- Every link creates a new reading path
- The network IS the knowledge

## Progressive Disclosure: The 4-Layer Pattern

**Problem:** By default Claude reads full files whenever they seem useful. Creates token bloat and noise.

**Solution:** Force Claude to be selective through **progressive disclosure**.

### Layer 1: File Tree

```json
{
  "hooks": {
    "SessionStart": [{
      "hooks": [{
        "type": "command",
        "command": "tree -L 3 -a -I '.git|.obsidian' --noreport"
      }]
    }]
  }
}
```

**What it does:**
- Injects full file tree before Claude touches anything
- Gives Claude the map before exploring
- Descriptive filenames work as first impression

Example filename:
- Good: `queries-evolve-during-search-so-agents-should-checkpoint.md`
- Bad: `search-notes.md`

**Just reading the tree shows what notes are about.**

### Layer 2: YAML Descriptions

Every note has one-sentence description in frontmatter:

```yaml
---
description: Memory retrieval in brains works through spreading activation where neighbors prime each other. Wiki link traversal replicates this, making backlinks function as primes that surface relevant contexts
---
```

**Usage:**
```bash
# If filename seems interesting, check description
rg "^description:" 01_thinking/*.md
```

Description elaborates the title. If it passes, move to layer 3.

### Layer 3: Outline

```bash
# Check section headers before reading full content
grep -n "^#" "01_thinking/knowledge-work.md"

# Output:
# 5:# knowledge-work
# 13:## Core Ideas
# 19:## Tensions
# 23:## Gaps
```

**Why:** Sometimes you only need one section. Loading full file creates noise.

Claude knows where to look and can read what it needs.

### Layer 4: Full Content

**Only load full file if it passed all three filters.**

Most notes never get here. That's the point. When Claude has to justify each read, it curates better.

### The MCP Parallel

This pattern mirrors MCP tool discovery:

```
Tool list → Tool search → Tool references → Full definitions
File tree → Descriptions → Outline → Full content
```

**Implementation:**
1. SessionStart hook that runs `tree`
2. YAML frontmatter with description field
3. Instructions in CLAUDE.md telling Claude to check descriptions before reading
4. Just a few constraints that force selectivity

## How Claude Operates

### 1. Orientation Phase

Every task starts with orientation:
1. Scan structure (tree hook)
2. Check vault index for relevant notes
3. Read topic page (MOC)
4. Follow links to build understanding
5. **Make no changes without context**

### 2. Navigation Learning

**Claude leaves breadcrumbs for future sessions:**

When Claude discovers something useful about navigating a topic, it records that in the topic page.

Future sessions:
- Read those notes
- Learn from past navigation
- Vault remembers how to think through itself

### 3. Connection Discovery

**Sometimes two notes interact in interesting ways:**

Claude creates a new note capturing the insight that emerges from combining them.

Every new capture triggers:
1. Search for related notes
2. Add links with context
3. Network grows organically

### 4. Topic Pages (MOCs)

**Maps of Content that act like tables of contents:**

- Link to related notes
- Contain navigation notes Claude leaves for itself
- Create learning paths
- Agent starts broad, narrows to relevant, follows links

## CLAUDE.md Structure

**This file teaches the AI your system. Start simple, evolve as you learn.**

Heinrich's is ~2000 lines because he keeps refining what works.

**What to include:**

1. **Philosophy:**
   - Depth over breadth
   - Quality over speed
   - Your specific purpose

2. **Conventions:**
   - How to name notes
   - Where things belong
   - When to create vs edit

3. **Operations:**
   - Progressive disclosure workflow
   - How to add links
   - How to use topic pages

4. **Quality standards:**
   - What makes a good note
   - When to split/merge
   - Review criteria

5. **Hooks and automation:**
   - SessionStart configuration
   - Navigation patterns
   - Context injection rules

**Key insight:** "Watching an AI completely disrespect my philosophies taught me this the hard way."

You have to **textualize everything**. Implicit knowledge must become explicit.

## Vault Index Pattern

**Problem:** Claude needs to understand what exists without reading every file. Impossible with thousands of notes.

**Solution:** Vault index with one-sentence descriptions.

```markdown
# Vault Index

## 01_thinking/notes/

- [[quality-is-the-hard-part]] - Depth over breadth in knowledge work, why curation beats volume
- [[spreading-activation-models-how-agents-should-traverse]] - Memory retrieval through spreading activation, backlinks as primes
- [[queries-evolve-during-search-so-agents-should-checkpoint]] - Search refinement requires saving state
- [[network-value-exceeds-node-value]] - Individual notes matter less than relationships
```

**Claude can scan 50 notes in seconds without opening them.**

Combined with:
- Tree structure (orientation)
- Topic pages (guided navigation)
- Progressive disclosure (selective reading)

## Benefits of This System

1. **Markdown is the system**
   - Tools like Obsidian are just windows into it
   - Vault survives any app disappearing
   - Plain text, any editor can read
   - Any AI can process
   - You own your data completely

2. **Network effects compound**
   - Every link creates new reading paths
   - Well-maintained vault feels different from noisy one
   - Depth accumulates over time

3. **Agent memory across sessions**
   - LLMs have no memory
   - Vault gives them one
   - Navigation notes compound
   - System learns how to think through itself

4. **Selective context injection**
   - Progressive disclosure prevents token bloat
   - Only relevant context loaded
   - Quality curation over volume

5. **Thinking partner, not writing assistant**
   - System encodes how you think, not just what you thought
   - Methodology becomes part of the system
   - Operating system for thinking with AI

## How to Start

### 1. Create Structure

```bash
mkdir -p vault/{00_inbox,01_thinking/notes,02_reference,03_creating,04_published,05_archive,06_system}
```

Create folders that match your purpose. Think about what you actually need to organize.

### 2. Write CLAUDE.md

Start simple. Explain:
- What this vault is for
- How you want to organize knowledge
- What quality means in your context
- Basic conventions (naming, linking, structure)

Evolve it as you learn what works.

### 3. Add SessionStart Hook

In Claude Code settings or config:

```json
{
  "hooks": {
    "SessionStart": [{
      "hooks": [{
        "type": "command",
        "command": "tree -L 3 -a -I '.git|.obsidian' --noreport"
      }]
    }]
  }
}
```

### 4. Add Progressive Disclosure Instructions

In CLAUDE.md:

```markdown
## Context Selection Protocol

Before reading full files, use progressive disclosure:

1. Check file tree (already loaded via SessionStart hook)
2. If promising, check description: `rg "^description:" path/to/files/*.md`
3. If still relevant, check outline: `grep -n "^#" file.md`
4. Only then read full content

Most notes should never reach step 4. Justify each read.
```

### 5. Create Vault Index

```markdown
# Vault Index

One-sentence descriptions of every note.

## 01_thinking/notes/
- [[note-name]] - Description

[Update this as vault grows]
```

### 6. Let Claude Operate

**Start small:**
- Capture something
- Ask Claude to find connections
- Let it navigate and discover relationships
- Let it suggest where things belong

**ALWAYS review what it produces and edit for quality.**

### 7. Iterate on Philosophy

Watch how Claude behaves. When it does something wrong, ask:
- Was my instruction unclear?
- Did I textualize this assumption?
- Does CLAUDE.md need updating?

Refine your system based on what works.

## Key Insights

### "Depth matters more than breadth"

Well-maintained vault feels different from noisy one. You can feel the difference.

### "Notes that stand alone and are composable"

Each note complete on its own, but connects to build bigger structures.

### "Your job becomes judgment"

Deciding what matters, not writing everything yourself.

### "Network is the knowledge"

Individual notes matter less than relationships. Links create reading paths.

### "The vault encodes how you think"

Not just what you thought about. The methodology becomes part of the system.

### "Every vault needs its own philosophy"

Purpose determines rules. Don't copy systems blindly.

### "When Claude has to justify each read, it curates better"

Progressive disclosure forces selectivity. Most notes never fully loaded.

## Comparison to Other Patterns

| Pattern | Context Approach | Best For |
|---------|-----------------|----------|
| **Default Claude Code** | Reads full files when relevant | Simple codebases |
| **Progressive Disclosure** | 4-layer filtering before full read | Large knowledge bases |
| **MCP Tool Discovery** | Deferred loading until searched | Many tools (50+) |
| **OpenClaw Memory** | Split files by purpose (active-tasks, daily logs) | Agent memory |

**Common theme:** Structure that lets AI orient quickly without loading everything.

## Anti-Patterns

### 1. References as Footnotes

Bad:
```markdown
This is important. See: [[other-note]]

References:
- [[ref-1]]
- [[ref-2]]
```

Good:
```markdown
Because [[quality-is-the-hard-part]], we focus on [[curation-over-volume]].
```

### 2. Topic Titles

Bad: `thoughts-on-knowledge-work.md`
Good: `quality-is-the-hard-part.md`

### 3. Notes That Require Context

Bad: Note only makes sense if you read 5 others first
Good: Note stands alone, links provide optional depth

### 4. Dumping Everything in CLAUDE.md

Bad: 5000-line CLAUDE.md with everything
Good: 2000-line CLAUDE.md that links to topic pages for details

### 5. No Progressive Disclosure

Bad: Claude reads every file fully
Good: 4-layer filtering, most notes never fully loaded

## Related Topics

- [[openclaw]] - Agent memory architecture patterns
- [[obsidian-workflows]] - Obsidian-specific features and plugins
- [[knowledge-graphs]] - Graph structures for knowledge representation
- [[ai-assisted-thinking]] - Using AI as thinking partner

## References

- Heinrich (@arscontexta): Obsidian + Claude Code 101
- Heinrich: Context Engineering with Progressive Disclosure
- Pattern mirrors MCP tool discovery architecture
- Inspired by spreading activation models from cognitive science
