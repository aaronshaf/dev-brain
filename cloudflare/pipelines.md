---
tags: [cloudflare, pipelines, data-ingestion, streaming]
created: 2026-03-05
difficulty: beginner
status: draft
---

# Cloudflare Pipelines

Pipelines let you ingest high volumes of events and stream them into storage (like R2) without building your own data pipeline infrastructure. You send events from Workers, and Pipelines handles batching, delivery, and schema validation automatically.

## Key Features

- **Event ingestion** from Workers into R2 buckets
- **Typed TypeScript bindings** for type-safe event publishing
- **Dropped event metrics** to identify schema mismatches when events fail validation
- **Simple setup mode** for R2 buckets with minimal configuration

## Basic Usage

### wrangler.jsonc

```jsonc
{
  "pipelines": [
    {
      "binding": "MY_PIPELINE",
      "pipeline": "my-pipeline"
    }
  ]
}
```

### Worker

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    await env.MY_PIPELINE.send({
      event: "page_view",
      url: request.url,
      timestamp: Date.now(),
    });

    return new Response("Event sent");
  }
};
```

## Use Cases

- Clickstream and analytics event ingestion
- Log aggregation into R2
- IoT data collection
- Event sourcing pipelines

## Related Topics

- [[workers]] - Event producers
- [[workflows]] - Complex multi-step processing after ingestion

## Resources

- [Pipelines Documentation](https://developers.cloudflare.com/pipelines/)
