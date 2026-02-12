---
description: Error handling patterns, retries, and graceful degradation
---

# Error Handling Standards

## General Principles
- Errors are not exceptional — they're expected. Design for them.
- Handle errors at the right level. Don't catch too early or too late.
- Fail fast on programmer errors (assertions, invariant violations).
- Recover gracefully from operational errors (network, disk, external services).

## Error Types
- **Programmer errors**: Bugs in code (null reference, type error). Fix the code, don't catch.
- **Operational errors**: Expected failures (timeout, 404, disk full). Handle gracefully.
- **Validation errors**: Bad input from users/clients. Return clear messages with field-level detail.
- **Infrastructure errors**: Service down, network partition. Retry with backoff or degrade.

## Error Messages
- Be specific: "User email already registered" not "An error occurred".
- Include context: what was attempted, what failed, what to do about it.
- Separate user-facing messages from internal/debug details.
- Never expose stack traces, internal paths, or DB schemas to end users.

## Exception/Error Patterns by Context

### Backend APIs
- Catch at the middleware/handler level. Return structured error responses.
- Log the full error with stack trace internally.
- Return appropriate HTTP status codes with error body.
- Use error classes/types for different categories (ValidationError, NotFoundError, etc.).

### Frontend
- Use error boundaries (React) or equivalent to prevent full-page crashes.
- Show user-friendly error states, not blank screens.
- Provide retry actions where appropriate.
- Report errors to monitoring (Sentry, LogRocket, etc.).

### Background Jobs / Workers
- Log failures with full context (job ID, input data, attempt number).
- Implement dead letter queues for permanently failed jobs.
- Alert on repeated failures of the same job type.

## Retry Patterns
- Retry only on transient errors (5xx, timeouts, connection refused).
- Never retry on 4xx (client errors) — the request is wrong, retrying won't help.
- Use exponential backoff with jitter: `delay = base * 2^attempt + random_jitter`.
- Set a max retry count (3-5 for most cases). After that, fail and alert.
- Make retried operations idempotent.

## Graceful Degradation
- If a non-critical service is down, degrade the feature instead of failing entirely.
- Use circuit breakers for external dependencies: open after N failures, half-open to test recovery.
- Cache last-known-good data for read-heavy features.
- Show meaningful fallback UI when data can't be loaded.

## What NOT to Do
- Don't catch errors just to re-throw them unchanged.
- Don't use exceptions for control flow (e.g., catching to branch logic).
- Don't swallow errors silently (`catch {}` with no logging or handling).
- Don't return null/undefined as an error indicator — use Result types or throw.
- Don't catch overly broad exceptions (`catch Exception` / `catch (error)`) without re-raising unknown ones.
