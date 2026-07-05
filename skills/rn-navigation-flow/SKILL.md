---
name: rn-navigation-flow
description: Use when adding or modifying React Native or Expo routes, tabs, stacks, deep links, auth gates, route params, modal flows, or navigation-driven screen focus behavior. Do not use for feature UI that does not touch navigation.
---

# rn-navigation-flow

## Purpose

Add, change, or review navigation behavior in an existing React Native or Expo app without introducing route drift, duplicate navigation systems, unsafe params, or unclear auth gates.

Read `../_shared/skill-system-constitution.md` before editing this skill or creating adjacent navigation skills.

Read `../_shared/react-native-modern-standard.md` before implementing. Preserve the project's established navigation library and route conventions unless they directly conflict with the user's goal.

## Use this skill when

- The user asks to add, rename, remove, or reorganize app routes.
- The work touches Expo Router routes, React Navigation stacks, tabs, drawers, modals, or nested navigators.
- The work adds or changes route params, deep links, protected routes, auth gates, onboarding gates, or role gates.
- The work needs screen focus behavior tied to route lifecycle.
- The user asks for a navigation review or navigation-specific refactor.

## Do not use this skill when

- The request is broad feature implementation with only ordinary route placement; use `add-react-native-feature` as the owning workflow.
- The app has not been initialized; use `setup-react-native`.
- The change is only data fetching, forms, or auth storage with no navigation behavior.
- The user asks for a full quality review; use `rn-quality-review`.

## Preconditions

Before changing navigation:

- Inspect the project structure and package metadata.
- Identify whether the app uses Expo Router, React Navigation directly, or another established route system.
- Inspect existing route files, navigator definitions, layout files, linking configuration, and route constants.
- Identify how params are typed and parsed.
- Identify auth, onboarding, role, or feature gates that already affect navigation.
- Check for existing screen focus data behavior, such as focus listeners or query refetch on focus.

Stop before edits if the requested route cannot fit the existing navigation model without a larger migration. Explain the conflict and propose the smallest migration path.

## Process

1. Define the navigation outcome: reachable screens, entry points, back behavior, modal behavior, protected access, and deep link behavior where relevant.
2. Map the existing navigation tree before editing. Complete when the route owner, parent layout/navigator, and route naming convention are clear.
3. Place new screens in the project's established route location. For Expo Router, keep route files thin and move substantial UI or logic into feature modules. For React Navigation, update navigator definitions and typed param lists together.
4. Keep route params small. Prefer IDs and primitive flags; avoid passing large objects, server payloads, callbacks, or mutable state through navigation.
5. Type params at the route boundary. Parse and validate params before using them in service calls or forms.
6. Apply auth, onboarding, or role gates at the highest coherent navigation boundary. Avoid duplicating the same gate in many screens.
7. Preserve expected platform behavior for headers, back gestures, tab state, modal dismissal, and hardware back handling.
8. Add or update deep link configuration only when the user needs external entry points. Complete when the path, params, and fallback behavior are explicit.
9. If route focus should refresh data, coordinate with the existing data layer and avoid adding broad app-wide refetch behavior for one screen.
10. Run relevant validation and fix issues introduced by the navigation change. Complete when route imports resolve, typed params compile, and the route can be reached through the expected entry point.

## Standards

Follow `../_shared/react-native-modern-standard.md`.

Navigation-specific conventions:

- Do not introduce a second navigation pattern into an existing app.
- Keep route files and navigator declarations thin.
- Keep route names, file names, and linking paths consistent with the current app.
- Put feature UI and business logic in feature modules when it grows beyond simple route composition.
- Prefer typed route params and route helpers already used by the project.

## Validation checklist

- Existing navigation system was identified before edits.
- New or changed routes live in the established route location.
- Route params are small, typed, and parsed before use.
- Auth, onboarding, or role gates are centralized at a coherent boundary.
- Deep links are updated only when needed and include fallback behavior.
- Header, tab, modal, and back behavior remain coherent.
- No second navigation library or competing route pattern was introduced.
- TypeScript, lint, tests, or app start/build checks were run when configured, or skipped with a concrete reason.

## Avoid

- Do not migrate navigation libraries unless the user explicitly asks for migration.
- Do not pass full records, API responses, functions, or non-serializable values through route params.
- Do not duplicate navigation gates across many screens when a layout or navigator boundary can own them.
- Do not put API calls, form logic, or business rules directly in route files.
- Do not add screen focus refetches that mask stale data problems across the app.
- Do not change deep link paths casually; treat them as external contracts.
