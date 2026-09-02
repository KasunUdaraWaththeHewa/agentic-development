# Clean Code Development Rules

## Purpose

These rules apply whenever code is created, modified, refactored, reviewed, debugged, or extended.

The objective is not merely to produce code that works.

Code must also be:

- correct
- readable
- understandable
- maintainable
- testable
- changeable
- predictable
- cohesive
- appropriately abstracted
- minimally coupled
- free from unnecessary duplication
- free from unnecessary complexity
- easy for another developer to continue working on

Code is primarily maintained by humans.

Optimize code for the developer who must later:

- understand it
- debug it
- review it
- modify it
- test it
- extend it
- operate it

Working code that is unnecessarily difficult to understand is not considered complete.

---

# 1. Core Engineering Principles

## 1.1 Code Must Clearly Express Intent

Code should make its purpose apparent without forcing readers to reverse-engineer the author's thinking.

Prefer:

- explicit behavior
- descriptive names
- clear control flow
- well-defined responsibilities
- meaningful abstractions

Avoid:

- clever shortcuts
- cryptic expressions
- hidden behavior
- vague names
- surprising side effects
- unnecessary indirection

A developer should be able to understand the important behavior by reading the code itself.

## 1.2 Optimize for Readability

Code is read significantly more often than it is written.

When choosing between:

- a shorter implementation that is difficult to understand
- a slightly longer implementation that clearly expresses its intent

prefer the clearer implementation.

Do not optimize for minimum line count.

Do not write compressed code merely because the language allows it.

## 1.3 Keep the Codebase Continuously Clean

Do not treat code cleanup as something that happens only during dedicated refactoring projects.

Whenever modifying code, improve nearby problems when practical.

Examples:

- rename an unclear variable
- rename a misleading function
- remove dead code
- remove obsolete comments
- simplify a confusing conditional
- remove small duplication
- extract a meaningful function
- remove an unnecessary dependency
- improve an inaccurate test

Follow the Boy Scout principle:

> Leave the code cleaner than you found it.

Do not use this principle to justify massive unrelated refactoring during a small task.

Keep improvements relevant and controlled.

## 1.4 Do Not Deliberately Create a Known Mess

Do not intentionally write poor code with the assumption:

> "We will clean this later."

Temporary shortcuts tend to become permanent.

If a temporary compromise is genuinely unavoidable:

- keep it narrowly scoped
- make its purpose explicit
- document the reason when it cannot be expressed in code
- create a tracked follow-up when appropriate
- prevent the temporary implementation from becoming a dependency throughout the system

## 1.5 Prefer Simplicity

Prefer the simplest design that:

- correctly satisfies requirements
- remains understandable
- remains testable
- can reasonably accommodate expected changes

Do not introduce complexity for hypothetical future requirements without evidence that the abstraction is needed.

## 1.6 Do Not Confuse Abstraction With Quality

More abstractions do not automatically mean cleaner code.

Avoid unnecessary:

- interfaces
- abstract classes
- factories
- repositories
- wrappers
- generic helpers
- inheritance hierarchies
- design patterns
- architectural layers

Every abstraction introduces cognitive cost.

An abstraction must solve a real problem such as:

- duplication
- coupling
- volatile dependencies
- multiple implementations
- testing difficulty
- meaningful domain separation

## 1.7 Make Expected Behavior Unsurprising

Code should behave as its names and structure suggest.

A developer reading:

```ts
isUserActive()
```

should expect a query.

They should not discover that it also:

- updates the database
- sends a notification
- changes a session
- creates another entity

Prefer predictable APIs.

---

# 2. Meaningful Names

Naming is part of design.

Take time to choose names carefully.

Rename identifiers whenever a better name becomes apparent.

## 2.1 Use Intention-Revealing Names

Names should answer questions such as:

- What is this?
- Why does it exist?
- What does it represent?
- What does this operation do?

Avoid:

```ts
const d = 10;
const tmp = calculate();
const data = await repository.find();
const x = user.status;
```

Prefer:

```ts
const elapsedDays = 10;
const calculatedTax = calculateTax();
const activeCustomers = await repository.findActiveCustomers();
const accountStatus = user.status;
```

If a variable needs a comment to explain what it represents, first improve the variable name.

## 2.2 Avoid Disinformation

Do not use names that suggest something inaccurate.

Do not let names expose outdated implementation assumptions.

## 2.3 Make Meaningful Distinctions

When two things are different, their names should explain how they differ.

Avoid meaningless distinctions such as:

```ts
user1
user2

customerData
customerInfo

product
productObject
```

Prefer:

```ts
billingAddress
shippingAddress

requestedUser
authenticatedUser

currentConfiguration
previousConfiguration
```

## 2.4 Avoid Noise Words

Words such as the following often add little meaning:

- Data
- Info
- Object
- Manager
- Processor
- Helper
- Util
- Handler
- Thing
- Stuff

They are not completely forbidden.

Use them only when they accurately describe a recognized responsibility.

## 2.5 Use Pronounceable Names

Developers must be able to discuss code verbally.

Avoid cryptic abbreviations.

Common industry abbreviations are acceptable when universally understood in the project.

Examples:

- id
- url
- api
- http
- dto
- jwt
- sql

Do not invent abbreviations merely to save typing.

## 2.6 Use Searchable Names

Names that may need to be found throughout a codebase should be easy to search.

Avoid magic literals:

```ts
if (retryCount > 5) {
}
```

Prefer:

```ts
const MAX_RETRY_ATTEMPTS = 5;

if (retryCount > MAX_RETRY_ATTEMPTS) {
}
```

Avoid single-letter variables outside tiny local scopes.

## 2.7 Match Name Length to Scope

The longer an identifier remains visible or important, the more descriptive its name should be.

## 2.8 Avoid Encodings

Do not encode unnecessary type or scope information into names.

Avoid:

```ts
strUsername
arrUsers
objCustomer
m_description
iCounter
```

Names should describe meaning, not implementation metadata.

## 2.9 Avoid Mental Mapping

Do not require readers to remember that cryptic letters represent business concepts.

Professional code reduces the amount of information a developer must mentally translate.

## 2.10 Class Names Should Normally Be Nouns

Good examples:

- Customer
- Invoice
- Complaint
- Workflow
- Application
- AuditReport
- PermissionPolicy
- Notification

## 2.11 Method Names Should Normally Express Actions

Prefer:

```ts
createApplication()
calculatePayment()
approveComplaint()
sendNotification()
deleteDocument()
```

Boolean methods should naturally answer questions:

```ts
isActive()
hasPermission()
canApprove()
requiresReview()
```

## 2.12 Names Must Describe Side Effects

If a method has an important side effect, its name must not hide it.

Bad:

```ts
getUser()
```

when it creates the user if absent.

Better:

```ts
findOrCreateUser()
```

## 2.13 Use One Word Per Concept

Use consistent terminology for equivalent concepts.

## 2.14 Do Not Use One Word for Multiple Concepts

Use the most accurate verb for the actual operation:

- append
- insert
- enqueue
- register
- attach

## 2.15 Use Solution-Domain Terminology

When representing known programming concepts, use established technical terms.

## 2.16 Use Problem-Domain Terminology

Use language understood by domain experts and established by the product.

## 2.17 Add Meaningful Context

Use classes, namespaces, modules, and structures to provide context.

## 2.18 Do Not Add Gratuitous Context

Do not repeat application/module context inside every identifier when the enclosing structure already provides it.

## 2.19 Use Standard Nomenclature

Where standard language, framework, or design terminology exists, use it consistently.

## 2.20 Names Must Be Unambiguous

A reader should not need to inspect implementation details to understand which concept a name represents.

---

# 3. Functions

Functions are fundamental units of readability.

Functions should tell a clear story.

## 3.1 Keep Functions Small

Prefer small functions.

Large functions frequently indicate:

- multiple responsibilities
- mixed abstraction levels
- hidden concepts
- excessive branching
- excessive temporary state

There is no universal mandatory line limit.

Do not mechanically split a clear function into meaningless fragments.

## 3.2 Functions Should Do One Thing

A function should represent one coherent operation.

It should:

- do one thing
- do it well
- avoid unrelated responsibilities

## 3.3 Detect Multiple Responsibilities Through Extractability

If a section can be extracted into another meaningful function whose name represents a separate concept, the original function may be doing too much.

## 3.4 Avoid Sections Inside Functions

A function containing large conceptual sections such as validation, transformation, database, and notification is likely performing multiple responsibilities.

Prefer meaningful functions or objects instead.

## 3.5 Maintain One Level of Abstraction

Do not mix high-level business behavior with low-level implementation details.

## 3.6 Follow the Stepdown Principle

Code should read like a top-down narrative.

High-level functions should describe the process.

Lower-level functions should explain increasingly detailed implementation.

## 3.7 Keep Nesting Shallow

Use:

- guard clauses
- extracted functions
- meaningful predicates
- polymorphism
- appropriate data structures

when they improve clarity.

## 3.8 Keep Conditional Blocks Small

Large bodies inside `if`, `else`, loops, `switch`, or `catch` should usually be extracted into meaningful operations.

## 3.9 Use Descriptive Function Names

Avoid vague names such as:

- process
- execute
- handle
- run
- doTask
- doStuff

unless enclosing context completely removes ambiguity.

## 3.10 Keep Argument Counts Low

Arguments increase cognitive load.

Prefer approximately:

- zero when natural
- one where practical
- two when meaningful
- three only when clearly justified
- more than three should trigger design review

This is not a mathematical law.

## 3.11 Use Argument Objects for Real Concepts

When multiple arguments naturally form a concept, encapsulate them.

Do not create meaningless bags of parameters.

## 3.12 Avoid Flag Arguments

Boolean flags frequently indicate that a function performs different operations.

Prefer explicit operations when behavior differs.

## 3.13 Avoid Selector Arguments

Arguments that select substantially different execution paths often hide several functions inside one.

Consider polymorphism, separate functions, or strategies when appropriate.

## 3.14 Avoid Output Arguments

Arguments are normally expected to represent inputs.

Avoid surprising mutation.

## 3.15 Avoid Hidden Side Effects

A function must not secretly change unrelated state.

## 3.16 Separate Commands and Queries

A function should generally either:

- change system state, or
- answer a question

Avoid ambiguous combinations.

## 3.17 Prefer Exceptions or Explicit Error Types Over Error Codes

Use the application's established error strategy.

The normal execution path should remain readable.

## 3.18 Extract Error Processing

Avoid large `try/catch` structures mixed with business logic.

## 3.19 Error Handling Is a Responsibility

Functions devoted to error handling should focus on error handling.

## 3.20 Do Not Repeat Yourself

Avoid duplication of:

- algorithms
- conditions
- business rules
- transformations
- validations
- constants
- mapping logic

Do not abstract coincidentally similar code that represents unrelated concepts.

## 3.21 Multiple Returns Are Acceptable When They Improve Clarity

Do not mechanically enforce one return statement.

Small functions may use guard clauses and early returns when they improve readability.

Avoid `goto`.

---

# 4. Comments

Comments are secondary to expressive code.

Try, in this order:

1. improve the name
2. improve the function structure
3. introduce an explanatory variable
4. extract a meaningful operation
5. improve abstraction
6. comment only when useful information still cannot be expressed clearly

## 4.1 Comments Do Not Compensate for Bad Code

Do not explain confusing implementation instead of cleaning it.

## 4.2 Explain Intent Through Code

If a comment can be replaced by a descriptive function or variable, prefer the code.

## 4.3 Appropriate Comments

Comments may legitimately contain:

- legal information
- licensing requirements
- explanation of non-obvious intent
- rationale behind unusual decisions
- warnings about consequences
- clarification of external APIs
- public API documentation
- carefully managed TODOs
- algorithmic reasoning that cannot reasonably be expressed directly

## 4.4 Comments Should Explain Why

Comments are most valuable when they explain reasoning unavailable from the code.

## 4.5 Avoid Inappropriate Information

Do not place information in code comments that belongs in:

- issue trackers
- source control
- design documentation
- architecture documentation
- release notes
- external specifications

## 4.6 Remove Obsolete Comments

When code changes, verify comments.

Delete comments that are no longer correct.

## 4.7 Avoid Redundant Comments

Comments should not simply repeat code.

## 4.8 Write Comments Clearly

If a comment is necessary, write it carefully.

Do not leave vague explanations, incomplete thoughts, misleading statements, or unexplained abbreviations.

## 4.9 Never Keep Commented-Out Code

Version control already preserves history.

Delete dead code.

## 4.10 Avoid Journal Comments

Use Git history.

## 4.11 Avoid Noise Comments

Do not comment obvious properties, constructors, or getters simply to satisfy a documentation quota.

## 4.12 Avoid Position Markers Unless Genuinely Useful

Good module organization should communicate structure.

## 4.13 Avoid Closing-Brace Comments

Shorten overly long structures instead.

## 4.14 Keep Comments Local

A comment should describe the code near which it appears.

## 4.15 Avoid Excessive Information

Do not include long historical discussions or specification reproductions in comments when external documentation is more appropriate.

## 4.16 Ensure Comment-to-Code Connection Is Obvious

A reader should immediately know what code the comment explains.

## 4.17 Public APIs Should Be Properly Documented

Document purpose, important parameters, significant constraints, externally relevant failures, and behavior consumers need to know.

## 4.18 Private Methods Usually Do Not Need Documentation Headers

Well-named private implementation should normally explain itself.

## 4.19 TODOs Must Be Controlled

TODOs must be meaningful, actionable, and reviewed.

---

# 5. Formatting

Formatting communicates structure.

## 5.1 Formatting Must Be Consistent

Follow repository formatting rules and automated formatters.

## 5.2 Follow the Newspaper Principle

A source file should become more detailed as the reader moves downward.

## 5.3 Use Vertical Openness Between Concepts

Separate unrelated concepts with whitespace.

## 5.4 Use Vertical Density for Related Concepts

Code that belongs together should remain together.

## 5.5 Keep Related Code Vertically Close

Avoid forcing developers to navigate excessive distances.

## 5.6 Declare Variables Close to Their Use

Keep variable lifetime and scope as small as practical.

## 5.7 Place Dependent Functions Near Each Other

Keep strongly related functions conceptually close when the language/project structure permits.

## 5.8 Keep Conceptual Affinity Visible

Related concepts should be located near each other.

## 5.9 Order Functions Top-Down

Prefer high-level orchestration first and lower-level implementation afterward.

## 5.10 Keep Lines Reasonably Short

Use formatting and extraction to preserve readability.

## 5.11 Use Whitespace to Reveal Association

Spacing should help readers understand structure.

## 5.12 Avoid Artificial Horizontal Alignment

Do not create fragile manual alignment.

## 5.13 Indentation Must Reflect Scope

Deep indentation is a warning sign of excessive nesting.

## 5.14 Team Rules Override Personal Preference

Use one consistent team style.

---

# 6. Objects and Data Structures

## 6.1 Hide Implementation Details

Objects should expose meaningful behavior rather than unnecessary internal representation.

## 6.2 Data Abstraction Is More Than Getters and Setters

Expose concepts rather than raw implementation details.

## 6.3 Choose Object or Data-Structure Semantics Deliberately

Objects generally hide data and expose behavior.

Data structures generally expose data and contain little behavior.

DTOs are often legitimate data structures.

## 6.4 Avoid Object/Data Hybrids

Choose the intended abstraction deliberately.

## 6.5 Follow the Law of Demeter in Spirit

A module should know mainly about its immediate collaborators.

## 6.6 Avoid Train Wrecks

Long method/property chains tightly couple code to object structure.

## 6.7 Hide Structure

Ask objects to perform meaningful operations rather than extracting internals for external manipulation where practical.

## 6.8 DTOs Are Acceptable Data Structures

Keep DTOs primarily focused on transport, serialization, validation metadata, and representation.

## 6.9 Active Record Requires Deliberate Use

Where Active Record is used, recognize the coupling trade-offs and follow existing architectural conventions.

---

# 7. Error Handling

## 7.1 Use the Project's Consistent Error Strategy

Choose a consistent strategy by layer.

## 7.2 Prefer Exceptions to Returned Error Codes Where Appropriate

Use clearer failure mechanisms when supported by the language/framework.

## 7.3 Establish Error Handling Early

Consider failure boundaries and cleanup before filling in the entire normal flow.

## 7.4 Provide Context With Exceptions

Diagnostic context should help developers understand the failure.

Never leak sensitive data unnecessarily.

## 7.5 Define Exception Types Around Caller Needs

Translate low-level failures at appropriate boundaries.

## 7.6 Preserve the Normal Flow

The primary behavior should remain easy to read.

## 7.7 Do Not Return Null Casually

Use a clear and predictable absence contract.

## 7.8 Do Not Pass Null Casually

Avoid APIs where `null` silently selects behavior.

## 7.9 Never Silently Swallow Exceptions

If an error is intentionally ignored, make that decision clear.

## 7.10 Do Not Catch What You Cannot Handle

Catch only when you recover, translate, add meaningful context, clean up, or log at an appropriate boundary.

---

# 8. Boundaries and Third-Party Code

## 8.1 Keep Third-Party APIs Behind Boundaries

Avoid scattering vendor-specific calls across business logic.

## 8.2 Minimize the Exposed External Interface

Expose only what the application actually needs.

## 8.3 Do Not Create Meaningless Wrappers

Boundaries should simplify or stabilize usage.

## 8.4 Use Learning Tests

When integrating unfamiliar third-party code, write focused tests to understand behavior and assumptions.

## 8.5 Protect Code From Interfaces That Do Not Yet Exist

Define the interface the application wishes it had and adapt later when appropriate.

## 8.6 Keep Boundaries Clean

Depend on stable abstractions rather than unnecessary implementation details.

---

# 9. Unit Tests

## 9.1 Maintain a Comprehensive Test Suite

Significant behavior that could break should be meaningfully tested.

## 9.2 Keep Tests Maintainable

Tests should be readable, simple, focused, deterministic, and easy to diagnose.

## 9.3 Tests Enable Change

A clean test suite supports safe refactoring and upgrades.

## 9.4 Create Domain-Specific Testing Helpers

Use helpers/builders when they genuinely improve test readability.

## 9.5 Production and Test Code May Have Different Constraints

Tests may prioritize readability differently, but must remain clean.

## 9.6 Prefer One Concept Per Test

Multiple assertions are acceptable when they verify one conceptual outcome.

## 9.7 Follow FIRST

Tests should aim to be:

- Fast
- Independent
- Repeatable
- Self-validating
- Timely

## 9.8 Use Coverage Tools

Use coverage diagnostically, not as proof of correctness.

## 9.9 Do Not Skip Trivial Tests Automatically

Cheap regression protection can be valuable.

## 9.10 Treat Ignored Tests as Questions

Skipped tests must have a clear reason.

## 9.11 Test Boundary Conditions

Explicitly test edges and limits.

## 9.12 Test Extensively Near Bugs

When fixing a bug, reproduce it with a test when practical and test neighboring behavior.

## 9.13 Look for Patterns in Test Failures

Repeated failure characteristics may reveal the root cause.

## 9.14 Coverage Patterns Can Reveal Problems

Compare execution paths of passing and failing tests.

## 9.15 Tests Must Be Fast

Separate unit, integration, and end-to-end tests appropriately.

---

# 10. Classes

## 10.1 Follow Consistent Class Organization

Keep classes easy to scan.

## 10.2 Preserve Encapsulation

Keep implementation details private where possible.

## 10.3 Classes Should Be Small in Responsibility

Large responsibility surfaces are warning signs.

## 10.4 Follow Single Responsibility

A class should have one primary reason to change.

## 10.5 Maintain High Cohesion

Methods and fields inside a class should strongly relate.

## 10.6 Split Classes When Cohesion Falls

Extract cohesive responsibilities.

## 10.7 Organize for Change

Place related behavior together according to why it changes.

## 10.8 Isolate From Volatile Dependencies

Separate behavior from dependencies likely to change when the boundary justifies it.

---

# 11. Systems and Architecture

## 11.1 Separate System Construction From System Use

Keep dependency wiring/configuration separate from normal business execution.

## 11.2 Keep Main/Bootstrap Composition Separate

Business logic should primarily use dependencies rather than construct them.

## 11.3 Use Factories Where Construction Is Complex

Do not create factories for trivial constructors without reason.

## 11.4 Use Dependency Injection

Prefer explicit dependencies.

## 11.5 Scale Through Separation of Concerns

Do not let infrastructure choices spread through every module.

## 11.6 Separate Cross-Cutting Concerns

Use appropriate mechanisms for logging, auditing, authorization, transactions, metrics, caching, and telemetry.

## 11.7 Test-Drive Architectural Decisions

Validate important architecture through working code and tests.

## 11.8 Delay Decisions Until Enough Information Exists

Avoid premature commitment when requirements remain uncertain.

## 11.9 Optimize Decision-Making

Do not centralize every design decision unnecessarily.

## 11.10 Use Standards When They Demonstrably Add Value

Use standards when they improve interoperability, consistency, maintenance, reliability, security, or communication.

## 11.11 Build Domain-Specific Language Into the Code

Functions and classes should collectively form a vocabulary that describes the application's domain.

---

# 12. Emergent and Simple Design

## 12.1 The System Must Pass Its Tests

Correct observable behavior is the first requirement.

## 12.2 Remove Duplication

Eliminate duplicated knowledge.

## 12.3 Maximize Expressiveness

Code should clearly communicate intentions, responsibilities, concepts, and design decisions.

## 12.4 Minimize Classes and Methods

Do not create more entities than needed.

---

# 13. Concurrency

## 13.1 Separate Concurrent Behavior From Business Logic

Avoid unnecessary mixing of synchronization, scheduling, locking, and worker management with domain logic.

## 13.2 Limit Shared Data

Shared mutable state creates concurrency hazards.

## 13.3 Prefer Copies or Immutable Data Where Appropriate

Reducing shared mutation reduces synchronization complexity.

## 13.4 Keep Threads/Workers Independent

Independent units of work are easier to reason about.

## 13.5 Know the Concurrency Library

Prefer tested concurrency utilities over custom synchronization.

## 13.6 Use Thread-Safe Collections Where Appropriate

Do not assume normal data structures remain safe under concurrency.

## 13.7 Understand Execution Models

Know the concurrency model being implemented.

## 13.8 Beware Dependencies Between Synchronized Operations

Reason about sequences, not only individual methods.

## 13.9 Keep Critical Sections Small

Protect only what requires synchronization.

## 13.10 Shutdown Is Part of Correctness

Define graceful shutdown, in-flight handling, cancellation, cleanup, and timeout behavior.

## 13.11 Treat Intermittent Failures as Potential Concurrency Bugs

Do not dismiss rare failures as random without investigation.

## 13.12 Get Non-Concurrent Code Working First

Verify normal logic first where practical.

## 13.13 Make Concurrency Pluggable Where Practical

Business logic should be testable without relying only on real thread scheduling.

## 13.14 Make Concurrency Tunable Where Appropriate

Worker/thread counts and limits should be configurable when useful.

## 13.15 Stress Concurrent Code

Test under load, concurrency, repetition, and varied scheduling.

## 13.16 Instrument Concurrency Tests Where Necessary

Create conditions that increase the probability of exposing race conditions.

---

# 14. Successive Refinement and Refactoring

## 14.1 Start With Working Behavior

Understand the problem and obtain correct behavior.

## 14.2 Then Improve the Design

Improve naming, extract functions, simplify conditions, remove duplication, reduce arguments, move responsibilities, split classes, and improve abstractions.

## 14.3 Refactor Incrementally

Prefer small behavior-preserving transformations.

## 14.4 Keep Tests Passing During Refactoring

Do not perform large structural changes without verifying behavior.

## 14.5 Understand Before Refactoring

Inspect callers, tests, behavior, dependencies, and edge cases.

## 14.6 Refactoring Is Continuous

Refactoring is normal development work.

---

# 15. Environment Rules

## 15.1 The Build Should Require One Clear Operation

Avoid undocumented multi-step manual processes.

## 15.2 Running Tests Should Require One Clear Operation

Do not require developers to remember hidden manual preparation.

---

# 16. General Code Smells and Heuristics

## 16.1 Avoid Multiple Languages in One Source File

Separate substantial embedded languages when practical.

## 16.2 Implement Obvious Expected Behavior

Do not create surprising API gaps.

## 16.3 Correct Boundary Behavior

Explicitly inspect first, last, minimum, maximum, empty, zero, and exact limits.

## 16.4 Do Not Override Safety Mechanisms

Do not disable tests, validation, type checking, compiler warnings, security mechanisms, or lint rules merely to make code pass.

## 16.5 Eliminate Duplication

Duplicated knowledge should have one authoritative location.

## 16.6 Keep Code at the Correct Abstraction Level

High-level modules should contain high-level concepts.

## 16.7 Base Classes Should Not Depend on Their Derivatives

Preserve the intended abstraction direction.

## 16.8 Do Not Expose Too Much Information

Minimize public fields, helper methods, unnecessary getters/setters, implementation details, and concrete dependency types.

## 16.9 Remove Dead Code

Delete unused methods, unreachable branches, obsolete classes, unused variables, imports, and abandoned configuration.

## 16.10 Avoid Unnecessary Vertical Separation

Keep related concepts close.

## 16.11 Maintain Consistency

Similar operations should follow similar structure and naming.

## 16.12 Remove Clutter

Remove elements that contribute neither information nor behavior.

## 16.13 Avoid Artificial Coupling

Place constants, enums, shared functions, and interfaces where they conceptually belong.

## 16.14 Watch for Feature Envy

Behavior that mainly manipulates another object's data may belong closer to that object.

## 16.15 Avoid Selector Arguments

Prefer explicit operations or polymorphism when appropriate.

## 16.16 Avoid Obscured Intent

Prefer standard, readable constructs over clever ones.

## 16.17 Put Responsibilities in the Correct Place

Controllers should not become domain services; repositories should not become workflow engines.

## 16.18 Avoid Inappropriate Static Behavior

Use static behavior only when conceptually appropriate.

## 16.19 Use Explanatory Variables

Break complex expressions into named concepts where doing so improves readability.

## 16.20 Function Names Must Say What Functions Do

Rename or split functions that hide consequential behavior.

## 16.21 Understand the Algorithm

Do not patch until tests pass without understanding why.

## 16.22 Make Logical Dependencies Explicit

Do not rely on hidden assumptions or distant implicit state.

## 16.23 Prefer Polymorphism Over Repeated Type Switching

Repeated type-based behavior may indicate missing polymorphism.

Do not replace every simple switch with unnecessary classes.

## 16.24 Follow Standard Conventions

Use established language/framework conventions.

## 16.25 Replace Magic Numbers With Named Constants

Name values whose meaning is not obvious from context.

## 16.26 Be Precise

Be precise regarding nullability, units, time zones, limits, ownership, status transitions, validation, concurrency, side effects, and expected exceptions.

## 16.27 Prefer Structure Over Developer Memory

Use types, APIs, encapsulation, and relationships to enforce important rules.

## 16.28 Encapsulate Complex Conditionals

Use meaningful predicates when a condition represents a real business rule.

## 16.29 Prefer Positive Conditions

Use the clearest form; avoid unnecessary double negatives.

## 16.30 Functions Should Do One Thing

If a function combines independent responsibilities, split it.

## 16.31 Make Temporal Coupling Explicit

Encapsulate required call sequences where practical.

## 16.32 Do Not Be Arbitrary

Every placement and structure should have a reason.

## 16.33 Encapsulate Boundary Conditions

Centralize or name important boundary calculations where useful.

## 16.34 Functions Should Descend One Level of Abstraction

Do not jump unpredictably between architectural and implementation detail.

## 16.35 Keep Configurable Data at High Levels

Do not bury retry limits, URLs, timeouts, feature settings, or thresholds deep inside algorithms.

## 16.36 Avoid Transitive Navigation

A module should generally communicate with immediate collaborators.

---

# 17. Java-Specific Guidance From Clean Code

Apply only when working with Java and reconcile with the repository's established conventions.

## 17.1 Avoid Excessively Long Import Sections

Follow repository/linter conventions first.

Keep imports organized and remove unused imports.

## 17.2 Do Not Inherit Constants

Never use inheritance merely to gain access to constants.

## 17.3 Prefer Enums for Closed Sets of Meaningful Values

Use enums where they improve type safety, readability, and domain expression.

---

# 18. Naming Heuristics

Before accepting a name, verify:

- it is descriptive
- it is at the correct abstraction level
- it uses standard nomenclature
- it is unambiguous
- its length suits its scope
- it avoids unnecessary encoding
- it reveals important side effects

---

# 19. Testing Heuristics

Verify:

- there are enough meaningful tests
- coverage was examined where appropriate
- simple behaviors were not ignored without reason
- ignored tests are understood
- boundaries were tested
- the area around bugs was tested
- failure patterns were considered
- coverage patterns were considered
- tests are fast enough for frequent use

---

# 20. Security and Clean Code

Clean code does not replace secure coding.

## 20.1 Security Logic Must Be Explicit

Authentication and authorization rules should be easy to locate and understand.

## 20.2 Do Not Duplicate Authorization Logic

Centralize meaningful policies.

## 20.3 Keep Validation Consistent

Do not implement the same validation rule differently in multiple modules.

## 20.4 Never Hide Security Side Effects

Operations involving authentication, sessions, tokens, permissions, encryption, or account state must behave predictably.

## 20.5 Do Not Leak Sensitive Information Through Errors

Internal diagnostics and external responses may require different levels of detail.

---

# 21. Rules for AI Coding Agents

These rules are mandatory whenever an AI agent creates or changes code.

## 21.1 Inspect Existing Code First

Inspect surrounding files, callers, types, interfaces, related tests, and repository conventions.

## 21.2 Preserve Existing Architecture Unless Change Is Justified

Follow the project's established structure unless a change is required and justified.

## 21.3 Preserve Behavior Unless Change Is Requested

Refactoring must not silently alter business behavior.

## 21.4 Reuse Existing Concepts

Before introducing a new service, helper, utility, enum, constant, DTO, interface, repository, or validator, search for an existing equivalent.

## 21.5 Match Existing Domain Language

Use vocabulary already established by requirements, domain models, existing code, and product terminology.

## 21.6 Do Not Create Generic Dumping Grounds

Avoid `utils.ts`, `helpers.ts`, `common.ts`, or `misc.ts` becoming collections of unrelated behavior.

## 21.7 Do Not Generate Obvious Comments

Avoid AI-generated comment noise.

## 21.8 Never Generate Commented-Out Alternatives

Choose the appropriate implementation and rely on version control.

## 21.9 Do Not Hide Compiler Problems

Never use `any`, unsafe casts, `@ts-ignore`, disabled lint rules, or swallowed errors merely to finish implementation faster.

## 21.10 Do Not Disable Tests to Complete a Task

Understand failing tests and fix the correct issue.

## 21.11 Do Not Overengineer

Before introducing an abstraction, ask:

1. What concrete problem does it solve?
2. Does it eliminate real duplication?
3. Does it isolate a real dependency?
4. Does it improve readability?
5. Does it improve testability?
6. Is variation actually expected?
7. Is the indirection worth its cost?

## 21.12 Refactor Generated Code Before Completion

Review naming, duplication, error behavior, control flow, responsibilities, abstractions, and tests before finishing.

---

# 22. Code Review Procedure

## Naming

- [ ] Names are intention-revealing.
- [ ] Names are unambiguous.
- [ ] Domain terms are consistent.
- [ ] Unnecessary abbreviations are avoided.
- [ ] Magic values are reviewed/named where appropriate.
- [ ] Method names expose side effects.
- [ ] Implementation details are not unnecessarily encoded in names.

## Functions

- [ ] Each function has one coherent responsibility.
- [ ] Functions are reasonably small.
- [ ] Abstraction levels are consistent.
- [ ] Nesting is shallow.
- [ ] Arguments are reasonable.
- [ ] Output arguments are avoided.
- [ ] Flag/selector arguments are justified.
- [ ] Hidden side effects are absent.
- [ ] Commands and queries are clear.
- [ ] Duplicated logic is removed.

## Comments

- [ ] Comments provide useful information not clear from code.
- [ ] Comments are accurate.
- [ ] Obsolete comments are removed.
- [ ] Commented-out code is absent.
- [ ] TODOs are actionable.
- [ ] Obvious comments are removed.

## Formatting

- [ ] Formatting is consistent.
- [ ] Related concepts are close.
- [ ] Code reads top-down.
- [ ] Indentation is reasonable.
- [ ] Files are easy to scan.

## Objects and Classes

- [ ] Implementation details are encapsulated.
- [ ] Each class has focused responsibility.
- [ ] Cohesion is high.
- [ ] Public surface area is minimized.
- [ ] Object/data-structure choice is deliberate.
- [ ] Train-wreck chains are avoided.
- [ ] God classes are avoided.

## Errors

- [ ] The project's error strategy is followed.
- [ ] Exceptions contain useful context.
- [ ] Exceptions are not silently swallowed.
- [ ] Normal flow is understandable.
- [ ] Nullability is predictable.
- [ ] Errors are caught only where useful.

## Boundaries

- [ ] External dependencies are appropriately isolated.
- [ ] Third-party implementation does not leak unnecessarily.
- [ ] Boundary interfaces are narrow.
- [ ] Wrappers are meaningful rather than ceremonial.

## Tests

- [ ] New behavior is tested.
- [ ] Boundary conditions are tested.
- [ ] Bug regressions are tested.
- [ ] Tests are independent.
- [ ] Tests are deterministic.
- [ ] Tests are readable.
- [ ] Tests are fast.
- [ ] Ignored tests are justified.
- [ ] Coverage was checked where appropriate.

## Maintainability

- [ ] Duplication is minimized.
- [ ] Dead code is removed.
- [ ] Clutter is removed.
- [ ] Responsibilities are correctly located.
- [ ] Likely future changes are localized.
- [ ] Unnecessary abstraction is avoided.
- [ ] The changed area is cleaner than before.

---

# 23. Rules for Refactoring Existing Code

When encountering messy existing code, do not automatically rewrite the whole module.

Recommended order:

1. understand behavior
2. inspect or create relevant tests
3. add regression coverage where needed
4. improve misleading names
5. remove dead code
6. remove obsolete comments
7. extract meaningful functions
8. reduce duplication
9. simplify conditionals
10. reduce hidden state
11. move misplaced responsibilities
12. split low-cohesion classes
13. improve dependency boundaries
14. keep tests passing throughout

Prefer incremental refactoring unless a rewrite is clearly safer and more practical.

---

# 24. Rule Conflicts and Engineering Judgment

Clean Code rules are not mathematical laws.

Examples:

- a `switch` may be clearer than unnecessary polymorphism
- a longer function may be clearer than artificial fragmentation
- three arguments may naturally belong to an operation
- `null` may be part of a framework contract
- a DTO may correctly expose data
- an interface may be unnecessary with one stable implementation
- duplication may be preferable to a false abstraction
- a comment may be necessary for non-obvious business reasoning
- performance requirements may justify additional complexity

Use engineering judgment.

---

# 25. Priority Order

When trade-offs are necessary, prioritize:

1. Correctness
2. Security
3. Data integrity
4. Required business behavior
5. Readability
6. Maintainability
7. Testability
8. Simplicity
9. Consistency
10. Performance where performance matters
11. Conciseness

Never sacrifice correctness simply to make code aesthetically cleaner.

Never sacrifice readability merely to reduce line count.

Never sacrifice maintainability for speculative optimization.

Never introduce complexity simply to follow a design pattern.

---

# 26. Definition of Done for Code

A coding task is not complete merely because:

- the code compiles
- the endpoint responds
- the UI renders
- one test passes

Before completion:

- [ ] Required behavior works.
- [ ] Existing relevant behavior is preserved.
- [ ] Names clearly express intent.
- [ ] Functions have focused responsibilities.
- [ ] Classes/modules remain cohesive.
- [ ] Control flow is understandable.
- [ ] Important boundaries are handled.
- [ ] Errors are handled appropriately.
- [ ] Duplication has been reviewed.
- [ ] Magic values have been reviewed.
- [ ] Dead code has been removed.
- [ ] Commented-out code has been removed.
- [ ] Comments are accurate and useful.
- [ ] Relevant tests exist.
- [ ] Important edge cases are tested.
- [ ] Repository conventions are followed.
- [ ] No unnecessary abstractions were introduced.
- [ ] External dependencies are appropriately contained.
- [ ] Security-sensitive behavior is explicit.
- [ ] The touched code is at least as clean as before.

---

# 27. Final Development Principle

The objective of Clean Code is not:

- minimum lines
- maximum abstractions
- maximum number of classes
- perfect adherence to patterns
- aesthetically clever code

The objective is software whose behavior and design can be understood and changed safely.

Whenever writing or reviewing code, ask:

- What does this code actually mean?
- Can another developer understand it quickly?
- Does the name tell the truth?
- Does this function have one coherent purpose?
- Is this responsibility in the correct place?
- Am I exposing implementation details unnecessarily?
- Is there duplicated knowledge?
- Is there hidden behavior?
- Is this conditional unnecessarily complicated?
- Are important boundaries handled?
- Can failures be diagnosed?
- Can this code be tested easily?
- Is this abstraction solving a real problem?
- Can the design be simpler?
- Did I make the codebase better rather than merely adding code?

Prefer code that is simple, explicit, expressive, tested, and easy to change.
