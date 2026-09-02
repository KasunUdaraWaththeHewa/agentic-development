# Project-Specific Development Rules

## Purpose

These rules define how development must be done for this codebase.

They are not generic suggestions. They describe project-level conventions that must be followed when adding, changing, reviewing, or refactoring code in this repository.

Generic Clean Code and product-development rules still apply. These project-specific rules adapt those baselines to the actual codebase.

When these rules conflict with a generic preference, follow these rules unless doing so would weaken correctness, security, data integrity, legal/compliance requirements, or required product behavior.

---

# 1. Inspect the Existing Codebase First

Before implementing a change:

- inspect the existing folder structure
- inspect related modules and callers
- inspect existing naming conventions
- inspect existing DTOs, entities, services, components, hooks, utilities, and tests
- inspect how similar features are already implemented
- reuse established project patterns when they are working cleanly

Do not introduce a new project style just because it is personally preferred.

Do not create a new abstraction if an existing project convention already solves the problem clearly.

---

# 2. Always Use i18n for User-Facing Text

Do not hardcode user-facing text directly in UI components, pages, validation messages, notifications, emails, dialogs, buttons, labels, placeholders, empty states, errors, success messages, or navigation.

Use the project's i18n mechanism for all user-visible text.

Hardcoded text is allowed only when:

- the text is not user-facing
- the value is a technical identifier
- the value is test-only fixture data
- the project already has an explicit exception for that exact use case

When adding UI text:

- add translation keys in the appropriate locale/resource files
- use clear, stable key names based on product meaning
- keep key naming consistent with nearby translation keys
- avoid duplicating the same text under unrelated keys
- avoid building sentences through fragile string concatenation
- use interpolation/pluralization support when values are dynamic
- keep fallback behavior consistent with the project's i18n setup

When reviewing code, treat hardcoded user-facing text as a project-specific issue even if the code otherwise works.

---

# 3. Backend Must Follow the Monorepo Concept

Backend development must respect the repository's monorepo structure.

Backend development should use NestJS concepts and conventions where backend application code is being created or changed.

This project may use a customized NestJS monorepo template. Before changing backend structure, agents must understand the template-specific features and use them correctly.

When adding backend code:

- place code in the correct backend app, package, module, or library
- use NestJS modules, controllers, providers, services, guards, pipes, interceptors, filters, and decorators where they fit the existing architecture
- inspect customized NestJS monorepo template conventions before adding modules, packages, providers, decorators, configuration, guards, interceptors, filters, jobs, or shared libraries
- reuse template-provided helpers, base classes, decorators, response patterns, exception patterns, repository patterns, configuration patterns, and module wiring when they are the established project convention
- keep shared backend logic in shared packages only when it is truly shared
- avoid duplicating backend business logic across apps/packages
- avoid importing across boundaries in ways that break the monorepo architecture
- keep dependency direction clear
- keep package boundaries meaningful
- expose reusable behavior through explicit public APIs or module exports
- avoid circular dependencies between packages/modules
- keep backend configuration scoped to the correct app/package
- keep tests close to the backend app/package they verify

Do not place backend code in frontend packages.

Do not create generic shared packages just to avoid small duplication.

Do not move code into a shared package until there is a real shared responsibility or repeated backend use case.

Prefer simple local backend implementation first, then promote to shared monorepo packages only when the need is real.

Do not bypass NestJS structure with ad hoc routing, global mutable state, or framework-free service wiring unless there is a clear project-approved reason.

Do not ignore template-specific features and rebuild equivalent behavior manually.

Do not force generic NestJS examples into this repository when the customized monorepo template already provides a cleaner local pattern.

---

# 4. Use TypeORM Where Necessary

Use TypeORM for database persistence where the project uses TypeORM or where persistence work requires ORM-managed entities, repositories, relations, transactions, migrations, or query builders.

The primary database stack is SQL with PostgreSQL. Design persistence with PostgreSQL behavior, SQL constraints, transactions, indexes, and migrations in mind.

Use TypeORM when:

- defining or modifying database entities
- implementing repository-style persistence
- working with relations that TypeORM owns
- creating database migrations
- handling transactions through the project's established TypeORM patterns
- writing queries that benefit from TypeORM query builder or repository APIs
- preserving consistency with existing persistence modules

Do not bypass TypeORM with raw SQL unless there is a clear reason.

Raw SQL may be acceptable when:

- TypeORM cannot express the query cleanly
- performance requires a carefully written query
- a database-specific feature is needed
- a migration requires direct SQL
- existing project conventions already use raw SQL for that pattern

When using raw SQL:

- parameterize inputs
- avoid SQL injection risks
- write PostgreSQL-compatible SQL unless a documented project exception exists
- keep query ownership clear
- add tests for important behavior
- explain non-obvious database-specific logic

Do not mix TypeORM and another persistence style in the same feature without a clear boundary and reason.

Follow existing entity, migration, repository, transaction, and naming conventions.

Use database constraints, indexes, and migrations deliberately. Do not rely only on application code for data integrity that PostgreSQL can enforce safely.

Prefer TypeORM `synchronize: false`.

Do not rely on automatic schema synchronization for normal development, shared environments, staging, or production.

Schema changes should be made through migrations.

When schema changes are required:

- create a TypeORM migration
- keep the migration consistent with entity changes
- make migrations reversible where practical
- review destructive changes carefully
- avoid dropping or rewriting data without an explicit rollout and backup plan
- test migration behavior when the change is risky

Do not manually change the database schema without adding the corresponding migration.

When implementing query/filter behavior, prefer the project's `.query` conventions where they exist.

Use the established `filterAnd` and `filterOr` behavior for query filtering when the feature needs AND/OR filter composition.

Do not invent a separate filtering syntax, parser, DTO shape, or query abstraction unless the existing `.query`, `filterAnd`, and `filterOr` behavior cannot satisfy the product requirement cleanly.

---

# 5. API Contracts and NestJS Layer Responsibilities

Keep API contracts explicit, typed, and consistent.

Use DTOs for request and response shapes where the project pattern expects DTOs.

Keep validation and transformation at the API boundary using the project's NestJS patterns.

Prefer validation decorators, pipes, or existing template validation helpers instead of hand-written validation scattered inside services.

Do not return database entities directly from controllers when response DTOs or serializers are expected.

Keep API response format consistent with existing controllers.

NestJS responsibilities:

- controllers should stay thin and handle routing, request mapping, response mapping, and delegation
- services should hold application flow, product orchestration, and business use cases
- repositories/query services should own persistence queries
- DTOs should describe API boundary contracts
- entities should describe persistence shape and database relationships
- guards should handle authentication and authorization checks
- pipes should handle validation and transformation
- interceptors should handle cross-cutting response/request behavior
- exception filters should handle consistent error translation

Do not place business orchestration in controllers.

Do not place persistence query details throughout services when a repository/query service pattern exists.

Do not bypass existing response, exception, validation, or serialization conventions.

---

# 6. Error Handling and Logging

Follow the project's existing exception, response, and logging patterns.

Do not leak low-level database, provider, or infrastructure errors directly to users.

Translate low-level errors at the correct boundary.

User-facing errors must be ready for i18n where applicable.

Logs should include enough context to diagnose failures without exposing sensitive data.

Do not swallow exceptions silently.

Catch errors only when you can recover, translate, add useful context, clean up, or report them at the correct boundary.

---

# 7. Environment and Secrets

Do not hardcode secrets, credentials, tokens, private keys, database URLs, service URLs, or environment-specific values in code.

Use environment variables or the project's configuration service for runtime settings.

When a value clearly should not be hardcoded, make it configurable.

Prefer environment-file-based configuration where the value changes by environment, deployment, developer machine, customer, tenant, feature rollout, or operational need.

Values that should usually be configurable include:

- service URLs
- ports
- database settings
- credentials and tokens
- feature toggles
- rate limits
- timeouts
- retry counts
- page sizes and limits
- file size limits
- storage bucket/container names
- email/SMS sender settings
- external provider identifiers
- logging levels
- cache TTLs
- queue names
- cron schedules
- CORS origins
- public frontend environment values

Hardcode a value only when it is a stable product constant, language/framework constant, harmless UI constant, or domain value that should not vary by environment.

When making a value configurable:

- use the existing env/config loading pattern
- add the key to the appropriate `.env.example` or config example file
- validate required configuration at startup where possible
- provide safe defaults only when defaults are truly safe
- avoid scattering direct environment reads throughout the codebase
- keep configuration names clear, searchable, and consistent
- document non-obvious configuration behavior

When adding a new environment variable:

- add it to the appropriate example environment file
- document its purpose if not obvious
- validate it at startup where the project has validation support
- provide a safe default only when a default is genuinely safe

Do not hide required environment configuration inside code paths that fail only at runtime.

---

# 8. Docker and Runtime Environment

Use Docker concepts consistently for local development, service orchestration, and runtime dependencies where the project provides Docker support.

When changing backend, database, or infrastructure behavior:

- keep Dockerfiles and compose files aligned with the application structure
- keep PostgreSQL service configuration consistent with project expectations
- avoid hardcoding environment-specific values in code
- use environment variables or the project's configuration system for runtime settings
- document new required services, ports, volumes, or environment variables
- keep containers reproducible and easy to start

Do not add Docker complexity for purely local code changes that do not affect runtime services.

---

# 9. Product and Business Logic Placement

Place product/business logic where the existing codebase expects it.

Use UI code for:

- presentation
- layout
- labels through i18n
- user interaction
- visibility of non-authoritative UI elements
- UI-based configuration for low-risk product experience behavior

Use backend/domain code for:

- authoritative business rules
- validation that must not be bypassed
- authentication and authorization
- permissions
- money, pricing, billing, or financial behavior
- privacy and compliance behavior
- data integrity
- irreversible state transitions

Do not rely on UI-only checks for behavior that must be enforced securely or consistently.

Do not duplicate authoritative business rules between UI and backend unless one side is clearly derived or used only for user experience.

---

# 10. UI-Based Configuration

UI-based configuration is allowed when it speeds delivery and the behavior is mainly product experience, not authoritative enforcement.

Good candidates:

- labels through i18n keys
- layout
- ordering
- visibility
- filters
- table columns
- dashboard widgets
- form field options
- copy variants
- low-risk experiment variants

UI-based configuration must:

- be easy to read
- use product-oriented names
- have sensible defaults
- be safe when missing or invalid
- be easy to test
- avoid hardcoded user-facing text
- avoid hiding critical business rules

Do not turn UI configuration files into dumping grounds for unrelated product behavior.

---

# 11. Frontend Development Rules

Frontend code must be easy to read, easy to change, responsive, and consistent with the project's React patterns.

Use React concepts deliberately:

- keep pages focused on composition, routing-level behavior, and screen orchestration
- move reusable UI into components
- move reusable stateful React logic into hooks
- move pure reusable logic into helpers
- keep side effects explicit and close to the hook or page behavior that owns them
- keep component props clear and typed
- avoid deeply nested JSX that makes the screen hard to scan

Use Ant Design components where they fit the product need and existing UI style.

Use Bootstrap Icons for icons where icons are needed.

Use Tailwind concepts and utilities where the project uses Tailwind for styling, layout, spacing, responsiveness, and small visual adjustments.

Prefer project components over placing many raw Ant Design components directly inside pages.

Pages may use Ant Design components directly for small, simple screens, but when a page becomes hard to read, extract meaningful components.

Do not wrap every Ant Design component unnecessarily. Create a project component when it improves:

- readability
- reuse
- product naming
- consistency
- testability
- separation of page orchestration from UI details

Keep frontend files separated by responsibility where it makes the codebase easier to manage:

- pages for route-level screens
- components for reusable or meaningful UI sections
- hooks for React hook logic
- helpers for pure utility functions
- constants/config for stable frontend configuration
- types for shared frontend types when useful
- i18n resources for user-facing text

Do not split files unnecessarily when one small readable file is clearer.

Avoid unlimited page length. If a page grows too long to scan comfortably, extract named components, hooks, helpers, or configuration based on responsibility.

Keep functions small enough to remain maintainable.

Use readable names that explain product intent and UI behavior.

Do not introduce variables that are used only once unless the variable improves readability, names a meaningful concept, avoids repeated work, or clarifies a complex expression.

Maintain responsive behavior across supported screen sizes.

Use layout primitives, responsive Ant Design patterns, Tailwind utilities, CSS, or existing project utilities consistently.

Balance Ant Design and Tailwind responsibilities:

- use Ant Design for structured UI components, forms, tables, modals, menus, feedback, and common interaction patterns
- use Tailwind for layout, spacing, responsive behavior, small styling adjustments, and composition around components
- avoid fighting Ant Design styles with excessive Tailwind overrides
- avoid custom CSS when Ant Design props, Tailwind utilities, or existing project styles solve the need cleanly
- keep styling readable and local to the component responsibility

Do not hardcode text while creating UI. All user-facing text must go through i18n.

Maintain consistent casing in i18n labels and keys.

Follow the existing project casing convention. Prefer snake_case where the project already uses snake_case for translation keys, API fields, config keys, or database-facing names. Use camelCase where the project already uses camelCase for JavaScript/TypeScript variables, functions, props, and React code.

Do not mix naming cases arbitrarily inside the same file, feature, or i18n namespace.

When rules overlap, choose the balanced solution that best preserves correctness, readability, maintainability, project consistency, and delivery speed.

---

# 12. Frontend API Integration

Keep frontend API calls in service/client modules, hooks, or existing data-access abstractions.

Do not scatter raw API calls directly across pages when the project has an API client or service pattern.

Use typed request and response models where practical.

Keep loading, error, empty, and success states consistent with existing project UI patterns.

Do not duplicate backend validation rules in the UI except as user-experience hints.

UI validation should help users, but backend validation remains authoritative.

Keep API error handling readable and i18n-ready for user-facing messages.

---

# 13. Forms

Use Ant Design Form patterns consistently where forms are needed.

Form labels, helper text, placeholders, validation messages, errors, and button text must use i18n.

Keep large forms split into meaningful sections or components when doing so improves readability.

Do not place complex business logic inside form JSX.

Move reusable form behavior into hooks, helpers, validators, or configuration where appropriate.

Keep form field names, DTO fields, and i18n keys consistent with project casing conventions.

---

# 14. Tables and Lists

Use shared table/list patterns where the project has them.

Table titles, column labels, filters, empty states, actions, and status labels must use i18n.

Use server-side pagination, filtering, and sorting when data can grow meaningfully.

Use the project's `.query`, `filterAnd`, and `filterOr` conventions for server-side table/list filtering where applicable.

Keep column definitions and table configuration readable.

Extract table columns, filters, actions, or row mappers when a page becomes hard to scan.

Do not create a new table abstraction unless existing Ant Design/project table patterns cannot satisfy the need cleanly.

---

# 15. State Management

Use local React state for local UI-only state.

Use hooks for reusable stateful logic.

Use the project's existing global state, query, cache, or data-fetching pattern when shared or server state requires it.

Do not introduce a new state management library without a strong project reason.

Keep derived state minimal and explicit.

Avoid duplicating server state in local state unless the UI needs an editable draft or temporary interaction state.

---

# 16. Generated and Scaffolded Code

Do not manually edit generated files unless the project explicitly expects manual edits for that generated output.

Remove unused scaffold boilerplate before finishing.

If generated or scaffolded code becomes production code, refactor it to match project naming, structure, i18n, testing, error handling, and Clean Code rules.

Do not leave placeholder names, sample data, dead routes, unused providers, unused imports, or generic generated comments.

---

# 17. Configuration and Feature Flags

Configuration and feature flags must have clear ownership and lifecycle.

When adding configuration or flags:

- define the owner
- define the default value
- define allowed values
- validate values where user/admin input is possible
- define rollback behavior
- define cleanup or removal conditions for temporary flags
- keep names clear and searchable
- avoid hidden flags that future developers cannot discover

Use configuration to speed delivery when the behavior is simple, reversible, and safe to change without code.

Use code when the behavior is complex, security-sensitive, or protects money, permissions, privacy, legal/compliance requirements, or data integrity.

---

# 18. Keep Solutions Simple

Do not over-engineer this codebase.

Prefer a direct, readable implementation when it solves the product need cleanly.

Avoid unnecessary:

- services
- layers
- wrappers
- factories
- workflow engines
- state machines
- shared packages
- generic helpers
- configuration systems
- Docker services
- custom UI systems when Ant Design and Tailwind are enough
- abstractions around stable dependencies

Add architecture only when it solves a real problem in this codebase, such as repeated use, meaningful duplication, volatile dependencies, testing difficulty, ownership boundaries, or product risk.

Avoid unnecessary compatibility fallbacks.

Do not add fallback branches only to preserve previous behavior unless backward compatibility is an explicit product or API requirement.

Unnecessary compatibility fallbacks often make code harder to read, harder to test, and easier to break.

When compatibility is required:

- name the compatibility path clearly
- keep it narrow
- add tests for both current and legacy behavior
- document the removal condition when it is temporary
- avoid spreading legacy behavior across unrelated modules

Simplify logic that is unnecessarily reused within the same block.

If the same expression, lookup, condition, transformation, or calculation is repeated in a block, either:

- compute it once with a meaningful name when reuse improves clarity or performance
- extract a small helper when it represents a reusable concept
- inline it when a variable would add no readability or reuse value

Do not create variables for every expression mechanically.

Do not keep repeated logic merely because it was generated or copied.

Consider time complexity and space complexity pragmatically when implementing algorithms, loops, queries, filtering, transformations, and data processing.

Do not over-focus on complexity for a small block that is not practically overused, does not process large data, and is not on a hot path.

Do not judge complexity only from total system user count. Judge it from practical execution frequency, data volume, request path, query cost, and whether the code is likely to be reused heavily.

Prefer lower time and space complexity when there is a real performance risk, repeated execution, large data, expensive I/O, measured issue, or obvious inefficiency, and when improvement does not damage readability, correctness, or project consistency.

Avoid unnecessary nested loops, repeated database calls inside loops, repeated serialization/deserialization, repeated filtering of the same collection, and loading large datasets when the block is practically hot or data-heavy and pagination, filtering, aggregation, or streaming would solve the need more efficiently.

Do not introduce complex optimization unless there is practical evidence, measured need, or clear risk.

---

# 19. Testing Expectations

Tests should follow the existing project testing style and live in the expected app/package/module location.

Add or update tests when:

- behavior changes
- business rules change
- TypeORM entities, repositories, migrations, or queries change
- database schema changes require migrations
- TypeORM `synchronize` or database configuration changes
- NestJS modules, controllers, services, guards, pipes, or providers change
- customized NestJS monorepo template behavior is used or extended
- `.query`, `filterAnd`, or `filterOr` behavior changes
- PostgreSQL-specific SQL, constraints, indexes, or migrations change
- Docker/runtime configuration changes
- configuration or feature flags affect behavior
- UI-based configuration has important visible states
- frontend components, hooks, helpers, or responsive behavior change in meaningful ways
- frontend API integration, forms, tables, lists, or state management change in meaningful ways
- bugs are fixed
- security, permissions, or data integrity logic changes

Do not skip tests just because the change is product-driven or delivery is urgent.

If full automated coverage is not practical, document the reason and provide focused coverage for the highest-risk behavior.

---

# 20. Review Checklist

Before implementation:

- [ ] Existing codebase structure and similar implementations were inspected.
- [ ] User-facing text will use i18n instead of hardcoded strings.
- [ ] Frontend changes will use React, Tailwind, Ant Design, Bootstrap Icons, components, hooks, and helpers according to project conventions.
- [ ] Pages will remain readable and will be split only when doing so improves maintainability.
- [ ] Naming and i18n casing conventions will remain consistent.
- [ ] Responsive behavior is considered for frontend screens.
- [ ] Backend changes fit the monorepo structure.
- [ ] Backend changes use NestJS concepts and conventions.
- [ ] Customized NestJS monorepo template features were inspected and reused where appropriate.
- [ ] TypeORM is used where persistence work requires it or where the project already uses it.
- [ ] TypeORM `synchronize: false` is preferred and schema changes use migrations.
- [ ] API contracts use DTOs, validation, and consistent response patterns where applicable.
- [ ] NestJS layer responsibilities are respected.
- [ ] Error handling and logging follow project patterns.
- [ ] Secrets and environment-specific values use environment/config mechanisms.
- [ ] Values that should be configurable are not hardcoded and use env/config files where applicable.
- [ ] Existing `.query`, `filterAnd`, and `filterOr` conventions are used for query filtering where applicable.
- [ ] SQL/PostgreSQL behavior, constraints, migrations, and indexes are considered where persistence changes are made.
- [ ] Docker/runtime configuration is considered when services, database, or environment behavior changes.
- [ ] Product/business logic is placed in the correct UI, backend, domain, or persistence layer.
- [ ] UI-based configuration is used only where it is safe and product-appropriate.
- [ ] The solution is not over-engineered.

Before finishing:

- [ ] No hardcoded user-facing text was introduced.
- [ ] Translation keys/resources were added or updated where needed.
- [ ] Frontend pages, components, hooks, helpers, and configuration are separated by responsibility where useful.
- [ ] Ant Design components are not dumped directly into long unreadable pages.
- [ ] Tailwind is used consistently for layout, spacing, responsiveness, and small styling needs where appropriate.
- [ ] Bootstrap Icons are used consistently where icons are needed.
- [ ] Functions and JSX blocks remain maintainable in size.
- [ ] Unnecessary single-use variables were avoided unless they improve readability.
- [ ] Responsive behavior was preserved or added where relevant.
- [ ] Frontend API calls, forms, tables/lists, and state management follow project patterns where applicable.
- [ ] Backend package/module boundaries remain clean.
- [ ] NestJS modules, providers, controllers, services, guards, pipes, and decorators follow existing project patterns.
- [ ] Customized NestJS monorepo template conventions were not bypassed or reimplemented unnecessarily.
- [ ] TypeORM usage follows existing entity, repository, migration, and transaction patterns.
- [ ] Schema changes include TypeORM migrations and do not rely on automatic synchronization.
- [ ] API responses do not leak database entities or low-level errors where DTOs/error translation are expected.
- [ ] New environment variables are documented and validated where applicable.
- [ ] Configurable values use existing env/config patterns instead of scattered literals.
- [ ] Query/filter behavior follows existing `.query`, `filterAnd`, and `filterOr` patterns where applicable.
- [ ] PostgreSQL SQL and migrations are safe, parameterized where needed, and consistent with project conventions.
- [ ] Docker files or compose configuration remain reproducible where changed.
- [ ] Unnecessary backward-compatibility fallbacks were avoided or clearly justified.
- [ ] Repeated logic in the same block was simplified where doing so improves clarity or performance.
- [ ] Time complexity and space complexity were considered pragmatically for hot, reused, data-heavy, or expensive changed logic.
- [ ] Configuration and feature flags have ownership, defaults, validation, rollback, and cleanup where needed.
- [ ] Tests match the risk and project conventions.
- [ ] No project-specific rule was bypassed silently.
