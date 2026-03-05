---
tags: [cloudflare, queues, messaging, async, workers]
created: 2026-03-05
difficulty: beginner
status: draft
---

# Cloudflare Queues

Queues let you send messages from one Worker and process them in another, decoupling your producers from consumers. Think of it as a reliable to-do list between your services -- one side adds tasks, the other side works through them at its own pace.

## Free Plan (Feb 2026)

Queues are now available to **all accounts on the Workers Free plan**, including 10,000 daily operations. Previously required a paid plan.

## Key Features

- **At-least-once delivery** - Messages are guaranteed to be delivered
- **Batching** - Consumer receives messages in configurable batches
- **Retry with backoff** - Failed messages automatically retry
- **Dead letter queues** - Capture messages that exhaust retries
- **Fan-out** - Multiple consumers can process from the same queue

## Basic Usage

### wrangler.toml

```toml
[[queues.producers]]
binding = "MY_QUEUE"
queue = "my-queue"

[[queues.consumers]]
queue = "my-queue"
```

### Producer

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    await env.MY_QUEUE.send({ url: request.url, timestamp: Date.now() });
    return new Response("Queued");
  }
};
```

### Consumer

```typescript
export default {
  async queue(batch: MessageBatch, env: Env): Promise<void> {
    for (const msg of batch.messages) {
      console.log(msg.body);
      msg.ack();
    }
  }
};
```

## Queues vs Workflows

| Feature | Queues | Workflows |
|---------|--------|-----------|
| **Best for** | Single-step async jobs | Multi-step processes |
| **Duration** | Seconds to minutes | Seconds to weeks |
| **State** | Stateless | Stateful |
| **Delivery** | At-least-once | Exactly-once per step |
| **Control** | Fire-and-forget | Pause, resume, terminate |

Use both together: Queues as high-throughput entry point, Workflows for complex processing.

## Related Topics

- [[workers]] - Producers and consumers
- [[workflows]] - Complex multi-step orchestration
- [[pipelines]] - High-volume event ingestion

## Resources

- [Queues Documentation](https://developers.cloudflare.com/queues/)
- [Queues Pricing](https://developers.cloudflare.com/queues/platform/pricing/)
