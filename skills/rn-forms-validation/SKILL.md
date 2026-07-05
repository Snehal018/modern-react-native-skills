---
name: rn-forms-validation
description: Use when adding or refactoring React Native forms, edit/create screens, React Hook Form controllers, Zod schemas, field errors, submit states, keyboard behavior, or form-driven mutations.
---

# rn-forms-validation

## Purpose

Build or reshape React Native forms so field state, validation, keyboard behavior, submission, error display, and mutation boundaries stay predictable and type-safe.

Read `../_shared/skill-system-constitution.md` before editing this skill or creating adjacent form skills.

Read `../_shared/react-native-modern-standard.md` before implementing. Prefer React Hook Form with Zod and hookform resolvers when adding substantial forms, unless the project has an established compatible alternative.

## Use this skill when

- The user asks to add or refactor a form, create/edit screen, onboarding step, settings form, search/filter form, or multi-step input flow.
- The work touches React Hook Form, Zod schemas, field-level validation, submit state, field errors, or form reset behavior.
- The work coordinates a form submit with a mutation, API service, navigation, or local state update.
- The work needs keyboard-aware layout, input focus progression, disabled submit behavior, or platform-specific input handling.
- The user asks for a form-specific review.

## Do not use this skill when

- The request is broad feature implementation and a form is only one part; use `add-react-native-feature` as the owning workflow.
- The work is only API cache behavior with no form state; use `rn-api-data`.
- The work is auth session persistence or token storage; use `rn-auth-secure-storage`.
- The app has not been initialized; use `setup-react-native`.

## Preconditions

Before changing form code:

- Inspect existing form libraries, schema libraries, input components, field wrappers, and validation patterns.
- Identify where schemas, types, hooks, services, and feature components live.
- Identify submit behavior, mutation hooks, and API service boundaries.
- Identify existing keyboard-aware layout, safe area, scroll, focus, and error display patterns.
- Identify accessibility conventions for labels, hints, errors, disabled states, and touch targets.

Stop before edits if the project has multiple competing form patterns and the requested change would deepen the inconsistency. Summarize the conflict and choose the smallest compatible pattern, or ask for a migration decision when needed.

## Process

1. Define the form contract: fields, initial values, validation rules, submit payload, success behavior, failure behavior, and reset behavior.
2. Reuse existing input primitives and field components when they are accessible and compatible.
3. Create or update a Zod schema for validation when adding a substantial form. Derive form value types from the schema when practical.
4. Keep form setup in the screen or a feature hook depending on complexity. Move repeated submit logic, derived values, or side effects into a hook when it improves clarity.
5. Use React Hook Form controllers or registered fields according to the project's existing React Native pattern.
6. Normalize values at the form boundary. Keep display formatting separate from submitted payload shape.
7. Show field errors near the relevant input and form-level errors near the submit area.
8. Coordinate submit with the data layer. Use mutation hooks for API writes and avoid embedding request logic in form components.
9. Handle pending, disabled, success, failure, retry, and cancellation states where relevant.
10. Account for keyboard behavior and small screens using existing layout patterns before adding new dependencies.
11. Run relevant validation and fix issues introduced by the form change. Complete when invalid values are blocked, valid values submit once, and errors are visible and understandable.

## Standards

Follow `../_shared/react-native-modern-standard.md`.

Form-specific conventions:

- Prefer React Hook Form with Zod for substantial forms.
- Keep schemas close to the feature unless shared across features.
- Keep API writes in mutation hooks or services, not input components.
- Use feature components for repeated field groups when they represent meaningful UI.
- Keep form state local unless there is a clear product reason to persist or share it.

## Validation checklist

- Existing form and input patterns were inspected before edits.
- Validation schema matches the submitted payload.
- Invalid values produce field or form errors.
- Valid submit triggers exactly the intended local update or mutation.
- Pending, disabled, success, and failure states are handled where applicable.
- API calls are not embedded in input or presentational components.
- Keyboard, scroll, focus, and safe-area behavior remain usable on small screens.
- Labels, errors, and disabled states are accessible according to project conventions.
- TypeScript, lint, tests, or app start/build checks were run when configured, or skipped with a concrete reason.

## Avoid

- Do not duplicate schemas, field wrappers, or input formatting helpers.
- Do not keep server mutation state in local form state when TanStack Query owns it.
- Do not submit display-formatted strings when the API expects normalized values.
- Do not hide validation errors until the user has no way to recover.
- Do not add a form library when the app already has a coherent compatible form pattern.
- Do not create tiny wrapper components for every field unless they remove meaningful duplication.
