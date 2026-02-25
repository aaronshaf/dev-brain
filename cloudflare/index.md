---
tags: [cloudflare, serverless, platform, index]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Cloudflare Platform

Cloudflare lets you build and deploy apps that run instantly worldwide without managing servers. It combines serverless functions, AI models, databases, and more into one platform where you only pay for what you use.

## Documentation Structure

- **[[workers]]** - Workers runtime, best practices, and configuration
- **[[durable-objects]]** - Stateful coordination with SQLite and WebSockets
- **[[d1]]** - SQLite-based serverless SQL database
- **[[workflows]]** - Durable execution and multi-step processes
- **[[agents]]** - Building autonomous AI agents
- **[[chat-agents]]** - Comparison of agent chat frameworks
- **[[sandbox]]** - Containerized execution environments
- **[[containers]]** - Docker containers on the edge
- **[[workers-ai]]** - AI models, Workers AI, and inference
- **[[ai-search]]** - AI Search and Vectorize for semantic search and RAG
- **[[ai-gateway]]** - AI Gateway for routing, caching, and observability
- **[[vectorize]]** - Vector database for semantic search
- **[[tunnels]]** - Cloudflare Tunnels for secure connectivity
- **[[zero-trust]]** - Zero Trust network access
- **[[vs-aws]]** - Cloudflare vs AWS cost and infra tradeoffs

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

- [[mcp]] - Model Context Protocol (Cloudflare builds MCP tooling)
- [[mcp-auth]] - MCP auth including Cloudflare's workers-oauth-provider
- [[agentic-coding/README|Agentic Coding]] - AI agent development patterns
