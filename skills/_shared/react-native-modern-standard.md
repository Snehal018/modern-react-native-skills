# React Native Modern Standard

This file defines the shared baseline for React Native skills in this repo. Skills should apply it in context, preserve existing project choices when they are sound, and avoid broad rewrites unless the user explicitly requests migration work.

This standard sits under `skill-system-constitution.md`. Use the constitution for skill-system rules and this file for React Native app-code rules.

## Project Philosophy

- Use TypeScript first.
- Prefer fully type-safe code.
- Avoid `any`. Use it only when a narrower type is not practical and explain why near the boundary.
- Avoid `unknown` unless the value is genuinely unsafe external input, caught errors, or another runtime validation boundary. Narrow it before use.
- Prefer clean, readable code over clever code.
- Favor reusable and modular code, but avoid over-abstraction.
- Avoid duplication as much as reasonably possible.
- Prefer custom hooks when they improve readability, reuse, or separation of concerns.
- Keep state management elegant and simple.
- Use the lightest state tool that fits the problem.

## Component Philosophy

- Keep components lean and practical.
- Do not create extremely tiny component files unnecessarily, especially 10-15 line wrappers that add little value.
- Do not create huge components that mix UI, API calls, business logic, and side effects.
- Make components customizable where useful, but expose only props that are currently needed.
- Avoid speculative props and premature generalization.
- Keep screen components reasonably thin.
- Move repeated behavior, side effects, derived state, and business logic into hooks or services when it improves clarity.

## Comments

- Use comments carefully.
- Do not add heavy comments everywhere.
- Add comments only where intent, tradeoff, workaround, or non-obvious logic needs explanation.
- Keep comments short, usually 1-2 lines per comment block.
- Do not comment obvious code.

## State Management

Use this split:

- Local UI state: `useState` or `useReducer`
- Server/API state: TanStack Query
- Global client state: Zustand
- Forms: React Hook Form with Zod and hookform resolvers
- Secure persisted secrets: Expo SecureStore
- Non-sensitive preferences/cache: AsyncStorage only when needed

Rules:

- Do not store API/server data in Zustand unless there is a strong reason.
- Do not call APIs directly inside components if a service or hook abstraction would be cleaner.
- Keep Zustand stores small and focused.
- Prefer feature-level hooks and services.

## Architecture

Use feature-based organization. Prefer this structure when setting up or extending projects:

```txt
src/
  app/
  components/
  features/
    example-feature/
      components/
      hooks/
      services/
      schemas/
      types/
      utils/
      index.ts
  hooks/
  services/
  store/
  utils/
  constants/
  types/
```

Only create folders that serve current code. Do not add empty folders just for symmetry.

Use absolute imports when configured:

```ts
import { Button } from "@/components/Button";
```

Avoid deep relative import chains:

```ts
import { something } from "../../../../services/something";
```

## API And Data Practice

- Create a central API client.
- Keep API functions in service files.
- Validate external API data with Zod where useful.
- Keep query and mutation hooks close to features.
- Use clear loading, error, empty, and success states.
- Avoid copy-pasted request logic.

## TanStack Query React Native Branch

When setup, feature work, or review touches TanStack Query in a React Native app, open the official React Native guide before choosing the implementation pattern:

https://tanstack.com/query/v5/docs/framework/react/react-native

Apply the guide only where relevant to the app:

- Use React Query normally for server state; it works in React Native.
- Configure online status with `onlineManager` when reconnect refetch behavior matters. Prefer the project's existing network package; for Expo apps, consider `expo-network` if already available or justified.
- Configure app focus with `focusManager` and `AppState` when app foreground/background refetch behavior matters.
- Add screen-focus refetch only for screens whose data should refresh after navigation focus.
- Use `subscribed` with screen focus when an out-of-focus screen should not stay subscribed to query updates.
- Treat React Native query devtools as optional debugging support, not baseline production setup.

## Navigation

- Prefer Expo Router for new Expo projects unless the existing app already uses React Navigation directly.
- Do not introduce a second navigation pattern into an existing app.
- Keep route params small.
- Prefer passing IDs instead of large objects.
- Avoid putting business logic in route files.

## Package Standards

Default stack for modern Expo/React Native work:

- Expo with a development build mindset
- TypeScript
- Expo Router where appropriate
- TanStack Query
- Zustand
- React Hook Form
- Zod
- Hookform resolvers
- Expo SecureStore
- AsyncStorage only when needed
- Reanimated and Gesture Handler when navigation or UI requires them
- ESLint
- Prettier

Sentry or similar observability can be documented as a placeholder or follow-up, but do not force full production setup unless the user requests it.

### Dependency Installation

When adding packages, install the latest compatible version unless the user explicitly requests a fixed version.

Rules:

- Inspect lock files before choosing a package manager.
- Use the package manager implied by the existing lock file.
- If `yarn.lock` exists, use Yarn, including when `package-lock.json` also exists.
- If no `yarn.lock` exists but `pnpm-lock.yaml` exists, use pnpm.
- If no `yarn.lock` or `pnpm-lock.yaml` exists but `package-lock.json` or `npm-shrinkwrap.json` exists, use npm.
- If no lock file exists but `package.json` has `packageManager`, use that package manager.
- If no lock file or `packageManager` field exists, use the manager already used by package scripts or repo docs; otherwise ask before installing.
- Do not mix package managers or generate a second lock file without explicit approval.
- Prefer explicit latest installs such as `yarn add package@latest`, `npm install package@latest`, or `pnpm add package@latest`.
- For Expo-managed native packages, use the project's Expo-compatible install path when required, and do not knowingly install a version incompatible with the current Expo SDK.

## File And Folder Conventions

- Use kebab-case for feature folders.
- Use PascalCase for component files.
- Use camelCase for hooks, utilities, and services.
- Prefix hooks with `use`.
- Prefer `index.ts` exports at feature boundaries, not for hiding unclear module structure.
- Keep shared code in `src/components`, `src/hooks`, `src/services`, `src/store`, `src/utils`, `src/constants`, and `src/types` only when it is genuinely shared.

## Validation Baseline

Before claiming completion, verify the relevant checks for the repo:

- TypeScript check passes, if configured.
- Lint passes, if configured.
- Tests pass, if relevant and configured.
- Imports resolve, including path aliases.
- The app starts or builds when the user's task requires runtime validation.
- No unjustified `any`.
- No unsafe `unknown` without narrowing.
- State is placed in the correct layer.
- API logic is not duplicated or embedded in UI components.
- Components are neither over-fragmented nor overly large.
