---
name: clean-code-review
description: Review an existing code change, module, service, or repository area for Clean Code violations, maintainability risks, unnecessary complexity, duplication, weak naming, poor responsibility boundaries, product architecture fit, project-specific rule fit, testing weaknesses, and refactoring opportunities. Use together with .agent/rules/clean-code.md, .agent/rules/generic-product-mindset.md, .agent/rules/project-specific.md, and the repository-specific rules.
---

# Clean Code Review Skill

## Objective

Perform a structured Clean Code review without changing business behavior unless the user explicitly asks for implementation changes.

The review must be grounded in the actual repository code and existing tests.

Do not review from isolated assumptions when relevant callers, types, tests, or neighboring modules are available.

Review with a generic product development mindset: identify the product capability, infer the architecture from the product's workflows and rules, and then evaluate whether the code expresses that architecture cleanly.

Clean Code remains mandatory even when the code is being shaped by product development priorities.

Account for delivery speed: configurable behavior, UI-based configuration, feature flags, admin settings, CMS-driven content, and UI-owned presentation rules may be the right product decision when they are clear, validated, reversible, and not replacing required authoritative enforcement.

Do not over-engineer the codebase when a simple, readable, correct solution satisfies the product need.

Evaluate product risk, source of truth, configuration governance, feature flag lifecycle, delivery stage, operational readiness, and whether progressive architecture is being used instead of premature platform design.

Apply project-specific rules before generic preferences when they define the product, company, repository, team, architecture, configuration, testing, security, or operational expectations.

For this project, explicitly check that formatting follows the project's Prettier configuration, user-facing text uses i18n, frontend code follows the project's React and Tailwind structure, Ant Design and Bootstrap Icons are used consistently, backend code follows the customized NestJS monorepo structure, TypeORM is used where persistence work requires it or existing project patterns expect it, TypeORM `synchronize: false` is preferred, schema changes use migrations, `.query` filtering uses established `filterAnd` and `filterOr` behavior, SQL/PostgreSQL usage is deliberate, and Docker/runtime configuration remains consistent.

---

# 1. Preparation

Before reviewing:

1. Read `.agent/rules/clean-code.md`.
2. Read `.agent/rules/generic-product-mindset.md`.
3. Read `.agent/rules/project-specific.md`.
4. Read any relevant repository rule files:
   - `.agent/rules/backend.md`
   - `.agent/rules/frontend.md`
   - `.agent/rules/database.md`
   - `.agent/rules/testing.md`
   - `.agent/rules/security.md`
5. Inspect the target code.
6. Inspect directly related:
   - callers
   - dependencies
   - DTOs/types
   - interfaces
   - tests
   - configuration
   - persistence code
7. Identify the product capability, workflow, business rules, domain responsibility, delivery-speed constraint, delivery stage, product risk, source of truth, and project-specific constraints before judging structure.

---

# 2. Review Categories

Review the code under these categories.

## 2.1 Naming

Check for:

- vague names
- misleading names
- inconsistent terminology
- unnecessary abbreviations
- generic `data`, `info`, `manager`, `helper`, `util`
- magic values
- hidden side effects in method names

## 2.2 Functions

Check for:

- large functions
- multiple responsibilities
- mixed abstraction levels
- deep nesting
- excessive parameters
- boolean/selector flags
- hidden side effects
- duplicated logic
- command/query confusion
- complex conditionals

## 2.3 Classes and Modules

Check for:

- god classes/services
- low cohesion
- misplaced responsibilities
- excessive public API
- artificial coupling
- feature envy
- poor dependency direction
- unnecessary static behavior

## 2.4 Comments

Check for:

- comments compensating for bad code
- redundant comments
- obsolete comments
- commented-out code
- vague TODOs
- excessive documentation noise

## 2.5 Error Handling

Check for:

- swallowed exceptions
- missing context
- inconsistent strategy
- deeply nested error logic
- leaking low-level errors
- ambiguous nullability

## 2.6 Boundaries

Check for:

- third-party SDK spread
- database implementation leaking into business logic
- unnecessary wrappers
- insufficient abstraction around volatile dependencies

## 2.7 Tests

Check for:

- missing important behavior
- missing boundaries
- bug areas without regression tests
- flaky behavior
- excessive mocking
- slow tests
- ignored tests
- implementation-detail coupling

## 2.8 Security

Check relevant security concerns using `.agent/rules/security.md`.

Do not claim a security issue without a plausible exploit/failure path.

## 2.9 Product Architecture Fit

Check whether the code structure follows `.agent/rules/generic-product-mindset.md`.

Look for:

- architecture chosen from product behavior instead of generic patterns
- architecture scaled to product risk
- delivery stage recognized as experiment, MVP, production feature, or platform capability
- product capability boundaries that are easy to find
- business rules named in product/domain language
- one clear source of truth for meaningful product rules and configuration
- workflows and state transitions located deliberately
- infrastructure dependencies isolated where useful
- likely product changes localized to coherent modules
- simple direct solutions preferred over unnecessary architecture
- configurable behavior used appropriately for speed and reversibility
- UI-based configuration used deliberately for fast-changing product experience details
- configuration with clear ownership, defaults, validation, and rollback behavior
- feature flags with owner, default state, rollout scope, removal condition, cleanup path, and monitoring where needed
- UI-owned rules limited to presentation, guidance, copy, layout, or non-authoritative workflow behavior
- authoritative enforcement kept outside the UI when correctness, security, money, permissions, privacy, or data integrity require it
- operational readiness appropriate to product risk
- lightweight decision records for non-obvious product or architecture tradeoffs
- progressive architecture that evolves from evidence rather than speculation
- no generic dumping grounds for product behavior
- Clean Code practices preserved while implementing product-driven decisions

## 2.10 Project-Specific Rule Fit

Check whether the code follows `.agent/rules/project-specific.md`.

Look for:

- existing project architecture conventions followed
- formatting follows the project's Prettier configuration without unrelated formatting churn
- user-facing text routed through i18n instead of hardcoded strings
- frontend pages kept readable through meaningful components, hooks, helpers, and configuration
- Ant Design components used consistently without dumping large raw UI trees directly into pages
- Tailwind used consistently for layout, spacing, responsiveness, and small styling needs
- Bootstrap Icons used consistently where icons are needed
- React hooks kept in hooks when reusable or stateful logic needs separation
- helper functions kept in helpers when pure reusable logic needs separation
- responsive behavior preserved for frontend screens
- naming case and i18n label/key casing kept consistent
- backend changes placed correctly within the monorepo structure
- backend package/module boundaries kept clean
- NestJS modules, controllers, providers, services, guards, pipes, interceptors, filters, and decorators used consistently where applicable
- customized NestJS monorepo template features understood and reused instead of reimplemented manually
- API contracts use DTOs, validation, serialization, and consistent response patterns where applicable
- NestJS layer responsibilities kept clear
- project error-handling and logging patterns followed without leaking low-level errors
- secrets, environment-specific values, and clearly variable settings kept in environment/config mechanisms instead of hardcoded literals
- TypeORM used consistently for entities, repositories, migrations, transactions, and ORM-owned queries where necessary
- TypeORM `synchronize: false` preferred and schema changes implemented through migrations
- `.query`, `filterAnd`, and `filterOr` conventions used for query filtering where applicable
- SQL/PostgreSQL constraints, indexes, transactions, migrations, and raw queries handled safely and consistently
- Docker/runtime configuration kept reproducible and aligned with project services where changed
- frontend API calls kept in services, hooks, clients, or existing data-access abstractions rather than scattered in pages
- Ant Design forms, tables, and lists follow project patterns with i18n labels/messages
- state management follows existing local/global/query/cache patterns without unnecessary new libraries
- generated or scaffolded code refactored before becoming production code
- unnecessary backward-compatibility fallbacks avoided unless explicitly required
- repeated logic inside the same block simplified where clarity or performance improves
- time complexity and space complexity considered pragmatically for hot, reused, data-heavy, or expensive logic, not merely from total system user count
- product behavior placed in the expected module, layer, or capability
- project source-of-truth rules respected
- project UI-based configuration rules respected
- project configuration and feature flag governance followed
- project testing expectations satisfied
- project security, privacy, data, and operational rules considered
- no generic preference overriding an explicit project rule without justification

---

# 3. Severity Classification

Use the following severity levels.

## Critical

A problem likely to cause:

- security compromise
- data corruption
- severe production failure
- incorrect authorization
- irrecoverable behavior

## High

A design/code issue that is likely to create:

- frequent bugs
- major maintenance difficulty
- unsafe change behavior
- major duplication of business rules
- serious testing gaps

## Medium

A meaningful maintainability/readability issue that should be corrected but is not immediately dangerous.

## Low

A small cleanup or consistency improvement.

Do not inflate severity.

---

# 4. Review Output

Return findings in this format:

## Summary

Briefly state:

- overall code quality
- main strengths
- main risks
- whether refactoring is required before merge

## Findings

For each finding provide:

### [Severity] Short title

**Location**
File/function/class.

**Problem**
What is wrong.

**Why it matters**
Concrete maintainability/correctness/security impact.

When relevant, connect the impact to the product workflow, business rule, architecture boundary, or likely future product change.

**Recommended change**
Specific improvement.

**Example**
Provide a small code example only when it materially clarifies the recommendation.

## Positive Observations

Mention patterns worth preserving.

## Refactoring Order

If multiple changes are recommended, order them from safest/highest-value to more invasive.

## Tests to Add/Update

List missing or required tests.

---

# 5. Refactoring Mode

If the user asks to refactor rather than only review:

1. Preserve existing business behavior.
2. Verify existing tests first.
3. Add characterization/regression tests when behavior is unclear.
4. Refactor incrementally.
5. Prefer:
   - rename
   - extract function
   - simplify conditional
   - remove duplication
   - move responsibility
   - split class
   - isolate boundary
6. Keep tests passing after each meaningful step.
7. Avoid unrelated architecture changes.
8. Do not overengineer.

---

# 6. Review Discipline

Do not:

- demand tiny functions mechanically
- demand interfaces without a real boundary
- replace every switch with polymorphism
- reject all null usage blindly
- create abstraction for coincidental duplication
- recommend patterns merely because they are fashionable
- rewrite working code only for style

Always prefer the design that is:

- correct
- secure
- aligned with product intent
- explicit
- readable
- maintainable
- testable
- simple enough for the actual problem

---

# 7. Completion Checklist

Before finishing a Clean Code review, confirm:

- [ ] Existing architecture was inspected.
- [ ] Product capability, workflow, and business rules were identified.
- [ ] Product risk, delivery stage, and source of truth were identified.
- [ ] Project-specific constraints were identified and applied.
- [ ] Formatting follows the project's Prettier configuration.
- [ ] User-facing text uses i18n rather than hardcoded strings.
- [ ] Frontend pages, components, hooks, helpers, React, Tailwind, Ant Design usage, Bootstrap Icons, responsiveness, and casing conventions were reviewed where applicable.
- [ ] Backend changes follow the monorepo structure.
- [ ] Backend changes follow NestJS conventions where applicable.
- [ ] Customized NestJS monorepo template features were understood and applied where applicable.
- [ ] API contracts, NestJS layer responsibilities, error handling, logging, environment/secrets usage, and configurable values were reviewed where applicable.
- [ ] TypeORM usage follows project persistence conventions where applicable.
- [ ] TypeORM `synchronize: false` and migration-based schema changes were reviewed where applicable.
- [ ] `.query`, `filterAnd`, and `filterOr` query behavior was reviewed where applicable.
- [ ] SQL/PostgreSQL usage and Docker/runtime changes were reviewed where applicable.
- [ ] Frontend API integration, forms, tables/lists, state management, and generated/scaffolded code were reviewed where applicable.
- [ ] Unnecessary compatibility fallbacks, repeated logic, and practical time/space complexity risks were reviewed.
- [ ] Delivery-speed constraints and configurable alternatives were considered.
- [ ] Related tests were inspected.
- [ ] Findings are tied to actual code.
- [ ] Severity is justified.
- [ ] Recommendations preserve behavior unless change was requested.
- [ ] Unnecessary abstractions were not proposed.
- [ ] Product architecture recommendations still preserve Clean Code.
- [ ] Recommendations do not over-engineer a simple product need.
- [ ] Configuration and feature flag governance was considered where applicable.
- [ ] Operational readiness was considered according to product risk.
- [ ] Progressive architecture was preferred over premature platform design.
- [ ] Project-specific rules were not bypassed silently.
- [ ] Important security/testing implications were considered.
- [ ] Refactoring recommendations are ordered safely.
