---
description: Documentation standards for code and projects
globs:
  - "**/*.md"
  - "**/docs/**/*"
---

# Documentation Standards

## Code Comments
- Write comments to explain *why*, not *what*. The code shows what.
- Don't add comments to obvious code.
- Keep comments up to date when code changes.
- Use TODO/FIXME/HACK with your name and a tracking issue: `# TODO(georgey): refactor after v2 migration (#123)`

## Function/Method Documentation
- Document public APIs with docstrings/JSDoc.
- Include: purpose, parameters, return value, exceptions/errors, and a usage example for complex functions.
- Skip docstrings for trivially obvious functions.

## README Structure (for projects)
- Project name and one-line description
- Quick start (install + run in <5 steps)
- Configuration / environment variables
- Architecture overview (for non-trivial projects)
- Contributing guidelines
- License

## Architecture Decision Records
- For significant decisions, document: context, decision, consequences
- Keep in `docs/adr/` or similar
- Number sequentially: `001-use-postgres.md`

## API Documentation
- Document all public endpoints with request/response examples
- Use OpenAPI/Swagger for REST APIs
- Keep docs co-located with code or auto-generated from schemas
