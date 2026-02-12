---
description: Guidelines for managing project dependencies
---

# Dependency Management

## When to Add a Dependency
- The problem is non-trivial and the package is well-maintained (active repo, recent releases).
- Building it yourself would take significant time and introduce bugs.
- The package solves a security-sensitive problem (crypto, auth, sanitization).
- NOT for simple utilities you can write in <20 lines.

## When to Write It Yourself
- The functionality is trivial (string helpers, simple formatters).
- The package brings a large transitive dependency tree for a small feature.
- The package is unmaintained, has known vulnerabilities, or <100 GitHub stars.
- You only need 5% of a large library's functionality.

## Evaluation Criteria
Before adding a dependency, check:
- [ ] Last commit/release within 6 months
- [ ] Active issue triage and PR merging
- [ ] Reasonable dependency tree (check with `npm ls`, `pipdeptree`, etc.)
- [ ] No known critical vulnerabilities
- [ ] License compatible with your project
- [ ] Bundle size impact (for frontend: use bundlephobia.com or similar)

## Lockfiles
- Always commit lockfiles (`package-lock.json`, `poetry.lock`, `Cargo.lock`, `Gemfile.lock`).
- Use exact versions in lockfiles. Let lockfile pin, let manifest define ranges.
- Regenerate lockfiles on a clean install if they get corrupted.

## Auditing
- Run `npm audit` / `pip-audit` / `cargo audit` regularly (ideally in CI).
- Address critical and high vulnerabilities immediately.
- For unfixable transitive vulnerabilities, document the risk and create a tracking issue.

## Updating
- Update dependencies regularly — stale deps accumulate risk.
- Review changelogs before major version bumps.
- Run full test suite after updating.
- Prefer automated dependency update tools (Dependabot, Renovate) in CI.

## Vendoring
- Vendor dependencies only when offline builds are required or for supply chain security.
- If vendoring, keep vendored copies up to date.
