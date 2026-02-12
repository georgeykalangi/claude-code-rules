# Global Claude Code Standards

## Philosophy
- Write simple, readable, maintainable code. Avoid over-engineering.
- Prefer editing existing files over creating new ones.
- Understand code before modifying it — always read first.
- Match existing patterns and conventions in each project.
- Minimal changes: only touch what's needed for the task.

## Code Quality
- DRY: Extract shared logic only when used 3+ times.
- Functions should do one thing well. Keep them short (<40 lines preferred).
- Use descriptive names: variables, functions, classes should be self-documenting.
- Avoid magic numbers/strings — use named constants.
- Prefer composition over inheritance.
- No dead code, no commented-out code in commits.

## Error Handling
- Handle errors at system boundaries (user input, external APIs, file I/O).
- Fail fast with clear error messages. No silent swallowing.
- Use language-idiomatic error patterns (try/except, Result types, etc.).

## Performance Awareness
- Watch for N+1 queries — batch or join instead.
- Lazy load heavy resources. Avoid unnecessary computation in hot paths.
- Profile before optimizing. Don't guess at bottlenecks.

## Dependencies
- Prefer stdlib or small focused packages over large frameworks for simple tasks.
- Every dependency is a liability — justify additions by maintenance cost vs build cost.
- Always use lockfiles. Pin versions in production.

## Logging & Observability
- Use structured logging (JSON) with consistent fields: timestamp, level, message, context.
- Log levels: DEBUG for dev, INFO for events, WARN for recoverable issues, ERROR for failures.
- Never log secrets, PII, tokens, or full request/response bodies.

## Workflow
- Always read and understand code before modifying it.
- Run existing tests before and after changes.
- Verify your work actually works before claiming it's done.
- When fixing a bug, write a test that reproduces it first.

## Security
- Never hardcode secrets, tokens, or credentials.
- Validate and sanitize all external input.
- Use parameterized queries for databases.
- Follow OWASP top 10 awareness across all projects.

## Testing
- Write tests for non-trivial logic. Match the project's existing test patterns.
- Test behavior, not implementation details.
- Use descriptive test names that explain the scenario.

## Git Conventions
- Commit messages: imperative mood, concise subject (<72 chars).
- One logical change per commit.
- Never force-push to main/master without explicit approval.
- Never commit secrets, .env files, or large binaries.

## Communication
- Be concise. No filler words or unnecessary preamble.
- Reference file paths with line numbers when discussing code.
- No emojis unless explicitly requested.

## Available Rules
Rules are loaded from `~/.claude/rules/` based on context:
- `code-quality.md` — Naming, formatting, type safety, async patterns
- `security.md` — Security checklist, CORS, CSP, file uploads
- `git-conventions.md` — Commits, branching, PRs, semver, changelogs
- `testing.md` — Test pyramid, coverage, fixtures, CI integration
- `api-design.md` — REST design, rate limiting, health checks (path-filtered)
- `documentation.md` — Code and project documentation standards (path-filtered)
- `performance.md` — Caching, query optimization, bundle sizes (path-filtered)
- `database.md` — Migrations, indexing, transactions, queries (path-filtered)
- `dependency-management.md` — When to add deps, auditing, lockfiles
- `logging-observability.md` — Structured logging, tracing, log levels
- `accessibility.md` — a11y for frontend/UI work (path-filtered)
- `error-handling.md` — Error boundaries, retries, graceful degradation
