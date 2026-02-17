---
tags: [openclaw, cost, optimization, models, spending, budget]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Cost Optimization

Running agents 24/7 can burn money fast if you're not careful with model selection and spending limits. These patterns keep costs predictable while maintaining quality output.

## Model Economics (February 2026)

| Model | Input | Output | Use Case | Monthly Cost |
|-------|--------|--------|----------|--------------|
| Kimi K2.5 | $0.0003/K | $0.0011/K | Primary model (70-80% tasks) | $5-20 |
| Sonnet 4.5 | $0.003/K | $0.015/K | Fallback, execution tasks | Included in total |
| Opus 4.6 | $0.015/K | $0.075/K | External content only | $10-30 |

**Recommended setup:**
1. **Primary:** Kimi K2.5 (cheap, fast, agentic)
2. **Fallback:** Sonnet 4.5 (auto-switches if Kimi rate-limited)
3. **External content:** Opus 4.6 (strongest security)

**Setup:**

```bash
# Configure primary + fallback
openclaw models auth add  # Add Kimi
openclaw models auth add  # Add Anthropic

# Set fallback
openclaw models fallbacks add anthropic/claude-sonnet-4-5

# Register aliases for easy switching
openclaw config set agents.defaults.models '{
  "moonshotai/kimi-k2.5": { "alias": "kimi" },
  "anthropic/claude-sonnet-4-5": { "alias": "sonnet" },
  "anthropic/claude-opus-4-6": { "alias": "opus" }
}'

# Switch models in chat
/model opus    # for reading external content
/model kimi    # back to default
```

## Spending Limits

**Moonshot (Kimi K2.5):**
- Prepaid credits (safer - can't overspend)
- Start with $5-10
- No auto-reload initially
- https://platform.moonshot.ai

**Anthropic (Claude):**
```bash
# Set at console.anthropic.com
Daily limit: $5/day
Monthly limit: $50/month
Alerts: 50% and 80%
```

## Cost Tracking

```bash
# Monitor usage
openclaw status --usage

# Track cost per deliverable
# Flag anything >2x budget for review
```

**Example cost breakdown:**
- TikTok slideshow (6 images): $0.50 with real-time API, $0.25 with batch API
- Research report: $0.15 (Sonnet)
- External content analysis: $0.45 (Opus)

**Red flag:** 200K tokens for a simple task = 12x over budget. Investigate immediately.

## Cost Control Per Deliverable

**Track cost per deliverable, not per run:**

```python
# cost-tracker.py
deliverables = {
    "tiktok-slideshow": {
        "budget": 0.50,
        "actual": [0.52, 3.12, 0.48],  # Attempt 1, 2, 3
        "status": ["success", "hallucination", "success"]
    },
    "research-report": {
        "budget": 0.15,
        "actual": [0.14, 0.16],
        "status": ["success", "success"]
    }
}

# Flag anything >2x budget
for task, data in deliverables.items():
    for cost in data["actual"]:
        if cost > data["budget"] * 2:
            alert(f"⚠️  {task} cost ${cost} (budget ${data['budget']})")
```

## Related Topics

- [[security]] - Model selection for external content
- [[reliability]] - Token burn from context bloat
- [[patterns]] - Efficient cron vs heartbeat patterns
