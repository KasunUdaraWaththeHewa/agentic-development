# Testing Development Rules

## Purpose

These rules define how automated tests should be designed, written, reviewed, and maintained.

---

# 1. Test Behavior, Not Implementation Trivia

- Prefer tests that verify observable behavior/contracts.
- Avoid tests that fail merely because internal private structure was refactored.
- Mock boundaries/dependencies when useful, not every internal function.
- Do not overfit tests to method call order unless order is part of behavior.

---

# 2. Test Pyramid / Scope

Use the appropriate level:

## Unit Tests
Use for focused business logic, validation, transformations, algorithms, policies.

## Integration Tests
Use for database repositories, external adapters, framework wiring, serialization, real module integration.

## End-to-End Tests
Use for critical user/business flows across the running system.

Do not make every test E2E.

---

# 3. FIRST

Tests should be:

- Fast
- Independent
- Repeatable
- Self-validating
- Timely

---

# 4. Test Structure

Use a consistent structure such as Arrange / Act / Assert when it improves clarity.

Each test should clearly communicate:

- scenario
- action
- expected outcome

Use descriptive test names.

Example:

```text
should reject approval when the reviewer is not assigned
```

Prefer this over:

```text
testApproval2
```

---

# 5. One Concept Per Test

A test should focus on one behavioral concept.

Multiple assertions are acceptable when they verify one outcome.

Do not create one enormous test covering unrelated behaviors.

---

# 6. Boundary Testing

Explicitly test relevant:

- zero
- empty
- min
- max
- exactly-at-limit
- just-over-limit
- first/last
- null/undefined
- invalid enum/state
- date/time boundaries
- authorization boundaries

---

# 7. Regression Testing

For a bug fix:

1. reproduce the bug with a failing test when practical
2. implement the fix
3. add nearby edge cases if the bug reveals a broader weakness
4. keep the regression test permanently unless the behavior is removed

---

# 8. Determinism

Avoid flaky tests.

Do not depend on:

- real current time without control
- random values without seeding/control
- test execution order
- external internet services for ordinary unit tests
- arbitrary sleep delays

Use fakes/clocks/test containers/local dependencies where appropriate.

---

# 9. Mocks and Stubs

Mock external boundaries or expensive dependencies when useful.

Do not mock the entire application.

Avoid tests that only verify:

```text
method A called method B
```

unless that interaction is the intended contract.

Prefer asserting meaningful outcome/state.

---

# 10. Test Data

- Use builders/factories/fixtures when they improve readability.
- Keep each test's important data visible.
- Avoid huge shared fixtures where changing one value breaks unrelated tests.
- Generate unique values where database constraints require them.
- Do not use real secrets or production data.

---

# 11. Database Tests

- Isolate test data.
- Roll back/clean state reliably.
- Do not depend on tests running in order.
- Test constraints and transaction behavior where important.
- Integration tests should use a database sufficiently similar to production when dialect behavior matters.

---

# 12. API Tests

Verify:

- validation
- authorization
- success response
- error response
- important status codes
- serialization
- pagination/filtering where applicable

Do not test framework defaults exhaustively unless customized.

---

# 13. Coverage

Coverage is a diagnostic tool.

Use it to find important untested code paths.

Do not chase 100% mechanically.

Prioritize:

- business rules
- security rules
- error paths
- state transitions
- boundary behavior

---

# 14. Flaky Tests

Never normalize flaky tests as acceptable.

When a test flakes:

- investigate root cause
- determine whether the product code has a race
- remove timing assumptions
- improve isolation
- fix environment instability

Do not simply add longer sleeps.

---

# 15. Disabled Tests

Skipped/ignored tests must have:

- a clear reason
- preferably a tracking reference if long-lived

Do not disable failing tests merely to merge code.

---

# 16. Test Review Checklist

- [ ] Test name describes behavior.
- [ ] Test focuses on one concept.
- [ ] Important boundaries are covered.
- [ ] Bug fixes include regression coverage where practical.
- [ ] Tests are deterministic.
- [ ] Tests are independent.
- [ ] Mocks are not excessive.
- [ ] Assertions verify meaningful outcomes.
- [ ] Test data is readable.
- [ ] No real secrets/production data are used.
- [ ] Tests run at the appropriate scope.
- [ ] Disabled tests are justified.
- [ ] Coverage gaps were considered.
