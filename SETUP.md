# Setup & Usage Guide

## Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed and working
- Git installed

## Installation

### Step 1: Clone the repo

```bash
git clone https://github.com/georgeykalangi/claude-code-rules.git
cd claude-code-rules
```

### Step 2: Create the rules directory (if it doesn't exist)

```bash
mkdir -p ~/.claude/rules
```

### Step 3: Copy files into place

**Copy everything:**

```bash
cp CLAUDE.md ~/.claude/CLAUDE.md
cp rules/*.md ~/.claude/rules/
```

**Or cherry-pick specific rules:**

```bash
# Just the master config
cp CLAUDE.md ~/.claude/CLAUDE.md

# Pick the rules you want
cp rules/security.md ~/.claude/rules/
cp rules/testing.md ~/.claude/rules/
cp rules/code-quality.md ~/.claude/rules/
```

### Step 4: Verify

Start a new Claude Code session and ask:

```
What rules and CLAUDE.md files are loaded in this session?
```

Claude should list your global `CLAUDE.md` and all copied rules.

## Updating

When this repo is updated, pull and re-copy:

```bash
cd claude-code-rules
git pull
cp CLAUDE.md ~/.claude/CLAUDE.md
cp rules/*.md ~/.claude/rules/
```

## Uninstalling

Remove the files:

```bash
rm ~/.claude/CLAUDE.md
rm ~/.claude/rules/*.md
```

## Customizing for Your Workflow

### Edit any rule

All rules are plain markdown files. Open and modify them to match your preferences:

```bash
open ~/.claude/rules/security.md
```

### Add your own rule

1. Create a new file in `~/.claude/rules/`:

```bash
touch ~/.claude/rules/my-custom-rule.md
```

2. Add frontmatter at the top:

```yaml
---
description: What this rule covers
globs:
  - "**/relevant/paths/**"
---
```

3. Write your standards in markdown below the frontmatter.

4. Optionally add a reference in `~/.claude/CLAUDE.md` under "Available Rules".

### Add project-specific rules

For rules that apply only to one project, put them in that project's directory:

```bash
mkdir -p <your-project>/.claude/rules
```

Create rule files there — they extend (not replace) the global rules.

## File Overview

| File | What It Does |
|------|-------------|
| `CLAUDE.md` | Master config loaded in every session — philosophy, standards, rule index |
| `rules/code-quality.md` | Naming, formatting, type safety, async patterns |
| `rules/security.md` | OWASP, CORS, CSP, rate limiting, file uploads |
| `rules/git-conventions.md` | Commits, branching, PRs, semver, changelogs |
| `rules/testing.md` | Test pyramid, coverage, fixtures, CI integration |
| `rules/api-design.md` | REST design, rate limiting, health checks (loads for API files) |
| `rules/documentation.md` | Comments, docstrings, README structure (loads for docs/md files) |
| `rules/performance.md` | Caching, N+1 queries, bundle sizes (loads for source files) |
| `rules/database.md` | Migrations, indexing, transactions (loads for SQL/model files) |
| `rules/dependency-management.md` | When to add deps, auditing, lockfiles |
| `rules/logging-observability.md` | Structured logging, tracing, metrics |
| `rules/accessibility.md` | WCAG, semantic HTML, keyboard nav (loads for UI files) |
| `rules/error-handling.md` | Retry patterns, circuit breakers, graceful degradation |
| `LICENSE` | CC BY 4.0 |
| `README.md` | Full reference guide with architecture and extension docs |

## Troubleshooting

**Rules not loading?**
- Make sure files are in `~/.claude/rules/`, not a subdirectory
- Check frontmatter has valid YAML (opening and closing `---`)
- Start a fresh Claude Code session — rules are loaded at session start

**Path-filtered rules not activating?**
- They only load when you're working on files matching the `globs` pattern
- Check the globs in the rule's frontmatter match your file paths

**CLAUDE.md not picked up?**
- It must be at exactly `~/.claude/CLAUDE.md` (global) or `<project-root>/CLAUDE.md` (project)
- Verify with: `cat ~/.claude/CLAUDE.md`
