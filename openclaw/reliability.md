---
tags: [openclaw, reliability, verification, feedback, self-healing, observability]
created: 2026-02-16
difficulty: advanced
status: complete
---

# Reliability Engineering

Agents are 30% of the work—the immune system that keeps them running reliably is 70%. This covers verification patterns, feedback loops, self-healing audits, and failure detection for production deployments.

## The Immune System Architecture

```
┌────────┐ ┌────────┐ ┌────────┐
│ Agent  │ │ Agent  │ │ Agent  │
└───┬────┘ └───┬────┘ └───┬────┘
    └──────────┼──────────┘
         ┌─────┴─────┐
         │ Feedback  │ ← approve/reject with reasons
         │   Loop    │
         └─────┬─────┘
         ┌─────┴─────┐
         │   Self    │ ← Sunday audit, auto-restarts
         │  Healer   │
         └─────┬─────┘
         ┌─────┴─────┐
         │  Verify   │ ← check files exist, hit endpoints
         │  Layer    │
         └─────┬─────┘
         ┌─────┴─────┐
         │  Budget   │ ← cost tracking per deliverable
         │ Controls  │
         └───────────┘
```

**Reality:** Agents are 30% of the work. The immune system is 70%.

## 1. Output Verification

**Problem:** Agent reports "task complete" but produces nothing.

```
AGENT REPORT          vs.      REALITY
✅ Script created              (empty)
✅ Dashboard done              no files exist
✅ Cron added
✅ Tests passing
200K tokens, $3.12            zero output
```

**Fix: Post-run verification**

```bash
# After every agent run, verify filesystem
ls -la /path/to/output

# Check file contents (not just existence)
wc -l output.py  # Should be >0 lines

# Hit API endpoints if applicable
curl -s http://localhost:3000/health | jq .status

# Only mark success if verification passes
```

**Pattern for skills:**

```markdown
# After generating output:
1. Verify file exists: ls -la output.py
2. Verify file has content: wc -l output.py
3. Run basic validation: python -m py_compile output.py
4. Report specific findings: "File exists (247 lines), compiles successfully"
```

## 2. Feedback Loops

**Problem:** Agent repeats same mistake forever.

```
Mon: Recommends "Jack from 24" → ❌ Reject (wrong fit)
Tue: Recommends "Jack from 24" → ❌ Reject
Wed: Recommends "Jack from 24" → ❌ Reject
     (Groundhog Day - no memory of feedback)
```

**Fix: feedback.json system**

```json
{
  "recommendations": [
    {
      "date": "2026-02-12",
      "item": "Jack from 24",
      "decision": "reject",
      "reason": "Company too small, not our ICP",
      "lesson": "Avoid companies <50 employees"
    },
    {
      "date": "2026-02-13",
      "item": "Sarah from Datadog",
      "decision": "approve",
      "reason": "Perfect ICP - enterprise infra, 500+ employees",
      "lesson": "Prioritize enterprise infrastructure companies"
    }
  ]
}
```

**Agent workflow:**
1. **Before generating** → read feedback.json
2. Apply learned lessons
3. Generate recommendation
4. **After user feedback** → append to feedback.json

**Results:**
- Before feedback: 15% approval rate
- After feedback: 60-100% approval rate

## 3. Self-Healing Audit

**Problem:** Cron jobs silently die for days. No error, no alert.

```bash
# Example: 6am and 7am crons never fired for 5 days
# Root cause: delivery: "announce" instead of "none"

WORKING (2pm cron):
  delivery: "none"       ✅
  wakeMode: "heartbeat"  ✅

BROKEN (6am, 7am):
  delivery: "announce"   ← silent killer
  wakeMode: "now"        ← also wrong
```

**Fix: Sunday night audit**

```bash
#!/bin/bash
# self-healer.sh - runs every Sunday at 11pm

# Get all scheduled crons
crons=$(openclaw cron list --json)

# For each cron:
for cron in $crons; do
  # Get last run timestamp
  last_run=$(echo $cron | jq -r '.lastRun')
  schedule=$(echo $cron | jq -r '.schedule')

  # Compare against expected schedule
  expected=$(calculate_expected_runs $schedule)
  actual=$(calculate_actual_runs $last_run)

  # If missed runs detected:
  if [ $actual -lt $expected ]; then
    echo "⚠️  Cron $cron missed runs. Auto-restarting..."
    openclaw cron trigger $cron --force
    log_to_dashboard "Healed: $cron"
  fi
done
```

**Cron configuration:**
```bash
0 23 * * 0 /home/user/scripts/self-healer.sh
```

**What it catches:**
- Silent cron failures
- Config issues
- Gateway restart timing problems

**Output:** Dashboard report of what it fixed automatically.

## 4. Gateway Restart Protection

**Problem:** Config change at 11pm → gateway restarts → 7:30am cron window passes during boot → job skipped, no retry.

**Fix patterns:**

```bash
# 1. Batch config changes (avoid near cron windows)
# 2. Watchdog redundancy

# Watchdog: if morning digest doesn't arrive by 2hrs past schedule, alert
0 9 * * * /home/user/scripts/check-morning-digest.sh

# check-morning-digest.sh
if ! grep -q "$(date +%Y-%m-%d)" /tmp/digest-sent.log; then
  echo "❌ Morning digest never sent! Manual trigger needed."
  openclaw cron trigger morning-digest --force
fi
```

## Recommended Cron Jobs

Three crons worth setting up for any production deployment:

```bash
# 1. Session cleanup — every 72 hours
# Deletes bloated session files that slow down your agent
0 0 */3 * * openclaw sessions cleanup --older-than 72h

# 2. Silent backups — every 2 hours
# Git push workspace so you never lose config/memory
0 */2 * * * cd ~/.openclaw && git add -A && git commit -m "auto backup" && git push

# 3. Daily security audit — every morning (see [[security]])
0 7 * * * openclaw security audit --fix
```

## Related Topics

- [[memory]] - active-tasks.md for crash recovery
- [[patterns]] - Sub-agent verification patterns
- [[troubleshooting]] - Common failure modes
- [[cost-optimization]] - Cost tracking per deliverable
- [[security]] - Security audit details
