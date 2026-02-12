---
description: Performance optimization patterns and anti-patterns
globs:
  - "**/*.ts"
  - "**/*.tsx"
  - "**/*.js"
  - "**/*.jsx"
  - "**/*.py"
  - "**/*.go"
  - "**/*.rs"
---

# Performance Standards

## General Principles
- Profile before optimizing. Measure, don't guess.
- Optimize the critical path. Ignore cold paths unless they're painful.
- Readability > performance unless proven otherwise by benchmarks.

## Database Queries
- Watch for N+1 queries — batch or use joins/eager loading.
- Add indexes for columns used in WHERE, JOIN, and ORDER BY.
- Use EXPLAIN/ANALYZE to verify query plans on slow queries.
- Paginate large result sets. Never `SELECT *` without LIMIT in production.
- Cache expensive queries with appropriate TTLs.

## Frontend Performance
- Lazy load routes, images, and heavy components.
- Minimize bundle size: tree-shake, code-split, avoid large dependencies.
- Debounce/throttle expensive event handlers (scroll, resize, input).
- Avoid layout thrashing: batch DOM reads and writes.
- Use virtual scrolling for long lists (>100 items).
- Optimize images: compress, use modern formats (WebP/AVIF), serve responsive sizes.

## Backend Performance
- Use connection pooling for databases and HTTP clients.
- Cache at appropriate layers: in-memory (hot data), Redis/Memcached (shared), CDN (static).
- Avoid blocking the event loop (Node.js) or main thread with synchronous I/O.
- Use streaming for large payloads instead of buffering in memory.
- Set appropriate timeouts on all external calls.

## Concurrency
- Use async I/O over threads for I/O-bound work.
- Use worker pools/threads for CPU-bound work.
- Limit concurrency to avoid overwhelming downstream services.
- Be aware of thundering herd — use jitter in retry/cache expiry.

## Anti-Patterns
- Premature optimization without profiling data.
- Caching everything (cache invalidation is hard — only cache what's needed).
- Synchronous calls in async contexts.
- Unbounded in-memory collections that grow with request volume.
- Logging in hot loops.
