---
tags: [cloudflare, sandbox, containers, code-execution]
created: 2026-02-16
difficulty: intermediate
status: complete
---

# Cloudflare Sandbox

Cloudflare Sandbox gives you secure, isolated containers where you can run code, execute shell commands, and even expose web services—perfect for building code interpreters, CI/CD systems, or anywhere you need to safely run untrusted code. It's like having a temporary computer that spins up in seconds and disappears when you're done.

Reference: https://sandbox.cloudflare.com/

## Capabilities

**Long-Running Processes**
- Execute extended computational tasks safely
- No time limits typical of serverless functions

**Real-Time Streaming**
- Monitor stdout/stderr output live during execution
- Track progress of long-running operations

**Preview URLs**
- Instantly create public URLs for container ports
- Automatic subdomain routing
- Share running applications without deployment

**Code Interpreter**
- Run Python/JavaScript with parsed visual outputs
- Display charts, tables, images directly
- Perfect for data analysis and visualization

**File System Operations**
- Perform filesystem tasks
- Clone Git repositories
- Manage files programmatically

**Interactive Terminals**
- Full PTY support with xterm-256color emulation
- Connect interactive CLI tools
- Debug running processes

**Cloud Storage Mounting**
- Integrate S3-compatible buckets as local filesystem paths
- Access R2 buckets directly from sandbox

**Custom Docker Images**
- Add sandbox capabilities to existing Docker images
- Bring your own runtime environment

**AI Coding Agents**
- Run assistants like Claude Code with preconfigured environments
- Isolated execution for AI-generated code

## Integration

Use the `@cloudflare/sandbox` npm package within Cloudflare Workers:

```typescript
import { Sandbox } from '@cloudflare/sandbox';

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const sandbox = await Sandbox.create();

    // Execute code
    const result = await sandbox.run('python', ['-c', 'print("Hello from sandbox")']);

    // Get preview URL for service on port 8080
    const url = await sandbox.getPreviewUrl(8080);

    return Response.json({ result, url });
  }
};
```

## Use Cases

- **Automated testing** - Run tests in isolated environments
- **Code execution** - Execute user-submitted code safely
- **Service exposure** - Quickly expose development services
- **AI agent deployment** - Run autonomous coding agents
- **Data analysis** - Execute analytical workloads with visualization

## Related Topics

- [[containers]] - Long-running container deployment
- [[workers]] - Serverless integration
- [[agents]] - AI agent environments

## Resources

- [Sandbox Documentation](https://sandbox.cloudflare.com/)
- [Sandbox SDK (@cloudflare/sandbox)](https://www.npmjs.com/package/@cloudflare/sandbox)
