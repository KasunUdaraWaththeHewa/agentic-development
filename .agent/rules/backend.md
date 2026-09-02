# Backend Development Rules

## Purpose

These rules apply to backend services, APIs, workers, queues, scheduled jobs, and server-side business logic.

---

# 1. Layer Responsibilities

- Controllers/handlers adapt transport concerns; they must not become business-logic containers.
- Business/application services coordinate use cases.
- Domain logic should live close to the domain concept where appropriate.
- Repositories/persistence adapters handle persistence concerns.
- External providers should be isolated behind meaningful adapters/gateways when useful.
- DTOs represent transport/data contracts; they are not business orchestration services.

---

# 2. Controllers

Controllers should primarily:

- parse/receive requests
- invoke validation mechanisms
- call application/business services
- map results to transport responses

Avoid in controllers:

- complex business rules
- direct SQL
- long authorization logic duplicated across endpoints
- third-party SDK orchestration
- report generation logic
- large transformations

---

# 3. Services

- Keep services focused on coherent use cases/responsibilities.
- Avoid god services.
- Use descriptive operation names.
- Keep dependencies minimal.
- Do not inject dependencies "just in case".
- Avoid service-to-service cycles.
- Separate orchestration from low-level infrastructure details.

---

# 4. Data Access

- Keep queries close to repositories/data-access modules.
- Avoid N+1 query patterns.
- Use transactions where operations must succeed/fail atomically.
- Keep transaction boundaries explicit.
- Do not hold transactions open across slow external network calls unless deliberately required.
- Return domain/application representations rather than leaking ORM internals broadly.
- Avoid broad `SELECT *` where explicit columns improve stability/security/performance.

---

# 5. API Design

- Use consistent resource naming.
- Use consistent status/error shapes.
- Validate request DTOs.
- Define pagination for unbounded collections.
- Define filtering/sorting explicitly.
- Avoid exposing internal database identifiers/fields without need.
- Preserve backward compatibility for public/consumer-facing APIs where required.
- Version APIs only when a real compatibility boundary requires it.

---

# 6. Error Handling

- Use consistent application/domain exceptions.
- Map exceptions to transport responses at a boundary.
- Do not duplicate HTTP exception mapping in every service.
- Preserve internal context for diagnostics.
- Never expose stack traces or sensitive internals to clients.

---

# 7. Transactions and Idempotency

- Use transactions for multi-step persistence invariants.
- For retryable commands, design idempotency where duplicate execution can cause harm.
- Webhook/event handlers should assume duplicates can occur.
- Queue consumers should define retry/dead-letter behavior.
- Avoid non-idempotent side effects before a step that may be retried unless deliberately handled.

---

# 8. Background Jobs and Queues

- Define retry limits.
- Define backoff behavior.
- Define dead-letter/failure handling.
- Make handlers idempotent where practical.
- Handle partial failures explicitly.
- Include correlation IDs/job IDs in useful logs.
- Define graceful shutdown behavior.
- Do not acknowledge messages before required durable work is complete.

---

# 9. Configuration

- Keep environment-specific configuration outside source logic.
- Validate required configuration at startup.
- Fail startup clearly when mandatory configuration is missing.
- Do not scatter environment-variable access across the codebase; centralize configuration access where practical.

---

# 10. Observability

- Log meaningful operations, failures, and identifiers.
- Avoid noisy logs in hot paths.
- Use structured logs where supported.
- Propagate correlation/request IDs where useful.
- Expose metrics for important operational behavior when required.
- Do not use logs as a substitute for proper error handling.

---

# 11. Performance

- Optimize only measured or obvious high-risk paths.
- Avoid premature micro-optimization.
- Watch for:
  - N+1 queries
  - unbounded queries
  - unbounded memory accumulation
  - large synchronous CPU work on request threads/event loops
  - repeated external calls
  - excessive serialization
- Use streaming for large payloads/files where appropriate.

---

# 12. Backend Code Review Checklist

- [ ] Controller is thin.
- [ ] Business logic is in the correct layer.
- [ ] Data access is isolated.
- [ ] Transactions are correct.
- [ ] Error handling is consistent.
- [ ] Authorization is applied.
- [ ] API contract is validated.
- [ ] Unbounded collections are paginated/limited.
- [ ] Queue/job behavior handles retries/duplicates.
- [ ] External dependencies are isolated where useful.
- [ ] Configuration is centralized/validated.
- [ ] Operational logging is sufficient and non-sensitive.
