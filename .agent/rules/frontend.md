# Frontend Development Rules

## Purpose

These rules apply to web/mobile UI code, components, state, forms, API integration, and user-facing behavior.

---

# 1. Component Design

- Components should have focused responsibilities.
- Avoid giant components containing data fetching, business logic, formatting, and complex presentation together.
- Extract reusable behavior only when it represents a real reusable concept.
- Prefer composition over deeply configurable "do everything" components.
- Keep presentational and stateful concerns reasonably separated where it improves clarity.

---

# 2. State Management

- Keep state as local as possible.
- Lift state only when multiple consumers require it.
- Avoid duplicating derived state.
- Compute derived values from source state when practical.
- Avoid global state for data that belongs to one screen/component.
- Keep server state and client UI state conceptually distinct.
- Define loading, empty, error, and success states explicitly.

---

# 3. Effects

- Use effects only for side effects.
- Do not use effects to implement ordinary derived values that can be calculated during render/computation.
- Keep effect dependencies correct.
- Clean up subscriptions/timers/listeners.
- Avoid effect chains that simulate imperative workflows unnecessarily.

---

# 4. Forms

- Validate on client for UX and on server for trust/security.
- Show useful field-level errors.
- Preserve entered values after recoverable failures.
- Prevent duplicate submissions.
- Make loading/submitting states clear.
- Do not hide validation rules only inside UI code.

---

# 5. API Integration

- Keep API access in dedicated clients/hooks/services where appropriate.
- Do not scatter raw fetch logic through many components.
- Normalize error handling.
- Handle cancellation/race conditions where stale requests can overwrite newer state.
- Avoid relying on undocumented response fields.

---

# 6. Accessibility

- Use semantic HTML first.
- Every interactive control must be keyboard accessible.
- Inputs require associated labels.
- Images require appropriate alt text when meaningful.
- Do not rely only on color to communicate state.
- Preserve focus behavior for dialogs/forms/navigation.
- Use ARIA only when native semantics are insufficient.

---

# 7. User Experience

- Always define:
  - loading state
  - empty state
  - error state
  - disabled state
  - success feedback where needed
- Avoid silent failures.
- Avoid destructive actions without appropriate confirmation or undo where risk warrants it.
- Preserve user context during validation errors.

---

# 8. Styling

- Follow repository design-system conventions.
- Avoid one-off styles when a shared token/component exists.
- Keep styling maintainable and scoped.
- Do not duplicate large style blocks across components.
- Prefer responsive layouts that degrade gracefully.

---

# 9. Security

- Never trust client-side authorization.
- Do not store sensitive tokens in unsafe storage without architectural justification.
- Avoid rendering unsanitized HTML.
- Do not expose secrets/configuration values intended only for backend use.
- Treat all browser-delivered code/configuration as public.

---

# 10. Performance

- Avoid unnecessary rerenders only when they are real or likely bottlenecks.
- Do not overuse memoization.
- Lazy-load heavy screens/assets where beneficial.
- Virtualize truly large lists.
- Optimize images/assets.
- Avoid unnecessary network requests.

---

# 11. Frontend Review Checklist

- [ ] Component responsibility is focused.
- [ ] State is not duplicated unnecessarily.
- [ ] Derived values are not stored unnecessarily.
- [ ] Effects are used only for real side effects.
- [ ] Loading/empty/error states exist.
- [ ] Form validation is clear.
- [ ] API access is consistent.
- [ ] Accessibility is preserved.
- [ ] No unsafe HTML rendering exists.
- [ ] No secrets are exposed.
- [ ] Styling follows design-system conventions.
- [ ] Performance changes are justified, not speculative.
