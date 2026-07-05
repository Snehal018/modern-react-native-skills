---
name: add-react-native-feature
description: Add React Native or Expo features to an existing app. Use when the user asks to implement screens, UI flows, forms, API-backed feature modules, feature-local hooks, services, schemas, types, or navigation entries. Do not use for initial project setup.
---

# add-react-native-feature

## Purpose

Add a feature module to an existing React Native app in a consistent, type-safe, feature-based structure. This skill extends an app; it does not initialize one.

Read `../_shared/skill-system-constitution.md` before editing this skill or creating adjacent feature skills.

Read `../_shared/react-native-modern-standard.md` before implementing. Reuse existing project conventions when they are established and compatible.

## Use this skill when

- The user asks to add a feature, screen, flow, form, integration, or feature-specific UI.
- The app already exists and has enough structure to extend.
- The feature should include local components, hooks, services, schemas, types, utilities, routes, or state.

## Do not use this skill when

- The project has not been initialized yet.
- The user wants baseline architecture setup; use `setup-react-native`.
- The user wants only a review; use `rn-quality-review`.
- The change is a tiny one-line fix that does not create or reshape feature code.

## Preconditions

Before adding code:

- Inspect the existing folder structure.
- Identify naming conventions, import style, test style, component style, and state patterns.
- Check whether Expo Router or React Navigation is used.
- Check existing shared components, hooks, services, schemas, utilities, and stores.
- Check whether TanStack Query, Zustand, React Hook Form, Zod, SecureStore, or AsyncStorage are already configured.
- Confirm where screens/routes belong.

Do not introduce a second navigation pattern or duplicate existing app foundations.

## Process

1. Understand the requested feature behavior and boundaries.
2. Inspect existing conventions and reusable code.
3. Decide the feature folder name in kebab-case.
4. Create only the folders needed by the feature:

```txt
src/features/<feature-name>/
  components/
  hooks/
  services/
  schemas/
  types/
  utils/
  index.ts
```

5. Keep feature-only UI inside `src/features/<feature-name>/components`.
6. Promote components to `src/components` only when they are genuinely reusable across features.
7. Keep API functions in feature service files or existing shared service modules.
8. Keep query and mutation hooks in feature hooks. If the feature uses TanStack Query, follow the TanStack Query React Native branch in the shared standard before writing hooks.
9. Use TanStack Query for server/API state.
10. Use Zustand only for true global client state.
11. Use React Hook Form with Zod for forms.
12. Validate external data with Zod where useful, especially untrusted API responses and form inputs.
13. Keep route files and screen components thin. Move repeated behavior, side effects, derived state, and business logic into hooks/services when it improves clarity.
14. Export the feature's intended public surface from `index.ts`.
15. Run relevant validation and fix issues introduced by the feature. Complete when validation passes or every skipped command has a concrete reason.

## Standards

Follow `../_shared/react-native-modern-standard.md`.

Expected file/folder conventions:

Use the feature structure from the shared standard. Create only folders that contain code for the requested feature.

Component guidance:

- Do not break UI into tiny files unless there is reuse or a clarity benefit.
- Do not keep everything in one large screen file.
- Create components when they isolate meaningful UI sections or repeated patterns.
- Expose only props that are needed now.
- Avoid generic prop APIs that are not used.

State guidance:

- Local UI state stays local with `useState` or `useReducer`.
- API state belongs in TanStack Query.
- App-wide client state belongs in small focused Zustand stores.
- Form state belongs in React Hook Form.
- Persisted secrets belong in SecureStore.
- Non-sensitive preferences/cache may use AsyncStorage when needed.

## Validation checklist

- Feature follows existing app conventions.
- Only necessary feature folders were created.
- No duplicate logic, services, hooks, schemas, utilities, or components were introduced.
- State is placed in the right layer.
- API calls are not inside UI components.
- Query/mutation hooks are close to the feature.
- Forms use schema validation where relevant.
- Types are explicit and safe.
- No unjustified `any`.
- Unsafe `unknown` values are narrowed before use.
- Comments are minimal and useful.
- Files are not too skinny or too bulky.
- Route params remain small and pass IDs instead of large objects.
- Loading, error, empty, and success states are handled where applicable.

## Avoid

- Do not initialize or restructure the whole project.
- Do not create empty folders for symmetry.
- Do not add speculative abstractions, props, hooks, or generic services.
- Do not duplicate existing shared components or request helpers.
- Do not introduce a second navigation system.
- Do not put API calls, business logic, or heavy side effects directly in UI components.
- Do not store server data in Zustand without a strong reason.
- Do not add excessive comments or comments that restate obvious code.
