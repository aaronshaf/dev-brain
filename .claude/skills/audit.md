---
skill: audit
description: Run vault health checks and find issues
args: optional focus area (e.g., "frontmatter", "orphans", "links", "stale")
---

# Audit Skill

Run health checks on the vault and report issues that need attention.

## Instructions

When this skill is invoked, run the following checks. If an arg is provided, only run that specific check. Otherwise run all checks.

### Check 1: Missing Frontmatter (`frontmatter`)

Scan all .md files (excluding README.md files and CLAUDE.md) for missing or incomplete frontmatter.

**Required fields:** tags, created, difficulty, status

Report notes that are missing any of these fields.

Use Grep to check for frontmatter blocks, then Read files that look incomplete.

### Check 2: Orphan Notes (`orphans`)

Find notes that have zero inbound links from other notes.

- Use the Obsidian CLI (`obsidian search` or `obsidian backlinks`) if available
- Fall back to Grep for `[[filename]]` patterns across the vault
- Exclude index/README files from being flagged as orphans
- Exclude CLAUDE.md and .claude/ directory

Report orphaned notes that should probably be linked from somewhere.

### Check 3: Broken Links (`links`)

Find wiki links (`[[target]]`) that point to notes that don't exist.

- Extract all `[[...]]` links from all files
- Check if each target file exists
- Report broken links with the source file and line number

### Check 4: Stale Drafts (`stale`)

Find notes with `status: draft` or `status: needs-review` that may need attention.

Report them sorted by created date (oldest first).

### Check 5: Missing Intros (`intros`)

Check that notes have a clear introductory paragraph after the H1 heading (per vault conventions, every note needs an accessible intro).

- Read the first ~10 lines after the H1 heading
- Flag notes where the intro is missing or is just a link/list without explanation

### Check 6: Large Files (`size`)

Find notes exceeding ~500 lines that might benefit from splitting into sub-topics.

Use `wc -l` or Read with line counting.

## Output Format

Present results as a summary table per check:

```markdown
## Vault Audit Results

### Missing Frontmatter (X issues)
| File | Missing Fields |
|---|---|
| file.md | tags, difficulty |

### Orphan Notes (X found)
- orphan-note.md — consider linking from [[parent-topic]]

### Broken Links (X found)
| Source | Broken Link |
|---|---|
| mcp.md:45 | [[nonexistent-note]] |

### Stale Drafts (X found)
| File | Status | Created |
|---|---|---|
| draft.md | draft | 2026-01-15 |

### Summary
- Total notes: X
- Issues found: X
- Health score: X/100
```

Calculate a simple health score: start at 100, subtract 2 per missing frontmatter field, 3 per orphan, 5 per broken link, 1 per stale draft.

## Usage

```
/audit              # run all checks
/audit frontmatter  # only check frontmatter
/audit orphans      # only find orphan notes
/audit links        # only check for broken links
/audit stale        # only find stale drafts
```
