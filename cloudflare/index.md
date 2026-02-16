---
tags: [cloudflare, serverless, platform, index]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Cloudflare Platform

Cloudflare's developer platform enables building serverless applications with AI capabilities, durable workflows, and edge compute - all without managing infrastructure.

## Documentation Structure

- **[[workers]]** - Workers runtime, best practices, and configuration
- **[[ai-search]]** - AI Search and Vectorize for semantic search and RAG
- **[[workers-ai]]** - AI models, Workers AI, and inference
- **[[workflows]]** - Durable execution and multi-step processes
- **[[agents]]** - Building autonomous AI agents
- **[[sandbox]]** - Containerized execution environments
- **[[containers]]** - Docker containers on the edge
- **[[libraries]]** - SDKs and integration libraries

## Quick Start

Add Cloudflare skills to your project:
```bash
npx skills add cloudflare/skills
```

## Core Concepts

Cloudflare's platform is built on several key primitives:

- **Workers** - Serverless JavaScript/TypeScript runtime at the edge
- **Durable Objects** - Stateful coordination with SQLite and WebSockets
- **R2** - S3-compatible object storage
- **D1** - SQLite-based serverless SQL database
- **KV** - Low-latency key-value storage
- **Queues** - Message queuing for async processing
- **Workflows** - Multi-step durable execution
- **Workers AI** - Serverless AI inference with 50+ models
- **Vectorize** - Vector database for semantic search
- **AI Search** - Managed RAG and search pipeline

## Official Resources

- [Cloudflare Developers](https://developers.cloudflare.com/)
- [Workers Documentation](https://developers.cloudflare.com/workers/)
- [Discord Community](https://discord.gg/cloudflaredev)

## Related Topics

- [[serverless-architecture]]
- [[edge-computing]]
- [[ai-agents]]
- [[durable-execution]]
