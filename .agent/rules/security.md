# Security Development Rules

## Purpose

These rules apply to all code that handles user input, authentication, authorization, secrets, sensitive data, external integrations, file handling, persistence, cryptography, networking, or administrative operations.

Security must be designed into implementation, not added only after feature completion.

---

# 1. Core Principles

- Apply least privilege.
- Deny by default.
- Validate all untrusted input.
- Encode output for its destination.
- Use parameterized data access.
- Do not trust client-side validation.
- Keep secrets out of source code.
- Never log secrets, tokens, passwords, private keys, or sensitive personal data unnecessarily.
- Prefer secure defaults over configurable insecure behavior.
- Fail safely.
- Keep security-sensitive behavior explicit and reviewable.
- Do not weaken a security mechanism merely to make development easier.

---

# 2. Input Validation

- Validate at trust boundaries.
- Validate type, length, format, range, and allowed values.
- Prefer allow-lists for constrained inputs.
- Reject unexpected structure rather than silently coercing dangerous values.
- Validate nested objects and arrays.
- Enforce server-side validation even if frontend validation exists.
- Do not rely on regex alone for context-sensitive security.
- Use framework/library validation where mature and appropriate.

---

# 3. Injection Prevention

## SQL/Database
- Use parameterized queries or safe ORM query builders.
- Never concatenate untrusted input into SQL.
- Review dynamic identifiers separately; placeholders often do not protect table/column names.

## Shell
- Do not construct shell commands with untrusted strings.
- Prefer direct process APIs with argument arrays.
- Avoid shell execution when a native library/API exists.

## HTML
- Escape/encode untrusted output by default.
- Sanitize rich HTML with a vetted sanitizer.
- Never disable framework escaping without a documented reason.

## Templates
- Do not evaluate user-controlled templates or expressions unless explicitly required and sandboxed.

---

# 4. Authentication

- Use established authentication libraries/protocols.
- Never build custom password hashing.
- Store passwords only using approved password hashing algorithms.
- Apply rate limiting / lockout controls appropriate to the system.
- Do not reveal whether an account exists unless the product explicitly requires it.
- Sessions/tokens must have defined expiration and revocation behavior.
- Token validation must include issuer/audience/signature/expiry checks as applicable.

---

# 5. Authorization

- Perform authorization server-side.
- Authorization must be checked for every protected action, not only page access.
- Check object-level authorization, not only role-level authorization.
- Centralize reusable authorization policies.
- Do not trust user-supplied role, owner, tenant, or permission values.
- Avoid scattered duplicated authorization conditionals.
- Admin or privileged operations require explicit authorization checks.

---

# 6. Secrets and Credentials

- Never hard-code secrets.
- Use environment variables or approved secret stores.
- Do not commit `.env` files containing real credentials.
- Rotate exposed secrets immediately.
- Do not log secrets.
- Do not return secrets in API responses.
- Restrict secret access to only components that need it.

---

# 7. Sensitive Data

- Minimize collection and storage.
- Encrypt sensitive data in transit.
- Use encryption at rest where required.
- Avoid storing duplicate sensitive data unnecessarily.
- Apply retention and deletion requirements.
- Mask/redact sensitive values in logs and UI where appropriate.

---

# 8. Logging and Audit

- Log security-relevant events with useful context.
- Avoid sensitive payloads in logs.
- Include correlation/request identifiers where useful.
- Authentication/authorization failures should be observable without leaking secrets.
- Audit privileged changes where required.
- Log integrity should be protected through platform controls where applicable.

---

# 9. Error Handling

- External responses must not expose stack traces, SQL queries, secret values, internal file paths, or infrastructure details.
- Internal logs may contain diagnostic context, but still must not contain secrets.
- Fail closed for authorization/security controls.
- Do not swallow security-relevant exceptions silently.

---

# 10. Files and Uploads

- Validate file size.
- Validate expected file types/content.
- Do not trust file extensions alone.
- Generate server-controlled storage names when practical.
- Prevent path traversal.
- Store uploads outside executable application directories when practical.
- Apply malware scanning where system risk requires it.
- Restrict access to uploaded files.

---

# 11. External Requests / SSRF

- Do not allow arbitrary user-controlled URLs to be fetched without controls.
- Use allow-lists where applicable.
- Block access to internal metadata/private network ranges when external URL fetching is required.
- Set timeouts.
- Limit redirects.
- Limit response/body sizes.

---

# 12. Cryptography

- Use mature libraries.
- Do not invent algorithms or protocols.
- Do not use obsolete hashes/ciphers for security purposes.
- Use cryptographically secure random generation for secrets/tokens.
- Keep keys separate from encrypted data where appropriate.
- Define rotation and lifecycle for long-lived keys.

---

# 13. Dependencies

- Prefer actively maintained libraries.
- Avoid unnecessary dependencies.
- Review security implications of new dependencies.
- Keep dependencies updated through controlled upgrades.
- Do not ignore known critical vulnerabilities without documented risk acceptance.

---

# 14. Configuration

- Secure configuration must be the default.
- Production must not run with debug/development security settings.
- CORS must be explicitly configured.
- Security headers should be applied where relevant.
- Timeouts, upload limits, and rate limits should be explicit where relevant.

---

# 15. AI Coding Agent Rules

Before finalizing security-sensitive code, verify:

- [ ] Every untrusted input is validated.
- [ ] Authorization is checked at the correct boundary.
- [ ] No secrets are hard-coded or logged.
- [ ] Database access is parameterized.
- [ ] HTML/output contexts are encoded or sanitized.
- [ ] File handling prevents traversal and unsafe execution.
- [ ] External requests cannot be trivially abused for SSRF.
- [ ] Errors do not leak sensitive implementation details.
- [ ] Security behavior is tested where practical.
- [ ] No security control was disabled merely to make the implementation work.

If uncertain about a security-sensitive pattern, choose the safer established approach rather than inventing a custom mechanism.
