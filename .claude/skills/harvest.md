---
skill: harvest
description: Teach a concept using paragraph → 3 bullets → quiz pattern
args: topic or concept to explain
---

# Harvest Learning Pattern

Extract a concept from the conversation context or specified topic and teach it using a structured learning format.

## Instructions

When this skill is invoked:

1. **Identify the concept** - Either use the topic provided in args, or extract the most relevant concept from recent conversation context

2. **Write a concise explanatory paragraph** - Explain the core concept in 3-5 sentences that capture what it is, why it matters, and how it works

3. **Provide three bullet points** - Expand on the paragraph with three key points that:
   - Clarify important details
   - Provide context or comparison
   - Highlight practical implications or tradeoffs

4. **Create a multiple-choice question** - Test comprehension with:
   - One question that gauges understanding of a key concept from the explanation
   - Four answer choices (A, B, C, D)
   - Exactly one correct answer
   - Distractors that are plausible but clearly wrong if the material was understood

## Format

Use this exact structure:

```markdown
## [Concept Name]

[Concise paragraph explaining the concept]

**Key points:**

- **[Point 1 heading]**: [Explanation]
- **[Point 2 heading]**: [Explanation]
- **[Point 3 heading]**: [Explanation]

**Comprehension check:**

[Question text]

A) [Answer option]
B) [Answer option]
C) [Answer option]
D) [Answer option]
```

## Examples

### Example 1: Teaching Cloudflare Workflows

```markdown
## Cloudflare Workflows

Cloudflare Workflows lets you build multi-step applications that can run for seconds, minutes, days, or even weeks without worrying about serverless timeouts or infrastructure failures. Instead of a normal serverless function that has to complete in 30 seconds or fail, Workflows breaks your code into "durable steps" that automatically checkpoint their progress—so if a step fails or you need to pause for hours waiting for a webhook or user approval, the workflow remembers where it was and picks up exactly where it left off.

**Key points:**

- **Eliminates timeout constraints**: Traditional serverless functions die after 30-60 seconds; Workflows can `step.sleep()` for days and resume execution automatically
- **Durable execution with state**: Each workflow step automatically saves its progress, so crashes or pauses don't lose work—it resumes from the last completed step
- **Event-driven pauses**: Your workflow can pause waiting for external events using `step.do()`, and Cloudflare handles the waiting/resuming infrastructure

**Comprehension check:**

What happens if a Cloudflare Workflow crashes in the middle of step 5 out of 10 steps?

A) The entire workflow starts over from step 1
B) The workflow resumes from step 5 because each step's completion is automatically saved
C) The workflow fails permanently and you must manually retry it
D) The workflow skips step 5 and continues from step 6 to avoid the error
```

### Example 2: Teaching Vector Embeddings

```markdown
## Vector Embeddings

Vector embeddings transform text, images, or other data into arrays of numbers (vectors) that capture semantic meaning in a way that similar concepts end up close together in multi-dimensional space. Instead of matching exact keywords, embeddings let you find conceptually similar content—"puppy" and "dog" would have nearby vectors even though the words are different, enabling semantic search, recommendation systems, and RAG applications that understand meaning rather than just matching strings.

**Key points:**

- **Semantic similarity as geometry**: Embeddings encode meaning as positions in high-dimensional space (often 768-1536 dimensions), where distance between vectors represents conceptual similarity—letting algorithms use math (cosine similarity, dot products) to find related concepts
- **Model-dependent representations**: Different embedding models create different vector spaces—text-embedding-ada-002 generates different numbers than sentence-transformers, and you can't mix embeddings from different models because their coordinate systems are incompatible
- **Fixed at indexing time**: You generate embeddings when indexing content and again when querying, then compare them—if you change models, you must re-embed all existing content or results will be meaningless

**Comprehension check:**

Why can't you use embeddings from text-embedding-ada-002 to search a vector database that was indexed using sentence-transformers/all-MiniLM-L6-v2?

A) Different models use different programming languages that are incompatible
B) Each model creates its own coordinate system, so vectors from different models can't be meaningfully compared
C) Vector databases can only store embeddings from one model at a time
D) Sentence-transformers is outdated and no longer supported
```

## Usage

Invoke with a specific topic:
```
/harvest "rust ownership"
/harvest cloudflare durable objects
```

Or invoke without args to teach a concept from current context:
```
/harvest
```

## Notes

- Keep paragraphs concise (3-5 sentences)
- Bullet points should expand on the paragraph, not repeat it
- Questions should test understanding, not memorization
- Wrong answers should be plausible but clearly incorrect if material was understood
- Use technical accuracy—don't oversimplify to the point of being wrong
