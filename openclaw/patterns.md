---
tags: [openclaw, patterns, workflows, sub-agents, cron, skills]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Agent Patterns

Stop running tasks sequentially when you can parallelize with sub-agents. These patterns are battle-tested approaches for scaling agent work and automating routine operations.

## 1. Sub-Agents: The 10x Multiplier

**Stop doing things sequentially.** Spawn 3-5 sub-agents in parallel.

**Example:** Deploying 11 websites
- Sequential: 11 × 15min = 165 minutes
- Parallel (4 agents): 3-4 websites each = ~20 minutes

**The trick:** Define clear success criteria BEFORE spawning:

```markdown
# Bad
"Deploy these 3 websites"

# Good
"Deploy websites 1-3. Success criteria:
- Site responds 200 OK at production URL
- SSL certificate valid
- Homepage loads in <2s
- Contact form submits successfully
Self-validate all criteria. Report specific pass/fail for each."
```

Each sub-agent must:
1. Complete its work
2. Validate its own output
3. Report specific pass/fail results
4. **You verify before announcing "done"**

## 2. Cron vs Heartbeats

**Use the right tool for the job:**

| Pattern | Use Case | Token Cost |
|---------|----------|------------|
| **Heartbeat** | Batch periodic checks (email + calendar + mentions) | Medium - runs every ~30min with full context |
| **Cron** | Precise schedules, independent tasks | Low - isolated context |

**Heartbeat pattern:**
```markdown
# HEARTBEAT.md (keep under 20 lines!)

Every ~30min:
- Check active-tasks.md freshness
- Session health (archive bloated sessions)
- Self-review every ~4 hours
```

**Cron pattern:**
```bash
# Precise schedules with isolated context
0 6 * * * openclaw run --skill="daily-content-ideas"
0 2 * * * openclaw run --skill="overnight-research-scout"
0 8 * * * openclaw run --skill="tech-watch"
```

**Why split them:**
- Heartbeats: continuous monitoring with shared context
- Crons: one-shot tasks with isolated context (saves tokens)

**Critical:** Keep HEARTBEAT.md under 20 lines. Heavy work goes in cron jobs.

## 3. Skill Routing Logic

Add "Use when / Don't use when" to every skill description:

```markdown
# Skill: tiktok-content

## Description
Generate 6-slide TikTok carousel with AI-generated images

## Use when
- User asks for TikTok content
- User mentions "slideshow" or "carousel"
- User provides app features to promote

## Don't use when
- User wants video content (not photos)
- User asks for Instagram/Twitter content
- Request is for written blog post

## Success criteria
- 6 images generated (1024×1536 portrait)
- Text overlay on slide 1
- Caption with hook + story
- Max 5 hashtags
- Posted as draft to TikTok
```

Without routing logic, agents misfire ~20% of the time picking wrong skills.

## Related Topics

- [[memory]] - active-tasks.md for crash recovery
- [[configuration]] - Teaching agents with AGENTS.md
- [[reliability]] - Verification and self-healing patterns
