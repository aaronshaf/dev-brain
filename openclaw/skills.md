---
tags: [openclaw, skills, distribution, clawhub, deployment]
created: 2026-02-20
difficulty: intermediate
status: complete
---

# OpenClaw Skills Distribution

Skills in OpenClaw are distributed and shared via **ClawHub**, the official public skills registry.

## ClawHub Registry

The distribution system is **ClawHub** — the public skills registry at [https://clawhub.ai](https://clawhub.ai).

### Publishing Skills

To publish your skills from a repository, use the `clawhub` CLI.

1. **Install the CLI:**
   ```bash
   npm i -g clawhub
   ```

2. **Authenticate:**
   ```bash
   clawhub login
   ```

3. **Publish a single skill:**
   ```bash
   clawhub publish ./my-skill --slug my-skill --name "My Skill" --version 1.0.0 --tags latest
   ```

4. **Sync everything at once:**
   This scans your repository and publishes any new or updated skills automatically.
   ```bash
   clawhub sync --all
   ```

## Repository Strategy

The official distribution path is through ClawHub rather than direct GitHub repository installation.

*   **Source of Truth:** Your local or GitHub repository (e.g., `~/github/skills`) serves as your development environment.
*   **Distribution:** Use `clawhub sync` to push your developed skills to the registry for others (or your other agents) to consume.

## Recommended Repo Structure

OpenClaw loads skills based on a folder containing a `SKILL.md` file. To manage multiple skills in one repository, use the following structure:

```text
skills/
├── your-skill/
│   └── SKILL.md
├── another-skill/
│   └── SKILL.md
└── README.md
```

### Automation
Run `clawhub sync --all` from the root of your `skills/` directory to publish all skills in bulk. The system uses a lockfile at `.clawhub/lock.json` to track which skills and versions have already been published, ensuring efficient updates.
