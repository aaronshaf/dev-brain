---
tags: [cloudflare, containers, docker, edge-computing]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Cloudflare Containers

Cloudflare Containers let you deploy Docker containers to 300+ cities worldwide with zero infrastructure setup. Write a Dockerfile, run one command, and your container runs globally on Cloudflare's edge—currently in public beta.

Reference: https://containers.cloudflare.com/

## Key Features

**Simple Deployment**
1. Write a Dockerfile
2. Add `compatibility_flags = ["containers"]` to wrangler.toml
3. Run `wrangler deploy`

**Global by Default**
- Deploy to "Region: Earth" with automatic global distribution
- No manual region configuration

**Docker-Native**
- Full Dockerfile support
- Use custom images from any registry

**Worker Integration**
- Use containers alongside Workers, Workflows, Queues, and AI services
- Unified platform experience

## Use Cases

- Stateless and stateful routing
- Regional placement
- Workflow and Queue integrations
- AI-generated code execution
- Short-lived workloads

## Concurrent Instance Limits (Feb 2026)

Significantly increased limits, a 15x increase over previous caps:

| Resource | Limit |
|----------|-------|
| Memory | 6 TiB |
| vCPUs | 1,500 |
| Disk | 30 TB |

## Docker-in-Docker (Feb 2026)

Containers now support Docker-in-Docker, enabling CI/CD pipelines, container builds, and nested container workflows directly on Cloudflare's edge.

## Beta Limitations

- No built-in autoscaling or load balancing (manual scaling in code)
- Limited orchestration tools for multi-service deployments

## Example Deployment

**Dockerfile:**
```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "server.js"]
```

**wrangler.toml:**
```toml
name = "my-container-app"
compatibility_flags = ["containers"]
```

**Deploy:**
```bash
wrangler deploy
```

## Containers vs Sandbox

| Feature | Containers | Sandbox |
|---------|-----------|---------|
| **Duration** | Short-lived workloads | Long-running processes |
| **Deployment** | Global edge distribution | On-demand execution |
| **Use case** | Production services | Code execution, testing |
| **Configuration** | Dockerfile + wrangler.toml | SDK in Workers |
| **Persistence** | Not designed for persistent | Ephemeral |

## Related Topics

- [[sandbox]] - Sandboxed code execution
- [[workers]] - Serverless functions
- [[workflows]] - Container orchestration

## Resources

- [Containers Documentation](https://developers.cloudflare.com/containers/)
- [Containers Product Page](https://containers.cloudflare.com/)
