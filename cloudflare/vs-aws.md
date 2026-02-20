---
tags: [cloudflare, aws, serverless, edge-computing, architecture, cost]
created: 2026-02-20
difficulty: intermediate
status: complete
---

# Cloudflare vs AWS

When spinning up bespoke apps and keeping costs low, Cloudflare's platform is often a better fit than AWS — not because it replaces AWS entirely, but because its pricing model and primitives are more naturally aligned with small, independent workloads.

## Why Cloudflare Wins for Bespoke/Low-Cost Apps

- **CPU-time billing, not wall-time** — Workers charge for actual execution, not idle waiting. Apps blocked on APIs, LLMs, or user input cost nothing while waiting.
- **No provisioning overhead** — no instances, clusters, or VPCs to manage. Deploy a Worker and it's globally available instantly.
- **Edge-native by default** — 330+ cities globally, compute runs close to users without extra config.
- **Durable Objects** — stateful micro-servers that scale to millions of instances. Perfect for per-user or per-tenant isolation without provisioning a database or server per customer.
- **Generous free tier** — 100k requests/day on Workers free tier, making experimentation essentially free.

## Where AWS Still Wins

- **Heavy compute / GPU workloads** — training, large batch jobs, SageMaker.
- **Complex enterprise compliance** — full VPC isolation, regulatory certifications, audit tooling.
- **Custom model inference** — Cloudflare's Workers AI is limited to their curated model catalog; no fine-tuning or BYO weights.
- **Deep ecosystem** — RDS, SQS, Step Functions, and hundreds of other managed services with mature tooling.

## Bottom Line

For bespoke apps where you want fast iteration, global reach, and minimal idle costs, Cloudflare is genuinely better architected than AWS. For heavy infrastructure, compliance-heavy workloads, or custom ML, AWS still wins.

## Related Topics

- [[workers]] - Workers platform and best practices
- [[durable-objects]] - Stateful coordination at scale
- [[workers-ai]] - Cloudflare's inference layer
