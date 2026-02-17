---
tags: [openclaw, configuration, agents, teaching, instructions]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Configuration Files: Teaching Your Agent

Your agent wakes up fresh every session knowing WHERE things are saved, but not thinking to check there FIRST. Configuration files teach it default behaviors so it stops asking dumb questions.

## The Problem

**Example failure pattern:**
```
User: "Deploy to staging"
Agent: "What are your staging credentials?"
User: "You already saved them!"
Agent: "Oh yes, they're in ~/.env. Let me read that..."
```

**The agent wasn't stupid - it just didn't have a rule to check first.**

## AGENTS.md: The Main Instruction File

This is your workspace-level instruction file. Add rules here to prevent "dumb questions":

```markdown
# AGENTS.md

## Before Asking Me For Things

- Before asking for credentials, check: ~/.env, ~/.openclaw/credentials/, memory/secrets.md
- Before asking for API documentation, check: memory/apis.md, project README
- Before asking what we're working on, read: memory/active-tasks.md
- Before asking for project structure, run: tree -L 2

## Tool Check Pattern

Before saying "I can't do X", check TOOLS.md for relevant tools.

## Memory Check Pattern

Before asking questions about past decisions, check:
1. memory/feedback.json (past approvals/rejections)
2. memory/lessons.md (learned patterns)
3. memory/projects.md (project-specific context)

## Default Behaviors

- Always verify output after generating (ls, curl, etc.)
- Always use sub-agents for parallel work (>3 similar tasks)
- Always log important decisions to memory/decisions.md
- Always check cost after tasks >$1 (flag if >2x budget)
```

## TOOLS.md: Available Capabilities

Document what tools/integrations you have:

```markdown
# TOOLS.md

## Installed

- Cloudflare API (workers, pages, DNS)
- GitHub CLI (`gh`)
- Stripe API (read-only)
- Notion API
- Telegram bot

## Authentication

All credentials in: ~/.openclaw/credentials/

## Before You Say "I Can't"

Check this file. If a tool is listed, you CAN do it.
```

## The Configuration Hierarchy

```
AGENTS.md       → Behavioral rules, default actions
MEMORY.md       → Identity, core capabilities (keep <200 lines)
SOUL.md         → Hard boundaries, security rules (NEVER override)
TOOLS.md        → Available integrations, capabilities
memory/*.md     → Detailed context, project data
```

**Think of it like onboarding a new employee:**
- Day 1: They ask dumb questions
- Week 2: You add rules to the handbook
- Month 3: They check the handbook first

**Same with your agent - capture repetitive corrections as rules in AGENTS.md.**

## Related Topics

- [[memory]] - Memory architecture and file organization
- [[patterns]] - Agent workflow patterns
- [[security]] - SOUL.md security boundaries
