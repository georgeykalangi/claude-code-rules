---
description: Code quality, formatting, and naming conventions
---

# Code Quality Standards

## Naming
- **Variables/functions**: camelCase (JS/TS), snake_case (Python/Rust/Go)
- **Classes/types**: PascalCase across all languages
- **Constants**: UPPER_SNAKE_CASE
- **Files**: Match the project's existing convention. Default to kebab-case.
- **Booleans**: prefix with is/has/should/can (e.g., `isActive`, `hasPermission`)

## Formatting
- Follow the project's existing formatter (Prettier, Black, rustfmt, gofmt, etc.)
- If no formatter is configured, match the surrounding code style exactly.
- Max line length: follow project config, default 100 chars.
- Consistent indentation: match the project (spaces vs tabs, 2 vs 4).

## Structure
- Imports at the top, grouped: stdlib > third-party > local. Alphabetized within groups.
- One class/component per file unless tightly coupled.
- Keep files under 300 lines. Split if larger.
- Avoid deeply nested code (max 3 levels). Extract early returns or helper functions.

## Type Safety
- Prefer strict/typed modes: `strict: true` (TS), type hints (Python), etc.
- Avoid `any`, `object`, or untyped returns. Be explicit about types.
- Use generics over type assertions. Validate before casting.
- Enable strict null checks. Handle nullable values explicitly.

## Early Returns & Guard Clauses
- Use guard clauses to handle edge cases at the top of functions.
- Prefer early return over deep nesting. Flat is better than nested.
- Example: check for invalid input and return/throw first, then proceed with happy path.

## Async & Concurrency
- Always handle promise rejections and async errors.
- Prefer async/await over raw promises or callbacks.
- Avoid fire-and-forget async calls — unhandled rejections crash processes.
- Use concurrency limits when making parallel requests (Promise.allSettled, asyncio.gather).
- Be aware of race conditions in shared state.

## Code Smells to Avoid
- Long parameter lists (>4 params) — use objects/structs instead.
- Boolean parameters — prefer separate functions or enums.
- Global mutable state.
- Circular dependencies.
- Type assertions/casts without validation.
- Callback hell — flatten with async/await or composition.
- Stringly-typed code — use enums or union types instead of magic strings.
