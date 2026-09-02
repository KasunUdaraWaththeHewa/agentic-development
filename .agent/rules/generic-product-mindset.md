# Generic Product Development Mindset

## Purpose

Use this mindset whenever building, changing, reviewing, or planning software.

The goal is to reason like a product-minded engineer: understand the product need, identify the architecture that best supports it, and still deliver clean, maintainable code.

Product mindset does not replace Clean Code.

Product mindset guides what should be built and why.

Clean Code governs how it should be built.

Fast product delivery is a legitimate architectural concern.

Product mindset should favor the simplest responsible implementation that can ship quickly, change safely, and remain understandable.

Do not over-engineer the codebase when the solution can be created simply.

A simple solution is preferred when it satisfies the product need, preserves correctness, remains readable, and can be changed safely.

---

# 1. Start From Product Intent

Before choosing an implementation, identify:

- the user or customer problem
- the business capability being created or changed
- the expected user workflow
- the data or state that must be protected
- the system boundaries involved
- the operational risks
- the expected future change areas

Do not begin from a framework, pattern, database table, API shape, or UI component unless the product intent is already clear.

Also identify the delivery constraint:

- how quickly the change must ship
- whether the change is experimental or permanent
- who needs to change the behavior later
- whether a configuration change is enough
- whether code changes are justified now
- what cleanup or hardening may be needed after validation

---

# 2. Identify the Architecture From the Product Shape

Architecture should be derived from the product's behavior and change patterns.

Consider:

- which capabilities belong together
- which parts change for the same reason
- which rules are core business rules
- which dependencies are volatile
- which workflows cross system boundaries
- which data requires consistency, auditing, privacy, or security
- which operations need synchronous behavior
- which operations can be asynchronous
- which areas need explicit extension points

Choose architecture that makes important product behavior easy to understand, test, modify, and operate.

Do not force a generic architecture onto every product.

Do not copy an architecture from another product without validating that the domain, scale, risk, team, and change patterns match.

Do not over-architect early when a smaller configurable change satisfies the product need safely.

Do not introduce extra services, layers, abstractions, patterns, state machines, configuration systems, or workflow engines when a direct implementation solves the current product problem cleanly.

Prefer configuration over code changes when:

- the behavior is simple
- the rule changes often
- non-engineers or operators need to adjust it
- the change does not require complex validation or security-sensitive enforcement
- the configuration can be tested, reviewed, and observed

Prefer code over configuration when:

- the behavior is complex
- the rule is security-sensitive
- the rule protects money, permissions, privacy, legal compliance, or data integrity
- invalid configuration could create severe failure
- clear types, tests, and explicit control flow would make the system safer

Prefer a simple direct solution over both code-heavy architecture and configuration-heavy architecture when the product behavior is stable, narrow, and easy to understand.

---

# 3. Build Around Capabilities, Not Only Technical Layers

Technical layers are useful, but product capabilities should remain visible.

Prefer structures that make it clear where a product behavior lives.

Examples of product capabilities:

- onboarding
- billing
- checkout
- document approval
- reporting
- notifications
- access control
- portfolio tracking
- task scheduling

Avoid scattering a single capability across unrelated generic modules when doing so makes the behavior hard to trace.

Use technical layers such as controllers, services, repositories, components, jobs, and adapters when they clarify responsibilities.

Do not let technical layers hide the product language.

---

# 4. Make Business Rules Explicit

Product rules should be named and located deliberately.

Examples:

- eligibility rules
- pricing rules
- permission rules
- validation rules
- workflow status transitions
- quota and limit rules
- retry and expiration policies

Avoid burying product rules inside:

- UI event handlers
- database queries
- generic helpers
- framework callbacks
- third-party SDK integration code
- duplicated conditionals

If a rule matters to the product, it should be discoverable, testable, and named in the product's language.

Not every product rule must be hard-coded in application logic.

Some product rules may be configuration, feature flags, CMS content, admin settings, database-backed policy records, UI-based configuration, or UI-owned display behavior.

This is acceptable when the rule is:

- easy to understand
- easy to validate
- easy to test
- easy to roll back
- owned by the right team
- observable in production
- not silently duplicating a deeper source of truth

Avoid placing authoritative business law only in UI code when backend, database, or service enforcement is required for correctness, security, money movement, permissions, privacy, or data integrity.

UI code may own presentation rules, workflow guidance, input affordances, and fast-changing product copy or layout behavior.

UI-based configuration may be the right product-minded choice when the company needs fast delivery and the behavior is primarily about:

- layout
- labels
- copy
- visibility
- ordering
- filters
- table columns
- dashboard widgets
- form field options
- non-authoritative workflow steps
- experiment variants

Treat UI-based configuration as product architecture, not as random front-end convenience.

Good UI-based configuration should have:

- clear ownership
- sensible defaults
- validation where users or operators can edit it
- fallback behavior
- test coverage for important states
- readable names that match product language
- a documented source of truth
- rollback or disable behavior

Do not duplicate critical business rules in UI configuration when a backend or domain layer must enforce the same rule.

Backend/domain code should own authoritative enforcement when a user could bypass the UI or when incorrect behavior would damage the product.

---

# 5. Separate Product Decisions From Infrastructure Decisions

Keep product behavior independent from infrastructure where practical.

Examples of infrastructure:

- databases
- message queues
- storage providers
- payment providers
- authentication providers
- email/SMS vendors
- analytics tools
- external APIs

Do not spread infrastructure-specific assumptions through core product behavior unless the dependency is genuinely part of the domain.

Use adapters, gateways, repositories, or integration modules when they reduce coupling, improve testability, or isolate volatility.

Do not create ceremonial wrappers around stable dependencies without a real reason.

---

# 6. Optimize for Product Change

Good architecture should make likely product changes localized.

Ask:

- If the workflow changes, where will the code change?
- If a rule changes, is there one authoritative place to update it?
- If a provider changes, how much product code is affected?
- If a new variant is added, does the model support it clearly?
- If the feature grows, will the current boundaries still make sense?

Avoid designs where small product changes require editing many unrelated files.

Avoid designs where technical convenience today creates scattered business logic tomorrow.

Avoid designs where architectural ambition creates more concepts than the product currently needs.

For speedy delivery, prefer changes that are:

- reversible
- measurable
- easy to configure
- easy to remove
- narrow in blast radius
- explicit about ownership

Configuration should have a clear shape, defaults, validation, and fallback behavior.

Do not create untyped stringly-typed configuration, hidden feature flags, or undocumented admin toggles that future developers must rediscover from production behavior.

---

# 7. Classify Product Risk Before Choosing Architecture

Architecture should scale with product risk.

Classify the change before deciding how much structure, validation, testing, and operational support it needs.

Low-risk changes usually include:

- copy
- labels
- layout
- visual ordering
- table columns
- dashboard widgets
- non-authoritative filters
- UI-only display preferences

Medium-risk changes usually include:

- workflow changes
- user input changes
- business process changes
- notification behavior
- saved user preferences
- moderate configuration changes
- behavior affecting multiple screens or services

High-risk changes include:

- money movement
- pricing enforcement
- permissions
- authentication
- authorization
- privacy
- legal or compliance behavior
- data integrity
- irreversible actions
- audit-sensitive workflows

Use lighter architecture for low-risk product experience changes.

Use stronger domain boundaries, validation, tests, observability, and rollback plans for medium-risk and high-risk changes.

Do not rely on UI-only enforcement for high-risk behavior.

---

# 8. Define the Source of Truth

Every meaningful product rule or configurable behavior should have one clear source of truth.

The source of truth may be:

- UI configuration
- backend/domain policy
- database-backed settings
- environment configuration
- feature flags
- CMS content
- admin-managed settings
- an external provider

Choose the source of truth deliberately based on ownership, risk, change frequency, and enforcement needs.

Avoid duplicating the same rule across UI, backend, database, and jobs without a clear synchronization strategy.

If duplication is unavoidable for performance or user experience, make one source authoritative and treat other copies as derived or cached.

Document how stale derived values are refreshed, invalidated, or corrected.

---

# 9. Govern Configuration and Feature Flags

Configuration and feature flags are product architecture.

They must be designed with the same care as code.

Configuration should define:

- owner
- purpose
- default value
- allowed values
- validation rules
- fallback behavior
- rollback behavior
- audit requirements where needed
- permissions for who can change it
- where the source of truth lives

Feature flags should define:

- owner
- purpose
- default state
- target audience or rollout scope
- expected lifetime
- removal condition
- cleanup task
- monitoring or success signal
- rollback behavior

Avoid permanent unknown flags.

Avoid flags with unclear ownership.

Avoid configuration that can silently create invalid product states.

---

# 10. Separate Experiments, MVPs, Production Features, and Platforms

Different delivery stages justify different architecture.

For experiments:

- optimize for learning
- keep scope narrow
- make rollback easy
- avoid building reusable platforms
- track what must be removed or hardened if the experiment succeeds

For MVPs:

- satisfy the core workflow
- preserve correctness and security
- keep implementation simple
- avoid premature extensibility
- identify known gaps explicitly

For production features:

- harden validation
- add meaningful tests
- handle operational failures
- define ownership
- ensure support and rollback paths exist

For platform capabilities:

- require evidence of repeated use
- define extension points deliberately
- document contracts
- invest in stronger tests and observability
- avoid generalizing from only one product use case

Do not build platform architecture for an experiment unless there is clear product evidence that the abstraction is needed.

Do not leave experiment-quality shortcuts hidden inside a permanent production feature.

---

# 11. Record Important Product and Architecture Decisions

Briefly document meaningful decisions when they affect product behavior, architecture, risk, or future change.

The note can live in an architecture decision record, code comment near a non-obvious decision, issue, pull request, or project documentation.

Record:

- the product problem
- the chosen approach
- why simpler alternatives were not enough
- why heavier alternatives were rejected
- source of truth
- risk level
- rollback approach
- what would trigger revisiting the decision

Do not create heavy documentation for trivial changes.

Use lightweight decision notes to preserve reasoning that future maintainers cannot infer from code alone.

---

# 12. Include Operational Readiness in Product Delivery

Fast delivery is safer when production behavior is visible and reversible.

Consider:

- logging
- metrics
- alerting
- audit events
- admin/support visibility
- migration safety
- graceful degradation
- rollback path
- failure diagnostics
- customer support impact

Low-risk UI-only changes may need minimal operational support.

Medium-risk and high-risk product changes should include enough visibility to diagnose failures and enough control to recover safely.

---

# 13. Use Progressive Architecture

Start with the simplest responsible architecture, then evolve it when real pressure appears.

Signals that architecture may need to evolve:

- duplication becomes real
- change frequency increases
- multiple variants appear
- multiple teams need ownership boundaries
- performance constraints become measured
- security or compliance risk increases
- testing becomes difficult
- deployment or rollback becomes risky
- product concepts become hard to locate

Do not predict every future requirement.

Do not ignore real pressure once it appears.

Progressive architecture means starting simple, watching for evidence, and improving structure before complexity becomes painful.

---

# 14. Preserve Clean Code While Moving Fast

Product development often requires iteration, but speed is not permission to create avoidable mess.

Always preserve:

- intention-revealing names
- focused functions
- cohesive modules
- explicit side effects
- clear error handling
- meaningful tests
- consistent boundaries
- minimal duplication
- readable control flow

If a shortcut is unavoidable:

- keep it narrow
- make the tradeoff explicit
- prevent it from becoming a broad dependency
- add a tracked follow-up when appropriate

Do not hide poor design behind "MVP", "prototype", "temporary", or "we will clean it later" unless the scope and cleanup path are clear.

When delivery speed requires compromise, prefer a small explicit compromise over a large generalized system.

Good fast delivery may look like:

- a small configuration option instead of a new workflow engine
- a feature flag around an experimental path
- a UI-owned presentation variant when backend truth is unchanged
- a UI-based configuration model for layout, visibility, labels, filters, widgets, or other fast-changing product experience details
- a database-backed setting when operators need safe control
- a narrow adapter instead of a broad abstraction
- a documented temporary branch with a removal condition
- a direct, readable implementation instead of unnecessary architecture

Bad fast delivery includes:

- duplicating authoritative rules across UI and backend
- hiding product behavior in generic helpers
- skipping validation around user-editable configuration
- creating flags no one owns
- relying on UI checks for security or data integrity
- making future cleanup impossible to locate
- introducing complex architecture only to make a simple feature look more formal

---

# 15. Product-Minded Implementation Checklist

Before implementing:

- [ ] The product problem is understood.
- [ ] The delivery stage is clear: experiment, MVP, production feature, or platform capability.
- [ ] Product risk is classified as low, medium, or high.
- [ ] The expected user or system workflow is understood.
- [ ] Core business rules are identified.
- [ ] Source of truth is defined for meaningful product rules or configuration.
- [ ] Delivery speed and reversibility are considered.
- [ ] Configuration is considered before custom code where appropriate.
- [ ] Configuration and feature flags have ownership, defaults, validation, rollback, and cleanup rules where needed.
- [ ] A simple direct solution is preferred when it satisfies the product need cleanly.
- [ ] Data ownership and state transitions are clear.
- [ ] Security, privacy, and authorization implications are considered.
- [ ] The existing architecture has been inspected.
- [ ] Existing product language has been reused.
- [ ] The proposed structure localizes likely product changes.
- [ ] Infrastructure dependencies are placed behind sensible boundaries.
- [ ] Operational readiness is appropriate for the product risk.
- [ ] Important product or architecture decisions are documented when the reasoning is not obvious.
- [ ] Clean Code rules remain mandatory.

Before finishing:

- [ ] Required behavior works.
- [ ] Important product rules are tested.
- [ ] Relevant edge cases are covered.
- [ ] Names match product concepts.
- [ ] Responsibilities are in the correct modules.
- [ ] No generic dumping grounds were created.
- [ ] No unnecessary services, layers, abstractions, or configuration systems were introduced.
- [ ] No avoidable duplication of business rules was introduced.
- [ ] Configurable behavior has clear ownership, defaults, validation, and rollback behavior.
- [ ] Feature flags have a removal condition when they are temporary.
- [ ] The source of truth is not duplicated accidentally.
- [ ] The architecture can evolve progressively if real product pressure appears.
- [ ] The implementation can be explained from product intent to architecture to code.

---

# 16. Priority Order

When product, architecture, and code-quality concerns compete, use this order:

1. Correctness
2. Security
3. Data integrity
4. Required product behavior
5. Clear product/domain expression
6. Clean Code maintainability
7. Testability
8. Simplicity
9. Consistency with the existing architecture
10. Performance where measured or product-critical
11. Conciseness

Never sacrifice correctness, security, or data integrity for speed.

Never sacrifice Clean Code merely to claim product velocity.

Never introduce architecture that does not support the product's actual needs.
