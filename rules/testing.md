---
description: Testing strategies and patterns for all projects
---

# Testing Standards

## General Principles
- Test behavior and outcomes, not implementation details.
- Each test should be independent — no shared mutable state between tests.
- Tests should be deterministic: no flaky tests, no time-dependent assertions.
- Use descriptive test names: `test_user_login_fails_with_invalid_password`

## Test Structure (AAA Pattern)
1. **Arrange** — set up test data and preconditions
2. **Act** — execute the code under test
3. **Assert** — verify the expected outcome

## What to Test
- Happy path: expected inputs produce expected outputs
- Edge cases: empty inputs, boundary values, null/undefined
- Error cases: invalid inputs, network failures, permission denied
- Integration points: API contracts, database queries, external services

## What NOT to Test
- Framework internals or third-party library behavior
- Private methods directly (test through public API)
- Trivial getters/setters with no logic
- Implementation details that may change during refactoring

## Mocking Guidelines
- Mock external dependencies (APIs, databases, file system) not internal logic.
- Keep mocks minimal — over-mocking makes tests brittle and meaningless.
- Prefer fakes/stubs over complex mock frameworks when possible.

## Test Pyramid
- **Unit tests** (majority): Fast, isolated, test single functions/methods.
- **Integration tests** (moderate): Test component interactions, API contracts, DB queries.
- **E2E tests** (few): Test critical user flows end-to-end. Keep these minimal.
- Ratio guideline: ~70% unit, ~20% integration, ~10% e2e.

## Coverage
- Aim for meaningful coverage, not 100%. Focus on critical paths and edge cases.
- New code should include tests for non-trivial logic.
- Coverage drops should be justified in PR reviews.
- Don't write tests just to hit a number — a bad test is worse than no test.

## Fixtures & Factories
- Use factories to create test data with sensible defaults.
- Override only the fields relevant to each test case.
- Share fixtures for common setups but keep them simple.
- Avoid loading production data in tests. Tests should be self-contained.

## Snapshot Testing
- Use for UI components where visual output matters.
- Review snapshot changes carefully — don't blindly update.
- Avoid for data structures that change frequently (timestamps, IDs).
- Keep snapshots small and focused. Large snapshots are hard to review.

## CI Integration
- All tests must pass before merge. No exceptions.
- Tests should run in CI on every push and PR.
- Flaky tests must be fixed or quarantined immediately — they erode trust.
- Keep test suite fast. Parallelize where possible.

## Test Organization
- Mirror the source directory structure in the test directory.
- Name test files consistently: `test_<module>.py`, `<module>.test.ts`, `<module>_test.go`
- Group related tests in describe/context blocks.
