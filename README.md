# Claude Code Customization — Reference Guide

A complete map of all customization files, what they do, and how to extend them.

---

## Architecture Overview

```
~/.claude/                          # Global config (applies to ALL projects)
├── CLAUDE.md                       # Master config — always loaded
├── rules/                          # Domain-specific rules
│   ├── code-quality.md             #   Naming, formatting, type safety, async
│   ├── security.md                 #   Security, CORS, CSP, file uploads
│   ├── git-conventions.md          #   Commits, branching, semver, changelogs
│   ├── testing.md                  #   Test pyramid, coverage, CI integration
│   ├── api-design.md               #   REST, rate limiting, health checks (filtered)
│   ├── documentation.md            #   Code & project doc standards (filtered)
│   ├── performance.md              #   Caching, queries, bundle sizes (filtered)
│   ├── database.md                 #   Migrations, indexing, transactions (filtered)
│   ├── dependency-management.md    #   When to add deps, auditing, lockfiles
│   ├── logging-observability.md    #   Structured logging, tracing, metrics
│   ├── accessibility.md            #   a11y for frontend/UI work (filtered)
│   └── error-handling.md           #   Retries, circuit breakers, degradation
├── settings.json                   # Plugins, MCPs, permissions
└── projects/                       # Per-project memory/overrides
    └── <project-hash>/
        └── memory/
            └── MEMORY.md           # Auto-memory per project

<any-project>/                      # Project-level (overrides/extends global)
├── CLAUDE.md                       # Project-specific standards
└── .claude/
    ├── rules/                      # Project-specific rules
    ├── settings.local.json         # Project-specific settings
    └── skills/                     # Project-specific skills
```

---

## Layer 1: CLAUDE.md (Always Loaded)

### Global (all projects)
- **Path**: `~/.claude/CLAUDE.md`
- **Purpose**: Universal coding standards, philosophy, and index of available rules
- **Loaded**: Automatically on every Claude Code session
- **Best practice**: Keep under 150 lines. High-level standards only.

### Project-level (per project)
- **Path**: `<project-root>/CLAUDE.md`
- **Purpose**: Project-specific conventions (framework, stack, architecture)
- **Loaded**: Automatically when working in that project
- **Best practice**: Reference project's tech stack, folder structure, and key patterns

### Precedence
Project CLAUDE.md **extends** (does not replace) the global one. Both are loaded.

---

## Layer 2: Rules (Context-Aware Standards)

### Global rules
- **Path**: `~/.claude/rules/*.md`
- **Purpose**: Domain-specific standards loaded based on context

### Project rules
- **Path**: `<project-root>/.claude/rules/*.md`
- **Purpose**: Project-specific rules (e.g., design system tokens, DB schema conventions)

### Path-based filtering (frontmatter)
Rules can target specific file patterns using frontmatter:

```yaml
---
description: API design standards
globs:
  - "**/api/**/*"
  - "**/routes/**/*"
---
```

Rules **without** globs are always loaded. Rules **with** globs only load when you're working on matching files.

### Current global rules

| File | Scope | Description |
|------|-------|-------------|
| `code-quality.md` | Always | Naming, formatting, type safety, async, code smells |
| `security.md` | Always | OWASP, CORS, CSP, rate limiting, file uploads, dep scanning |
| `git-conventions.md` | Always | Commits, branching, PRs, semver, changelogs, conflict resolution |
| `testing.md` | Always | Test pyramid, AAA, coverage, fixtures, CI, snapshots |
| `api-design.md` | `**/api/**`, `**/routes/**`, etc. | REST, rate limit headers, idempotency, health checks, CORS |
| `documentation.md` | `**/*.md`, `**/docs/**` | Comments, docstrings, README structure, ADRs |
| `performance.md` | `**/*.{ts,tsx,js,jsx,py,go,rs}` | N+1 queries, caching, lazy loading, bundle sizes, concurrency |
| `database.md` | `**/*.sql`, `**/models/**`, `**/migrations/**` | Schema design, migrations, indexing, transactions, integrity |
| `dependency-management.md` | Always | When to add deps, evaluation criteria, lockfiles, auditing |
| `logging-observability.md` | Always | Structured logging, log levels, tracing, metrics, alerting |
| `accessibility.md` | `**/*.{tsx,jsx,vue,svelte,html}`, `**/components/**` | WCAG, semantic HTML, keyboard nav, ARIA, contrast |
| `error-handling.md` | Always | Error types, retry patterns, circuit breakers, graceful degradation |

---

## Layer 3: Skills (User-Triggered Workflows)

### Location
- **Global**: `~/.claude/skills/<skill-name>/`
- **Project**: `<project-root>/.claude/skills/<skill-name>/`

### Structure of a skill directory
```
<skill-name>/
├── SKILL.md          # Core decision logic and workflow steps
├── examples.md       # Production-ready templates
├── reference.md      # Advanced patterns and edge cases
├── templates.md      # Reusable content structures
└── questions.md      # Information-gathering checklists
```

### How to invoke
Type `/<skill-name>` in Claude Code to trigger the skill.

### When to create a skill
- Repeatable multi-step workflows (e.g., `/deploy`, `/new-component`, `/review-pr`)
- You find yourself explaining the same process repeatedly
- The workflow has decision points that benefit from guided questions

---

## Layer 4: Subagents (Autonomous Specialists)

### Built-in subagents
| Name | Purpose |
|------|---------|
| Explore | Fast codebase exploration, file search, keyword search |
| Plan | Architecture planning, implementation strategy |
| General-purpose | Multi-step research and task execution |

### Custom subagents
- **Path**: `~/.claude/agents/` or `<project>/.claude/agents/`
- **Purpose**: Domain-specific autonomous tasks (security audits, migrations, etc.)
- **Format**: Markdown files defining the agent's role, tools, and behavior

---

## Layer 5: MCPs (External Integrations)

### Configuration
- **Path**: `~/.claude/settings.json` (global) or `<project>/.claude/settings.local.json` (project)

### Example MCP configuration
```json
{
  "mcpServers": {
    "github": {
      "command": "gh",
      "args": ["mcp"]
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://..."
      }
    }
  }
}
```

### Common MCP servers
- **GitHub** — Issues, PRs, repos
- **Postgres/MySQL** — Database queries
- **Puppeteer** — Browser automation
- **Slack** — Messaging
- **Linear** — Project management
- **Filesystem** — Extended file operations

---

## How to Extend

### Adding a new global rule
1. Create `~/.claude/rules/<topic>.md`
2. Add frontmatter with `description` and optional `globs`
3. Update the "Available Rules" section in `~/.claude/CLAUDE.md`

### Adding a project-specific rule
1. Create `<project>/.claude/rules/<topic>.md`
2. Add frontmatter with path filtering
3. Optionally reference it in the project's CLAUDE.md

### Adding a new skill
1. Create `~/.claude/skills/<skill-name>/SKILL.md` (minimum)
2. Add supporting files (examples, templates) as needed
3. Invoke with `/<skill-name>` in Claude Code

### Adding an MCP server
1. Edit `~/.claude/settings.json`
2. Add entry under `mcpServers` with command, args, and env
3. Restart Claude Code to pick up changes

---

## Common Mistakes to Avoid

| Mistake | Fix |
|---------|-----|
| Overloading CLAUDE.md with detail | Keep it <150 lines; put depth in rules |
| No path filtering on rules | Add `globs` frontmatter to domain-specific rules |
| Putting workflows in rules | Use skills for multi-step processes |
| Using skills for autonomous tasks | Use subagents for tasks that don't need user triggers |
| Skipping CLAUDE.md entirely | Always have one — saves repeating basics |

---

## Quick Reference: File Paths

| What | Global Path | Project Path |
|------|-------------|--------------|
| Master config | `~/.claude/CLAUDE.md` | `<project>/CLAUDE.md` |
| Rules | `~/.claude/rules/*.md` | `<project>/.claude/rules/*.md` |
| Skills | `~/.claude/skills/<name>/` | `<project>/.claude/skills/<name>/` |
| Agents | `~/.claude/agents/` | `<project>/.claude/agents/` |
| Settings | `~/.claude/settings.json` | `<project>/.claude/settings.local.json` |
| Memory | `~/.claude/projects/<hash>/memory/MEMORY.md` | — |
| Plugins | `~/.claude/settings.json` → `enabledPlugins` | — |

---

*Generated on 2026-02-11. Edit the source files directly to iterate on these standards.*
