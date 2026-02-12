---
description: Structured logging, tracing, and monitoring standards
---

# Logging & Observability

## Structured Logging
- Use JSON structured logging in production. Key-value pairs, not free-text.
- Standard fields on every log: `timestamp`, `level`, `message`, `service`, `request_id`.
- Add context fields: `user_id`, `endpoint`, `duration_ms`, `status_code`.
- Use a logging library (winston, structlog, slog, zerolog) — not print/console.log.

## Log Levels
- **DEBUG**: Detailed info for local development. Never in production by default.
- **INFO**: Significant events: request handled, job completed, service started.
- **WARN**: Recoverable issues: deprecated usage, retry succeeded, approaching limits.
- **ERROR**: Failures requiring attention: unhandled exception, external service down.
- **FATAL/CRITICAL**: Process cannot continue. Use sparingly.

## What to Log
- Request start and completion (with duration).
- Authentication events (login, logout, failed attempts).
- Business-critical operations (payment processed, order created).
- External service calls (with latency and response status).
- Errors with full stack traces and context.
- Configuration loaded at startup (without secrets).

## What to NEVER Log
- Passwords, tokens, API keys, or secrets.
- PII (emails, SSNs, phone numbers) — or mask them.
- Full request/response bodies in production (too verbose, potential PII).
- Health check pings (noise that drowns out real signals).

## Request Tracing
- Generate a unique request ID at the entry point. Propagate it through all services.
- Include request ID in all log entries for that request.
- Pass trace IDs in headers (`X-Request-ID`, `traceparent`) across service boundaries.

## Metrics & Monitoring
- Track the RED metrics: Rate, Errors, Duration for each endpoint.
- Set up alerts for: error rate spikes, latency p99 degradation, resource exhaustion.
- Use health check endpoints for uptime monitoring.
- Dashboard critical business metrics alongside system metrics.

## Error Reporting
- Aggregate errors by type/fingerprint, not by individual occurrence.
- Include context: which endpoint, what input (sanitized), stack trace.
- Set up alerting thresholds: don't alert on every error, alert on rate changes.
