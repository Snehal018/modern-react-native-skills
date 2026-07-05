---
name: rn-api-data
description: Use when adding or refactoring React Native API clients, service functions, TanStack Query hooks, mutations, optimistic updates, cache keys, external response validation, or React Native online/focus data behavior.
---

# rn-api-data

## Purpose

Build or reshape React Native API and server-state code so network boundaries, runtime validation, query caching, mutation flows, and UI states are explicit and maintainable.

Read `../_shared/skill-system-constitution.md` before editing this skill or creating adjacent data skills.

Read `../_shared/react-native-modern-standard.md` before implementing. If TanStack Query is involved, follow the TanStack Query React Native branch in that shared standard before choosing focus, online, subscription, or devtools behavior.

## Use this skill when

- The user asks to add or refactor API clients, request helpers, service functions, query hooks, mutation hooks, or cache keys.
- The work touches TanStack Query configuration, query invalidation, optimistic updates, retries, pagination, pull-to-refresh, or stale data behavior.
- The work validates external API responses, error payloads, or request input with schemas.
- The work needs React Native app focus, network reconnect, or screen-focus data behavior.
- The user asks for a data-layer review or server-state refactor.

## Do not use this skill when

- The request is broad feature implementation and data is only one part; use `add-react-native-feature` as the owning workflow.
- The work is only local form validation with no API boundary; use `rn-forms-validation`.
- The work is token/session storage or auth refresh behavior; use `rn-auth-secure-storage`.
- The app has not been initialized; use `setup-react-native`.

## Preconditions

Before changing API or server-state code:

- Inspect `package.json` for data, validation, storage, and network packages.
- Identify existing API clients, base URL handling, auth headers, error handling, and request helpers.
- Identify existing TanStack Query provider setup, query client defaults, query key conventions, and mutation patterns.
- Identify where services, hooks, schemas, and types live.
- Identify loading, error, empty, success, refresh, and retry UI patterns.
- Check whether app focus, online status, and screen focus behavior are already configured.

Stop before edits if the project has competing API clients or state patterns that make the target behavior ambiguous. Summarize the conflict and propose a focused normalization path.

## Process

1. Define the data contract: endpoint, request shape, response shape, errors, auth requirements, pagination, and cache lifetime where relevant.
2. Locate or create the smallest coherent service boundary. Keep request execution in services or existing API modules, not in UI components.
3. Validate external data with Zod or the project's established runtime validator when the response is untrusted or drives important UI/state.
4. Create explicit TypeScript types from schemas or service contracts. Avoid widening external data to `any` or leaving unsafe `unknown` values un-narrowed.
5. Design stable query keys near the feature or shared data module. Include only values that define the fetched data.
6. Implement query hooks for reads and mutation hooks for writes. Keep TanStack Query server state out of Zustand unless there is a strong documented reason.
7. Handle loading, error, empty, success, refresh, and retry states in the consuming UI or screen boundary.
8. For mutations, define success behavior: invalidation, cache update, optimistic update, rollback, navigation, and user feedback. Use optimistic updates only when the rollback path is clear.
9. For React Native focus or reconnect behavior, use the project's existing packages when possible and apply it at the narrowest useful scope.
10. Run relevant validation and fix issues introduced by the data change. Complete when services, hooks, schemas, and UI states line up with the contract.

## Standards

Follow `../_shared/react-native-modern-standard.md`.

Data-specific conventions:

- Keep API calls in services or API modules.
- Keep server state in TanStack Query hooks.
- Keep feature-specific hooks close to the feature that owns them.
- Keep shared API clients in `src/services` or the project's established equivalent.
- Use schemas at external boundaries where runtime uncertainty matters.

## Validation checklist

- Existing API client and query patterns were inspected before edits.
- Request logic is not embedded in UI components.
- Query keys are stable and scoped to the data they identify.
- External responses are validated where useful.
- `any` is avoided and unsafe `unknown` is narrowed.
- Loading, error, empty, success, refresh, and retry states are handled where applicable.
- Mutations have explicit invalidation or cache update behavior.
- Optimistic updates include rollback behavior when used.
- React Native focus and online behavior follows the shared standard when touched.
- TypeScript, lint, tests, or app start/build checks were run when configured, or skipped with a concrete reason.

## Avoid

- Do not create duplicate API clients or request helpers.
- Do not store server/API data in Zustand without a strong reason.
- Do not call APIs directly from components when a service or hook boundary would be clearer.
- Do not validate by only asserting TypeScript types over untrusted runtime data.
- Do not use optimistic updates when failure recovery is unclear.
- Do not add global reconnect or focus behavior for a single screen-specific need.
