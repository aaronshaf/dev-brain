---
tags: [cloudflare, workers, serverless, edge-computing, best-practices]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Cloudflare Workers

Cloudflare Workers run your JavaScript/TypeScript code on Cloudflare's global network, executing in milliseconds right next to your users instead of in one distant data center. Think AWS Lambda but faster, cheaper, and automatically distributed worldwide.

## Best Practices

Reference: [Workers Best Practices](https://developers.cloudflare.com/workers/best-practices/workers-best-practices/)

### Configuration

**Keep Compatibility Date Current**
- Set `compatibility_date` to today's date on new projects
- Update periodically on existing projects to access new APIs and fixes without code changes

**Enable nodejs_compat**
- The `nodejs_compat` flag grants access to Node.js built-in modules (`node:crypto`, `node:buffer`, `node:stream`)
- Prevents cryptic import errors at runtime for libraries depending on these modules

**Generate Binding Types**
```bash
wrangler types  # Generates type definitions matching your Wrangler configuration
```
Never hand-write your `Env` interface - catch mismatches at compile time rather than deployment.

**Store Secrets Securely**
```bash
wrangler secret put SECRET_NAME
```
- Never put secrets in Wrangler configuration or source code
- Access through `env` at runtime
- Use `.env` file (in `.gitignore`) for local development

**Configure Environments Deliberately**
- Use Wrangler environments for production, staging, and development
- Each environment creates a distinct Worker named `{name}-{env}`
- Treat root configuration as shared settings, override per environment

**Set Up Custom Domains or Routes**
- **Custom domains**: Worker is the origin; Cloudflare manages DNS and SSL
- **Routes**: Worker runs in front of existing origin; requires proxied DNS record first
- Without proxied DNS record, route requests fail with `ERR_NAME_NOT_RESOLVED`

### Request and Response Handling

**Stream Request and Response Bodies**
- ⚠️ Buffering entire bodies (`await response.text()`, `await request.arrayBuffer()`) can crash Workers (128 MB memory limit)
- Use `TransformStream` to pipe data without holding everything in memory
- Enforce maximum size before reading consumed request bodies (JSON, file uploads)

**Use waitUntil for Work After Response**
```javascript
ctx.waitUntil(logAnalytics());  // Background work after response sent
```
- Keeps responses fast while completing background tasks
- ⚠️ Avoid destructuring `ctx` (loses `this` binding, causes "Illegal invocation" errors)
- 30-second time limit after response is sent

### Architecture

**Use Bindings for Cloudflare Services**
- Services (R2, KV, D1, Queues, Workflows) available as bindings
- Direct, in-process references requiring no network hop or authentication
- Avoid calling REST APIs from Workers for these services

**Use Queues and Workflows for Async Work**

**Queues**:
- Decouple producer from consumer
- Best for fan-out scenarios, message buffering, single-step background jobs
- Provides at-least-once delivery

**Workflows**:
- Handle multi-step processes with dependencies
- Ideal for complex conditional logic and long-running tasks (hours/days/weeks)
- Can pause and resume

Use both together: high-throughput entry point feeding into complex processing.

**Use Service Bindings for Worker-to-Worker Communication**
- Zero-cost, bypass public internet
- Support type-safe RPC instead of HTTP requests to public URLs

**Use Hyperdrive for External Database Connections**
- Always use Hyperdrive for remote PostgreSQL or MySQL databases
- Maintains regional connection pools, eliminating per-request TCP handshakes and TLS negotiation
- Create new `Client` on each request (Hyperdrive manages underlying pool)
- Requires `nodejs_compat`

**Use Durable Objects for WebSockets**
- Plain Workers lack persistent state and hibernation for WebSocket connections
- Use Durable Objects with Hibernation API for reliable, long-lived connections
- Use `this.ctx.acceptWebSocket()` instead of `ws.accept()` to enable hibernation
- Use `setWebSocketAutoResponse` for ping/pong heartbeats

**Use Workers Static Assets**
- Recommended approach for deploying static sites, SPAs, and full-stack apps
- Point `assets.directory` at build output for static sites
- Add `main` entry point and `ASSETS` binding for full-stack apps

### Observability

**Enable Workers Logs and Traces**
- Production Workers without observability are a black box
- Enable before deploying to production
- Use `head_sampling_rate` to control volume and costs
- Use structured JSON logging with `console.log` for searchable, filterable logs
- Use `console.error` for errors, `console.warn` for warnings (correct severity levels)

### Code Patterns

**Do Not Store Request-Scoped State in Global Scope**
- ⚠️ Workers reuse isolates across requests
- Variables set during one request persist into the next (cross-request data leaks, stale state)
- Pass state through function arguments or store on `env` bindings
- Never use module-level variables for request state

**Always Await or waitUntil Your Promises**
- ⚠️ Floating promises (not `await`ed, `return`ed, or passed to `ctx.waitUntil()`) cause silent bugs
- Dropped results, swallowed errors, unfinished work
- Enable `no-floating-promises` lint rule

### Security

**Use Web Crypto for Secure Token Generation**
```javascript
crypto.randomUUID()              // Unique identifiers
crypto.getRandomValues(buffer)   // Random bytes
crypto.subtle.timingSafeEqual()  // Timing-safe comparison
```
- ⚠️ Never use `Math.random()` for security-sensitive operations
- Use `crypto.subtle.timingSafeEqual()` to prevent timing side-channel attacks
- Hash both values to fixed size first (avoiding length leaks)

**Do Not Use passThroughOnException as Error Handling**
- `passThroughOnException()` is fail-open mechanism (sends requests to origin on unhandled exceptions)
- Use explicit try/catch blocks with structured error responses instead

### Development and Testing

**Test with @cloudflare/vitest-pool-workers**
- Runs tests inside Workers runtime
- Provides access to real bindings (KV, R2, D1, Durable Objects)
- Catches issues that Node.js-based tests miss
- **Note**: Automatically injects `nodejs_compat` - confirm your `wrangler.jsonc` includes this flag if your code depends on Node.js built-in modules

## Example Worker

```typescript
export default {
  async fetch(request: Request, env: Env, ctx: ExecutionContext): Promise<Response> {
    // Stream response (don't buffer)
    const upstream = await fetch('https://example.com');

    // Background logging (don't block response)
    ctx.waitUntil(logRequest(request, env));

    return new Response(upstream.body, {
      headers: upstream.headers
    });
  }
};

async function logRequest(request: Request, env: Env) {
  await env.ANALYTICS.put(
    crypto.randomUUID(),
    JSON.stringify({
      url: request.url,
      timestamp: Date.now()
    })
  );
}
```

## Related Topics

- [[durable-objects]] - Stateful coordination
- [[workflows]] - Multi-step durable execution
- [[workers-ai]] - AI inference

## Resources

- [Workers Documentation](https://developers.cloudflare.com/workers/)
- [Workers Best Practices](https://developers.cloudflare.com/workers/best-practices/workers-best-practices/)
- [Workers Pricing](https://developers.cloudflare.com/workers/platform/pricing/)
