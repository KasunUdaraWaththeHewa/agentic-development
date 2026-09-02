# .agent Development Rules

This package contains reusable coding rules and one dedicated Clean Code review skill.

## Structure

```text
.agent/
+-- rules/
|   +-- clean-code.md
|   +-- generic-product-mindset.md
|   +-- project-specific.md
|   +-- security.md
|   +-- backend.md
|   +-- frontend.md
|   +-- database.md
|   +-- testing.md
+-- skills/
    +-- clean-code-review/
        +-- SKILL.md
```

## Recommended Usage

### Always-on rules

Use files under `.agent/rules/` as persistent development constraints.

- `clean-code.md` - universal Clean Code baseline.
- `generic-product-mindset.md` - product-minded architecture and development baseline.
- `project-specific.md` - product/company/repository-specific rules that adapt the generic baselines.
- `security.md` - secure coding baseline.
- `backend.md` - API/service/worker/backend conventions.
- `frontend.md` - frontend/UI/component conventions.
- `database.md` - schema/query/migration/database conventions.
- `testing.md` - testing strategy and test quality rules.

### Product development mindset

Use `.agent/rules/generic-product-mindset.md` whenever identifying architecture, planning a feature, reviewing a module boundary, or implementing product behavior.

This mindset requires the agent to reason from product intent to architecture to code:

1. identify the user or business capability
2. find the product rules and workflows
3. classify product risk and delivery stage
4. define the source of truth
5. inspect the existing architecture
6. choose the simplest responsible design
7. use configuration or UI-based configuration when it is the fastest safe option
8. govern feature flags and configuration with ownership, validation, rollback, and cleanup
9. include operational readiness appropriate to the product risk
10. document important product or architecture decisions when the reasoning is not obvious
11. evolve architecture progressively from evidence
12. implement with Clean Code practices at all times

Product mindset must not weaken Clean Code, testing, security, correctness, or maintainability expectations.

### Project-specific rules

Use `.agent/rules/project-specific.md` for concrete rules that must be followed when developing in this codebase.

Current project-specific requirements include:

- always use i18n for user-facing text instead of hardcoded strings
- use React and Tailwind concepts cleanly with readable pages, components, hooks, helpers, and responsive screens
- use Ant Design components consistently, but prefer project components over dumping large raw Ant Design trees directly into pages
- use Bootstrap Icons where icons are needed
- keep frontend files separated by responsibility where it improves maintainability
- keep functions and pages maintainable in size
- keep naming and i18n label/key casing consistent
- follow the backend monorepo structure
- use NestJS concepts and conventions for backend development
- understand and reuse customized NestJS monorepo template features correctly
- use TypeORM where persistence work requires it or where existing project patterns expect it
- prefer TypeORM `synchronize: false` and create migrations for schema changes
- keep API contracts explicit with DTOs, validation, and consistent response patterns
- keep NestJS layer responsibilities clear: controllers, services, repositories/query services, DTOs, guards, pipes, interceptors, and filters
- prefer the project's `.query`, `filterAnd`, and `filterOr` conventions for query filtering
- use SQL/PostgreSQL deliberately for constraints, indexes, migrations, transactions, and queries
- keep Docker/runtime configuration aligned with project services and environment needs
- follow project error-handling/logging patterns and avoid leaking low-level errors
- keep secrets and environment-specific values in environment/config mechanisms
- make clearly variable values configurable instead of hardcoded, using env files/config where applicable
- keep frontend API calls in services/hooks/client modules rather than scattered in pages
- use Ant Design Form/table/list patterns consistently with i18n labels and messages
- follow the existing state management/query/cache pattern and avoid new state libraries without strong reason
- refactor generated/scaffolded code before it becomes production code
- avoid unnecessary backward-compatibility fallbacks unless compatibility is an explicit requirement
- simplify repeated logic inside the same block when it improves clarity or performance
- consider time complexity and space complexity pragmatically for hot, reused, data-heavy, or expensive logic, not merely because the system has many users
- keep product/business logic in the correct UI, backend, domain, or persistence layer
- use UI-based configuration only where it is safe and product-appropriate
- avoid over-engineering simple solutions

Project-specific rules should guide implementation and review before generic stylistic preferences, unless they would weaken correctness, security, data integrity, legal/compliance requirements, or required product behavior.

### Skill

Use `.agent/skills/clean-code-review/SKILL.md` for an explicit review/refactoring workflow.

The skill assumes the relevant rule files are also available, especially `clean-code.md`, `generic-product-mindset.md`, and `project-specific.md`.

## Precedence

Repository-specific constraints should override generic preferences where there is a real conflict, except that correctness, security, and data integrity must not be weakened merely for stylistic consistency.

## Suggested Priority

1. Correctness
2. Security
3. Data integrity
4. Required business behavior
5. Product/domain clarity
6. Readability
7. Maintainability
8. Testability
9. Simplicity
10. Consistency
11. Performance where measured/relevant
12. Conciseness
