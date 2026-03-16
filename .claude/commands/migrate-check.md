Review database migration files in the current changeset or specified directory.

Check for:
1. **Reversibility** — does every migration have a proper rollback/down migration?
2. **Data safety** — could this migration lose data? (dropping columns, truncating, changing types)
3. **Lock risk** — will this lock large tables? (adding indexes without CONCURRENTLY, ALTER TABLE on big tables)
4. **Compatibility** — can the old code still work while this migration is running? (important for zero-downtime deploys)
5. **Idempotency** — will this fail if run twice? (missing IF NOT EXISTS, IF EXISTS)
6. **Default values** — are NOT NULL columns added with sensible defaults?

For each issue found, explain the risk and suggest the safe alternative.

$ARGUMENTS
