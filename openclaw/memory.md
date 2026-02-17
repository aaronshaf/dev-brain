---
tags: [openclaw, memory, persistence, architecture]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Memory Architecture

Your OpenClaw agent starts every session with a blank slate—memory files are literally its brain. Organizing them well is the difference between an agent that remembers context and one that asks the same questions every day.

## File Organization

**Don't dump everything in MEMORY.md.** Split strategically:

```
~/.openclaw/workspace/
├── MEMORY.md              # Core identity, loaded every session (keep <200 lines)
├── SOUL.md                # Agent boundaries and hard limits
├── memory/
│   ├── active-tasks.md    # Current work - the "save game"
│   ├── 2026-02-15.md      # Daily raw logs
│   ├── 2026-02-14.md
│   ├── projects.md        # Project-specific context
│   ├── lessons.md         # Learned patterns
│   └── skills.md          # Custom workflow documentation
```

## MEMORY.md Strategy

**Keep it under 200 lines** - lines after 200 get truncated.

What goes in MEMORY.md:
- Agent identity and personality
- Core capabilities
- Links to detailed topic files
- Most critical lessons

What goes in separate files:
- Detailed project context → `memory/projects.md`
- Skill documentation → `memory/skills.md`
- Daily activity logs → `memory/YYYY-MM-DD.md`
- Current work state → `memory/active-tasks.md`

**Why this matters:** The agent loads MEMORY.md every session. Keep it lean and link to details.

## Persistent Memory Maintenance

**Cron job pattern** from @pejmanjohn:

```bash
# Every 1-3 days: distill recent memory into long-term
# Make it INTERACTIVE - don't let agent do it unsupervised

# Example cron (runs at 2am every 3 days)
0 2 */3 * * /home/user/scripts/consolidate-memory.sh
```

The consolidation script should:
1. Read last 3 days of daily logs
2. Extract important patterns/learnings
3. **Present suggestions to you for approval**
4. Update long-term memory files
5. Find and flag outdated entries

**Critical:** Never let memory consolidation run fully autonomous. Always human-in-the-loop.

## active-tasks.md: The Crash Recovery Pattern

This is your "save game" file:

```markdown
# Active Tasks

## In Progress

### Deploy 11 websites
- Status: spawned 4 sub-agents
- Session keys: [sess-1, sess-2, sess-3, sess-4]
- Batches:
  - Agent 1: sites 1-3
  - Agent 2: sites 4-6
  - Agent 3: sites 7-9
  - Agent 4: sites 10-11
- Started: 2026-02-15 14:30
- Validation criteria: Each site must respond 200 OK

## Completed Today
- [x] Research competitor TikTok hooks (6 examples logged)

## Blocked
- Database migration - waiting for staging credentials
```

**When to update:**
- **START** a task → write it
- **SPAWN** a sub-agent → note session key
- **COMPLETE** → update status
- **CRASH/RESTART** → agent reads this first and resumes

This file enables autonomous recovery without "what were we doing?"

## Related Topics

- [[configuration]] - Teaching your agent with AGENTS.md
- [[patterns]] - Agent workflow patterns
- [[reliability]] - Memory drift and state management
