# .agent Development Rules

This package contains reusable coding rules and one dedicated Clean Code review skill.

## Structure

```text
.agent/
+-- rules/
|   +-- clean-code.md
|   +-- generic-product-mindset.md
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

### Skill

Use `.agent/skills/clean-code-review/SKILL.md` for an explicit review/refactoring workflow.

The skill assumes the relevant rule files are also available, especially `clean-code.md` and `generic-product-mindset.md`.

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
