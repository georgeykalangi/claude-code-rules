---
description: REST API and endpoint design standards
globs:
  - "**/api/**/*"
  - "**/routes/**/*"
  - "**/endpoints/**/*"
  - "**/controllers/**/*"
---

# API Design Standards

## URL Structure
- Use nouns, not verbs: `/users` not `/getUsers`
- Plural resource names: `/users`, `/orders`, `/products`
- Nested resources for relationships: `/users/{id}/orders`
- Use kebab-case for multi-word paths: `/order-items`
- Max 3 levels of nesting. Beyond that, use query params or top-level resources.

## HTTP Methods
- `GET` — read (idempotent, no side effects)
- `POST` — create
- `PUT` — full replace
- `PATCH` — partial update
- `DELETE` — remove

## Response Standards
- Consistent envelope: `{ "data": ..., "error": ..., "meta": ... }`
- Use appropriate HTTP status codes:
  - 200 OK, 201 Created, 204 No Content
  - 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Unprocessable
  - 500 Internal Server Error
- Include pagination for list endpoints: `page`, `limit`, `total`
- Return meaningful error messages with error codes

## Versioning
- Use URL prefix versioning: `/api/v1/users`
- Or header-based: `Accept: application/vnd.api+json;version=1`

## Validation
- Validate all request bodies against a schema (Pydantic, Zod, JSON Schema, etc.)
- Return 422 with field-level errors for validation failures
- Reject unknown fields rather than silently ignoring them

## Authentication
- Use Bearer tokens in the Authorization header
- Return 401 for missing/invalid auth, 403 for insufficient permissions
- Never pass tokens in URL query parameters

## Rate Limiting
- Apply rate limiting to all public endpoints. Stricter on auth routes.
- Return standard headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`
- Return `429 Too Many Requests` with `Retry-After` header when exceeded.

## Idempotency
- GET, PUT, DELETE are idempotent by definition. Ensure implementations match.
- For POST (create), accept an `Idempotency-Key` header to prevent duplicate creation.
- Store idempotency keys with TTL to handle retries safely.

## Health & Readiness
- `GET /health` — basic liveness check (returns 200 if process is running)
- `GET /ready` — readiness check (returns 200 when dependencies are connected: DB, cache, etc.)
- Keep health checks lightweight. No auth required.

## CORS
- Configure allowed origins explicitly. No wildcards in production with credentials.
- Allow only the HTTP methods and headers your API actually uses.
- Set `Access-Control-Max-Age` to reduce preflight requests.

## Timeouts & Retries (Client Guidance)
- Document expected response times per endpoint.
- Recommend retry with exponential backoff for 5xx and 429 responses.
- Never retry non-idempotent requests (POST without idempotency key) automatically.
