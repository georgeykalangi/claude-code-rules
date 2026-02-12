---
description: Database design, migrations, and query patterns
globs:
  - "**/*.sql"
  - "**/models/**/*"
  - "**/migrations/**/*"
  - "**/schema/**/*"
  - "**/db/**/*"
---

# Database Standards

## Schema Design
- Every table needs a primary key. Prefer UUIDs or auto-increment integers.
- Use created_at and updated_at timestamps on all tables.
- Use soft deletes (deleted_at) for data that might need recovery.
- Normalize to 3NF by default. Denormalize only with measured performance justification.
- Use meaningful table and column names. No abbreviations (use `user_id` not `uid`).

## Migrations
- Migrations are forward-only in production. Never edit a deployed migration.
- Each migration does one thing: add table, add column, add index, etc.
- Always write reversible migrations (up + down) when the framework supports it.
- Test migrations against a copy of production-like data before deploying.
- Name migrations descriptively: `20240115_add_email_index_to_users`

## Indexing
- Index all foreign keys.
- Index columns frequently used in WHERE, JOIN, ORDER BY.
- Use composite indexes for queries that filter on multiple columns (order matters).
- Don't over-index: each index slows writes. Only index what queries actually need.
- Review slow query logs periodically and add indexes as needed.

## Queries
- Use parameterized queries. Never concatenate user input into SQL.
- Select only the columns you need. Avoid `SELECT *` in application code.
- Use LIMIT on all list queries. Default to reasonable page sizes (20-100).
- Avoid queries inside loops — batch or join instead.
- Use transactions for operations that must be atomic.

## Transactions
- Keep transactions short. Don't hold locks longer than necessary.
- Use the appropriate isolation level for your use case (READ COMMITTED default).
- Handle deadlocks with retry logic.
- Never make external API calls inside a transaction.

## Data Integrity
- Use foreign key constraints to enforce relationships.
- Use CHECK constraints for value validation at the DB level.
- Use UNIQUE constraints where business logic requires uniqueness.
- Use ENUMs or lookup tables for fixed sets of values.
