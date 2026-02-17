---
tags: [cloudflare, workers-ai, ai, llm, embeddings, inference]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Workers AI

Workers AI lets you run AI models like Llama and Stable Diffusion directly in your code without setting up GPUs or managing infrastructure. You call an API, Cloudflare runs the model on their GPUs worldwide, and you only pay for what you use.

## Capabilities

- **50+ curated open-source models** - Llama, Stable Diffusion, Mistral, and more
- **OpenAI-compatible API** - Drop-in replacement for existing applications
- **Serverless pricing** - Pay only for what you use, no idle infrastructure costs
- **Edge inference** - Low-latency model execution close to users
- **Multiple modalities** - Text generation, image classification, object detection, transcription, TTS, embeddings

## GLM-4.7-Flash Model

New multilingual text generation model (announced Feb 13, 2026) with exceptional capabilities:

- **131,072 token context window** - Handle lengthy documents and complex reasoning
- **Multi-turn tool calling** - AI agents can invoke functions across conversation turns
- **Multilingual support** - Generate content across multiple languages
- **Fast inference** - Optimized for low-latency chatbot and assistant applications
- **Strong instruction-following** - Excellent for code generation and structured tasks

**Access options:**
- Workers AI binding (inside Workers)
- REST API endpoints
- AI Gateway
- Vercel AI SDK via `workers-ai-provider`

## Pricing

- **Rate:** $0.011 per 1,000 Neurons
- **Free tier:** 10,000 Neurons per day (included in Free and Paid plans)
- **Neuron mapping:** Each model's input format (tokens, audio seconds, images) maps to Neurons
- **Third-party billing:** Pay for OpenAI, Anthropic, etc. usage through Cloudflare invoice (small convenience fee)

Reference: [Workers AI Pricing](https://developers.cloudflare.com/workers-ai/platform/pricing/)

## Libraries and SDKs

### @cloudflare/tanstack-ai

New library (Feb 2026) integrating TanStack AI with Cloudflare infrastructure.

**Configuration modes:**
1. Plain binding (inside Workers)
2. Plain REST (external)
3. AI Gateway binding
4. AI Gateway REST

**Supported features:**
- Chat with tool calling and structured output
- Image generation
- Transcription (speech-to-text)
- Text-to-speech
- Text summarization
- Third-party provider routing (OpenAI, Anthropic, Gemini) through AI Gateway

**Playground:** https://tanstack-ai-playground.threepointone.workers.dev

### workers-ai-provider (v3.1.1)

Vercel AI SDK provider for Workers AI with major improvements.

**New capabilities (v3.1.x):**
- **Transcription** - Speech-to-text with automatic model-specific input handling
- **Text-to-speech** - Voice and speed customization
- **Document reranking** - For RAG pipelines

**Reliability fixes (v3.0.5):**
- Proper token-by-token streaming
- Improved tool calling with preserved conversation history
- Detection of premature stream termination

**Playground:** https://workers-ai-provider-playground.threepointone.workers.dev

Quote from [@threepointone](https://x.com/threepointone/status/2022358006588149882):
> "We spent the past couple of weeks fixing tons of bugs in our libs related to streaming, tool calling, rendering, just everything. It's gone from being janky to very very smooth. Every model shines now."

## Example Usage

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const response = await env.AI.run(
      '@cf/meta/llama-3.1-8b-instruct',
      {
        messages: [
          { role: 'system', content: 'You are a helpful assistant.' },
          { role: 'user', content: 'Explain Cloudflare Workers' }
        ]
      }
    );

    return Response.json(response);
  }
};
```

## Related Topics

- [[ai-search]] - RAG and semantic search
- [[agents]] - Building AI agents
- [[workers]] - Workers integration

## Resources

- [Workers AI Documentation](https://developers.cloudflare.com/workers-ai/)
- [Workers AI Product Page](https://workers.cloudflare.com/product/workers-ai/)
- [Workers AI Pricing](https://developers.cloudflare.com/workers-ai/platform/pricing/)
- [Changelog](https://developers.cloudflare.com/changelog/2026-02-13-glm-47-flash-workers-ai/)
