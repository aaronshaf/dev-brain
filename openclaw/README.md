---
tags: [ai, automation, openclaw, agents, deployment]
created: 2026-02-15
difficulty: advanced
status: complete
---

# OpenClaw

OpenClaw is a framework that lets you run AI agents 24/7 on a server, giving them memory, tools, and the ability to complete tasks while you sleep.

## Contents

- **[[deployment]]** - VPS setup, Hetzner hosting, Tailscale security
- **[[memory]]** - Memory architecture, MEMORY.md strategy, file organization
- **[[configuration]]** - AGENTS.md, TOOLS.md, SOUL.md, teaching your agent
- **[[patterns]]** - Sub-agents, cron vs heartbeats, skill routing
- **[[security]]** - SOUL.md boundaries, Docker sandbox, threat model
- **[[cost-optimization]]** - Model selection, spending limits, cost tracking
- **[[reliability]]** - Verification, feedback loops, self-healing audits
- **[[troubleshooting]]** - Common failures and fixes

## Core Features

- **Persistent memory** across sessions
- **Tool access** (filesystem, APIs, web)
- **Skill system** for specialized workflows
- **Multi-channel** communication (Telegram, Matrix, WhatsApp)

**Critical concept:** Your agent wakes up fresh every session. Memory files ARE its brain.

## Production Principles

Treat agents like distributed systems, not chatbots with tools.

**1. Drift** - Agents scatter state everywhere (AGENTS.md, MEMORY.md, disk, databases). Enforce write locations in skills. Git-track all changes. Nightly workspace review.

**2. Trust boundaries** - Untrusted input = attack surface. Separate sandboxed agents with limited skills for external content. Code routes and filters before LLM. Never reverse.

**3. Autonomy calibration** - Start read-only. Graduate to filtered write (drafts, proposals, approval queues). Full write access is earned, not default.

**4. Cost discipline** - Cache stable IDs. Use deterministic filtering to skip LLM when code can decide. Route routine tasks to cheaper models. Cheapest token is the one you never send.

**5. Observability** - Trace any failure in 5 minutes or it's not production-ready. Log webhook status. Track cron state. Commit all changes to git. Rollback is first-class, not emergency.

**Operating discipline > prompt quality.**

## References

- OpenClaw Docs: https://docs.openclaw.ai
- Security Guide: https://docs.openclaw.ai/gateway/security
- Clawdex Skill Scanner: https://clawdex.koi.security
