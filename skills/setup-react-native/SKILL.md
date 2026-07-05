---
name: setup-react-native
description: Setup React Native or Expo projects that already exist. Use when the user asks to configure baseline architecture, packages, aliases, providers, folders, linting, API foundations, storage helpers, or standard project conventions. Refuse empty or non-RN target folders; do not create an app from scratch.
---

# setup-react-native

## Purpose

Configure an already-created Expo or React Native boilerplate project into the standard modern architecture. This skill must inspect first, preserve existing code, and make incremental changes.

Read `../_shared/react-native-modern-standard.md` before making decisions. Treat it as the default standard unless the existing project has a clear and consistent alternative.

## Use this skill when

- The user has already created an Expo or React Native project.
- The user asks to normalize architecture, install baseline app dependencies, set up TypeScript aliases, configure linting/formatting, or add core app foundations.
- The work is setup inside an app project, not creation of the app itself.

## Do not use this skill when

- The target folder is empty.
- The target folder does not appear to be an Expo or React Native project.
- The user wants a new app scaffolded from scratch.
- The user only wants a feature added to an already structured app; use `add-react-native-feature`.
- The user asks for a review without setup changes; use `rn-quality-review`.

## Preconditions

Inspect the target folder before edits. Check for:

- `package.json`
- Expo config: `app.json`, `app.config.ts`, or `app.config.js`
- `expo` dependency
- `react-native` dependency
- Existing app folders such as `app/`, `src/`, `ios/`, or `android/`
- Existing router/navigation setup
- Existing TypeScript, lint, formatting, env, and path alias configuration

Stop if the folder is empty or not recognizably RN/Expo. Tell the user to create a boilerplate first, for example:

```bash
npx create-expo-app@latest my-app
```

Then ask them to run the skill inside that project.

## Process

1. Inspect repository shape and package metadata.
2. Confirm the project is Expo or React Native before changing files.
3. Read existing configs: TypeScript, Babel/Metro, Expo config, ESLint, Prettier, router/navigation, env handling, and package scripts.
4. Identify conflicts between existing architecture and the shared standard. If the conflict is significant, summarize it and suggest a migration plan before rewriting structure.
5. Inspect `package.json` and add only missing dependencies. Complete when the dependency plan has no duplicates and preserves existing choices.
6. Configure or normalize `src/` structure using feature-based organization. Complete when user code is preserved and any moves are obvious, low risk, or approved.
7. Configure TypeScript path aliases, usually `@/*`, and update Babel/Metro settings only when required by the project. Complete when imports resolve.
8. Add or complete linting and formatting config if missing. Complete when existing rules are preserved unless they conflict with the standard.
9. Add `.env.example` or equivalent environment documentation without committing real secrets.
10. Add a central API client foundation in `src/services` or the project's established equivalent.
11. Add TanStack Query provider setup at the app root using the project's router/layout pattern. If TanStack Query is added or normalized, follow the TanStack Query React Native branch in the shared standard first.
12. Add a small Zustand store pattern only if useful as a convention; keep it focused and avoid sample global state clutter.
13. Add SecureStore helpers for secrets and AsyncStorage helpers only when non-sensitive persistence is actually needed.
14. Add basic reusable UI primitives only if they serve current setup or clarify conventions.
15. Add an example feature only if it helps demonstrate conventions and does not create unnecessary sample clutter.
16. Run available validation commands and fix issues introduced by setup. Complete when validation passes or every skipped command has a concrete reason.

## Standards

Follow `../_shared/react-native-modern-standard.md`.

Package standards:

Use the baseline stack from the shared standard. Add only packages that the project lacks and the setup actually needs.

Expected file/folder conventions:

Prefer:

```txt
src/
  app/
  components/
  features/
  hooks/
  services/
  store/
  utils/
  constants/
  types/
```

If the project already uses root `app/` for Expo Router, keep it unless moving to `src/app` is explicitly requested and safe.

## Validation checklist

- Project was confirmed to be Expo/RN before changes.
- Empty or non-RN folders were refused.
- Dependencies were added without duplicates.
- Existing user code was preserved.
- TypeScript remains valid.
- Imports work with path aliases.
- App still runs or builds with the repo's normal command.
- Folder structure is clear.
- React Query provider is wired correctly when TanStack Query is added.
- No real secrets were added.
- No unnecessary comments.
- No unjustified `any`.
- No over-fragmented components or sample clutter.

## Avoid

- Do not create the Expo project from scratch.
- Do not scaffold into an empty folder.
- Do not blindly install every baseline dependency.
- Do not overwrite user architecture destructively.
- Do not introduce a second navigation system.
- Do not store API/server data in Zustand without a strong reason.
- Do not add broad sample screens, mock features, or decorative code just to show structure.
- Do not add production services such as Sentry as a forced dependency unless requested.
