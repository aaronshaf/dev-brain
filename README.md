# Dev Brain

A knowledge base for modern software development, focusing on AI-assisted coding, cloud platforms, and tooling. Built with Obsidian and maintained by Claude Code.

## What's Inside

### Agentic Coding (`agentic-coding/`)
AI-driven development patterns, tools, and frameworks:
- **Patterns & Anti-Patterns** - Best practices for working with AI agents
- **Agent Systems** - OpenClaw, Taskmaster, Ralph Loop
- **Integration** - WebMCP, PRD writing, conversational work
- **Knowledge Systems** - Obsidian + Claude Code workflows

[Browse agentic-coding →](agentic-coding/)

### Cloudflare Platform (`cloudflare/`)
Serverless development on Cloudflare's edge network:
- **Workers** - Edge compute, best practices, configuration
- **AI Services** - Workers AI, AI Search, Vectorize
- **Infrastructure** - Workflows, Agents, Sandbox, Containers
- **Networking** - Tunnels, Zero Trust

[Browse cloudflare →](cloudflare/)

### General Topics
- **[[obsidian]]** - Knowledge management with Obsidian
- **[[mermaid]]** - Text-based diagrams for documentation
- **[[github-actions]]** - CI/CD automation
- **[[meilisearch]]** - Fast search engine

## Documentation Philosophy

Every note starts with a **super accessible 2-sentence intro** that explains what it is and why it matters in plain language. Technical details come later. This makes the vault friendly for beginners while staying comprehensive for experts.

**Example:**
> Zero Trust means nobody gets access by default—not even people inside your network. Every person and device must prove who they are before accessing any resource, making it way more secure than traditional VPNs.

See `CLAUDE.md` for complete documentation standards.

## Structure

```
dev-brain/
├── agentic-coding/        # AI-assisted development
│   ├── patterns.md
│   ├── anti-patterns.md
│   ├── tooling.md
│   ├── code-factory.md
│   ├── openclaw.md
│   ├── taskmaster.md
│   ├── ralph-looping.md
│   ├── prd.md
│   ├── conversational-work.md
│   ├── webmcp.md
│   └── obsidian-claude-system.md
├── cloudflare/            # Cloudflare platform docs
│   ├── workers.md
│   ├── workers-ai.md
│   ├── ai-search.md
│   ├── vectorize.md
│   ├── workflows.md
│   ├── agents.md
│   ├── sandbox.md
│   ├── containers.md
│   ├── tunnels.md
│   └── zero-trust.md
├── obsidian.md            # Obsidian workflows
├── mermaid.md             # Diagram syntax
├── github-actions.md      # CI/CD automation
├── meilisearch.md         # Search engine
├── CLAUDE.md              # Documentation standards
└── README.md              # This file
```

## Tools & Integrations

### Note-Taking
- **[Obsidian](https://obsidian.md)** - Primary interface for the vault
- **Obsidian CLI** - Automation for vault operations (54× faster searches)

### AI Development
- **[Claude Code](https://claude.com/code)** - AI-assisted development and knowledge capture
- **Taskmaster** - AI task management via MCP
- **Ralph Loop** - Autonomous coding loops
- **OpenClaw** - 24/7 AI agents

### Platforms
- **Cloudflare Workers** - Edge compute platform
- **GitHub Actions** - CI/CD automation
- **Meilisearch** - Fast search engine

## How to Use

### As an Obsidian Vault

1. **Open in Obsidian:**
   ```bash
   # Clone the repo
   git clone https://github.com/aaronshaf/dev-brain.git

   # Open as vault in Obsidian
   # File → Open Vault → dev-brain/
   ```

2. **Explore with graph view** - See connections between topics
3. **Search with tags** - Find notes by topic: `#cloudflare`, `#ai-agents`, etc.
4. **Follow wiki links** - `[[note-name]]` links connect related concepts

### With Claude Code

The vault is optimized for AI-assisted workflows:

1. **Read `CLAUDE.md`** - Contains documentation standards and conventions
2. **Use accessible intros** - Every note starts beginner-friendly
3. **Leverage Mermaid diagrams** - Visual explanations for complex topics
4. **Follow linking patterns** - Build knowledge networks

### Contributing Knowledge

When adding new content:

1. **Start with accessible intro** - 1-2 sentences, plain language
2. **Use proper frontmatter** - Tags, dates, difficulty, status
3. **Add Mermaid diagrams** - For architecture, flows, or complex concepts
4. **Link related topics** - Connect concepts with `[[wiki-links]]`
5. **Route to right location** - Follow the structure guidelines

See `CLAUDE.md` for complete guidelines.

## Key Features

- **Beginner-friendly intros** on every page
- **Comprehensive Mermaid diagrams** for visual learning
- **Organized by domain** (agentic coding, Cloudflare, tools)
- **AI-optimized** for Claude Code and other AI assistants
- **Plain text** - All content in Markdown, version controlled
- **Linked knowledge** - Wiki-style connections between concepts

## Topics Covered

### AI & Agents
- Agentic coding patterns and anti-patterns
- OpenClaw deployment and security
- Taskmaster for task management
- Ralph Loop autonomous workflows
- WebMCP for browser agent interactions

### Cloud Platforms
- Cloudflare Workers and edge compute
- Workers AI and AI Search
- Durable Workflows and Agents SDK
- Zero Trust networking
- Tunnels and Containers

### Developer Tools
- Obsidian knowledge management
- GitHub Actions CI/CD
- Meilisearch for search
- Mermaid diagrams

## Philosophy

This vault follows these principles:

1. **Accessible first** - Plain language intros, technical depth follows
2. **Visual explanations** - Mermaid diagrams for complex concepts
3. **AI-assisted** - Written with and for AI coding tools
4. **Organized by domain** - Clear structure, no sprawl
5. **Plain text forever** - Markdown files you own and control

## Getting Started

### New to the vault?
Start here: **[[agentic-coding/README|Agentic Coding Overview]]**

### Learning Cloudflare?
Start here: **[[cloudflare/index|Cloudflare Platform]]**

### Working with Obsidian?
Start here: **[[obsidian|Obsidian Workflows]]**

---

**Built with** [Obsidian](https://obsidian.md) • [Claude Code](https://claude.com/code) • [Mermaid](https://mermaid.js.org)
