# Database Development Rules

## Purpose

These rules apply to relational databases, schema design, migrations, SQL, indexes, constraints, and persistence-layer changes.

---

# 1. Schema Design

- Model real domain concepts explicitly.
- Use clear table/column names.
- Prefer consistent naming conventions across the schema.
- Use appropriate data types; do not store structured numeric/date/boolean data as text without reason.
- Use NOT NULL when absence is not valid.
- Use foreign keys when referential integrity should be enforced by the database.
- Use unique constraints for true uniqueness rules.
- Use check constraints for stable invariants where practical.

---

# 2. Keys

- Every entity table should have a stable primary key.
- Do not use mutable business values as primary keys unless deliberately justified.
- Foreign keys should reference appropriate unique/primary keys.
- Define delete/update behavior explicitly (`CASCADE`, `RESTRICT`, etc.) based on domain semantics.

---

# 3. Normalization and Duplication

- Avoid duplicated authoritative data.
- Normalize data where duplication would create inconsistency risk.
- Denormalize only for measured/justified performance or reporting needs.
- When denormalizing, define how consistency is maintained.

---

# 4. Migrations

- Every schema change must be represented by a migration.
- Migrations should be deterministic and repeatable within the migration framework.
- Do not manually edit production schemas without corresponding migration history.
- Consider backward compatibility during rolling deployments.
- Avoid destructive migrations without backup/rollback planning.
- Large table changes must consider locking and execution time.
- Data migrations should be explicit and testable.

---

# 5. SQL

- Use parameterized queries.
- Avoid `SELECT *` in application code when explicit columns are more stable.
- Keep SQL readable and formatted.
- Use meaningful aliases.
- Avoid deeply nested queries when a clearer equivalent exists.
- Use CTEs/subqueries only when they improve clarity or performance.
- Understand query semantics; do not rely on accidental ordering.

---

# 6. Indexes

Create indexes based on actual access patterns.

Review indexes for:

- foreign key lookups
- frequent filters
- joins
- ordering/grouping
- uniqueness

Do not add indexes blindly.

Remember indexes increase:

- storage
- write cost
- maintenance cost

Use composite index column order deliberately.

---

# 7. Performance

- Inspect query plans for slow/important queries.
- Avoid N+1 behavior from ORM usage.
- Paginate/limit large result sets.
- Avoid loading large unused columns.
- Avoid unbounded scans in request paths.
- Batch operations where appropriate.
- Do not optimize purely from intuition when plans/metrics are available.

---

# 8. Transactions

- Use transactions for multi-statement invariants.
- Keep transactions as short as practical.
- Avoid external network calls while holding DB transactions open unless deliberately required.
- Choose isolation behavior deliberately for concurrency-sensitive workflows.
- Handle retryable transaction failures when required.

---

# 9. Concurrency and Integrity

- Prefer database constraints for invariants that must remain true under concurrency.
- Do not rely only on "check then insert/update" logic when races are possible.
- Use unique constraints, locking, atomic updates, or transaction isolation appropriately.
- Understand lost-update risks.

---

# 10. Soft Delete

Use soft delete only when the domain/audit/retention requirement needs it.

If used:

- define default query behavior
- enforce uniqueness semantics carefully
- define restoration behavior
- define permanent deletion/retention policy

Do not introduce soft delete automatically for every table.

---

# 11. JSON/JSONB

Use JSON fields when data is genuinely semi-structured or schema-flexible.

Do not use JSON to avoid relational modeling for strongly structured/queryable relationships.

If JSON fields are queried frequently:

- validate expected shape
- consider appropriate indexes
- understand operator/type behavior

---

# 12. Database Review Checklist

- [ ] Schema models the domain clearly.
- [ ] Data types are appropriate.
- [ ] Nullability is deliberate.
- [ ] PK/FK/unique/check constraints are correct.
- [ ] Migration exists and is safe.
- [ ] SQL is parameterized.
- [ ] Query result sizes are bounded.
- [ ] Indexes match access patterns.
- [ ] N+1 risk was considered.
- [ ] Transactions preserve required invariants.
- [ ] Concurrency races are handled at the right layer.
- [ ] Destructive changes have rollback/backup consideration.
