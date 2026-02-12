---
description: Security checklist applied to all code changes
---

# Security Standards

## Secrets Management
- NEVER hardcode API keys, tokens, passwords, or connection strings.
- Use environment variables or secret managers.
- Add `.env`, `credentials.*`, `*.pem`, `*.key` to .gitignore.
- If a secret is accidentally committed, treat it as compromised — rotate immediately.

## Input Validation
- Validate ALL external input: user forms, API requests, URL params, file uploads.
- Sanitize before rendering (prevent XSS).
- Use parameterized queries (prevent SQL injection).
- Whitelist allowed values rather than blacklisting dangerous ones.

## Authentication & Authorization
- Hash passwords with bcrypt/argon2, never MD5/SHA1.
- Use constant-time comparison for tokens.
- Implement proper session management with expiry.
- Check authorization on every protected endpoint, not just the frontend.

## Dependencies
- Prefer well-maintained packages with active communities.
- Pin dependency versions in production.
- Be cautious with transitive dependencies.
- Run vulnerability scans regularly: `npm audit`, `pip-audit`, `cargo audit`, `bundler-audit`.
- Review dependency changelogs before major version bumps.

## CORS
- Whitelist specific origins. Never use `*` in production with credentials.
- Restrict allowed methods and headers to what's actually needed.
- Set appropriate `max-age` for preflight caching.

## Rate Limiting
- Apply rate limiting to all public-facing endpoints.
- Use stricter limits on auth endpoints (login, signup, password reset).
- Return `429 Too Many Requests` with `Retry-After` header.

## Content Security Policy
- Set CSP headers to prevent XSS: restrict script sources, disable inline scripts.
- Use nonces or hashes for necessary inline scripts.
- Set `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`.

## File Upload Security
- Validate file type by content (magic bytes), not just extension.
- Enforce size limits per file and per request.
- Store uploads outside the web root. Never serve uploaded files with their original name.
- Scan for malware when handling user uploads in production.

## Data Protection
- Log only what's necessary. Never log secrets, PII, or full request bodies in production.
- Use HTTPS for all external communication.
- Encrypt sensitive data at rest when applicable.

## Common Vulnerabilities Checklist
- [ ] No SQL/NoSQL injection vectors
- [ ] No XSS (output properly escaped)
- [ ] No CSRF (tokens validated on state-changing requests)
- [ ] No path traversal (file paths validated)
- [ ] No command injection (user input never in shell commands)
- [ ] No insecure deserialization
- [ ] No sensitive data in error messages or logs
