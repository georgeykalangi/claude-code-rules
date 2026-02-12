---
description: Git workflow, branching, commit, and PR standards
---

# Git Conventions

## Commit Messages
- Format: `<type>: <short description>` (imperative mood)
- Types: feat, fix, refactor, test, docs, chore, perf, ci
- Subject line: max 72 characters, no period at end
- Body (optional): explain *why*, not *what* — the diff shows what changed
- Examples:
  - `feat: add user authentication with JWT`
  - `fix: prevent race condition in queue processing`
  - `refactor: extract payment logic into service layer`

## Branching
- `main`/`master` — production-ready, always deployable
- `feature/<name>` — new features
- `fix/<name>` — bug fixes
- `chore/<name>` — maintenance, deps, CI changes
- Keep branches short-lived. Merge or rebase frequently.

## Pull Requests
- One logical change per PR. Keep PRs small and reviewable (<400 lines preferred).
- Title: concise summary of the change
- Body: Summary bullets, test plan, and link to related issues
- All CI checks must pass before merge.

## What NOT to Commit
- `.env` files or any secrets
- IDE/editor configs (unless shared by the team)
- OS files (`.DS_Store`, `Thumbs.db`)
- Build artifacts, `node_modules`, `__pycache__`, etc.
- Large binary files (use Git LFS if needed)

## Versioning & Releases
- Use semantic versioning: `MAJOR.MINOR.PATCH`
  - MAJOR: breaking changes
  - MINOR: new features, backwards compatible
  - PATCH: bug fixes, backwards compatible
- Tag releases: `git tag v1.2.3`
- Write release notes summarizing changes for each version.

## Changelog
- Maintain a CHANGELOG.md (or auto-generate from conventional commits).
- Group entries: Added, Changed, Fixed, Removed, Security.
- Link each entry to the relevant PR or issue.

## Conflict Resolution
- Prefer rebase for local branches to keep history linear.
- Use merge commits for shared/long-lived branches to preserve context.
- When resolving conflicts: understand both sides before picking. Don't blindly accept.
- After resolving, always run tests to verify the merge didn't break anything.

## Safety
- Never force-push to shared branches without explicit team approval.
- Never rewrite history on main/master.
- Always create new commits rather than amending shared history.
