---
name: clean-code-review
description: Review an existing code change, module, service, or repository area for Clean Code violations, maintainability risks, unnecessary complexity, duplication, weak naming, poor responsibility boundaries, product architecture fit, testing weaknesses, and refactoring opportunities. Use together with .agent/rules/clean-code.md, .agent/rules/generic-product-mindset.md, and the repository-specific rules.
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

---

# 1. Preparation

Before reviewing:

1. Read `.agent/rules/clean-code.md`.
2. Read `.agent/rules/generic-product-mindset.md`.
3. Read any relevant repository rule files:
   - `.agent/rules/backend.md`
   - `.agent/rules/frontend.md`
   - `.agent/rules/database.md`
   - `.agent/rules/testing.md`
   - `.agent/rules/security.md`
4. Inspect the target code.
5. Inspect directly related:
   - callers
   - dependencies
   - DTOs/types
   - interfaces
   - tests
   - configuration
   - persistence code
6. Identify the product capability, workflow, business rules, domain responsibility, delivery-speed constraint, delivery stage, product risk, and source of truth before judging structure.

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
- [ ] Important security/testing implications were considered.
- [ ] Refactoring recommendations are ordered safely.
