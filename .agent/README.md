# .agent Development Rules

This package contains reusable coding rules and one dedicated Clean Code review skill.

## Structure

```text
.agent/
├── rules/
│   ├── clean-code.md
│   ├── security.md
│   ├── backend.md
│   ├── frontend.md
│   ├── database.md
│   └── testing.md
└── skills/
    └── clean-code-review/
        └── SKILL.md
```

## Recommended Usage

### Always-on rules

Use files under `.agent/rules/` as persistent development constraints.

- `clean-code.md` — universal Clean Code baseline.
- `security.md` — secure coding baseline.
- `backend.md` — API/service/worker/backend conventions.
- `frontend.md` — frontend/UI/component conventions.
- `database.md` — schema/query/migration/database conventions.
- `testing.md` — testing strategy and test quality rules.

### Skill

Use `.agent/skills/clean-code-review/SKILL.md` for an explicit review/refactoring workflow.

The skill assumes the relevant rule files are also available.

## Precedence

Repository-specific constraints should override generic preferences where there is a real conflict, except that correctness, security, and data integrity must not be weakened merely for stylistic consistency.

## Suggested Priority

1. Correctness
2. Security
3. Data integrity
4. Required business behavior
5. Readability
6. Maintainability
7. Testability
8. Simplicity
9. Consistency
10. Performance where measured/relevant
11. Conciseness
