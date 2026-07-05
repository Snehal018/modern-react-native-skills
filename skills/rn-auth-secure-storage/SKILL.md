---
name: rn-auth-secure-storage
description: Use when adding or refactoring React Native login, logout, token refresh, protected routes, session persistence, SecureStore usage, auth headers, cache clearing, or secret storage.
---

# rn-auth-secure-storage

## Purpose

Implement or review authentication, session persistence, token lifecycle, protected navigation, and secret storage in React Native apps with clear security boundaries.

Read `../_shared/skill-system-constitution.md` before editing this skill or creating adjacent auth skills.

Read `../_shared/react-native-modern-standard.md` before implementing. Use Expo SecureStore or the project's established secure storage abstraction for secrets; do not store secrets in AsyncStorage.

## Use this skill when

- The user asks to add or refactor login, logout, signup, password reset, token refresh, or session restoration.
- The work stores, reads, rotates, or clears access tokens, refresh tokens, API keys, or other secrets.
- The work adds protected routes, auth gates, onboarding gates tied to auth, or role-based navigation.
- The work coordinates auth state with API headers, query cache clearing, or app bootstrap.
- The user asks for an auth or secure-storage review.

## Do not use this skill when

- The work is only ordinary route placement with no auth gating; use `rn-navigation-flow`.
- The work is only form validation without session behavior; use `rn-forms-validation`.
- The work is only API querying without auth lifecycle changes; use `rn-api-data`.
- The app has not been initialized; use `setup-react-native`.

## Preconditions

Before changing auth or secure storage:

- Inspect package metadata for SecureStore, AsyncStorage, auth SDKs, query libraries, and API clients.
- Identify existing auth providers, session stores, token helpers, secure storage wrappers, and bootstrap logic.
- Identify where API clients attach auth headers and handle unauthorized responses.
- Identify navigation gates, protected layouts, role checks, and logout paths.
- Identify cache clearing, persisted query cache, and user-specific local storage behavior.
- Identify existing error handling for expired, revoked, missing, or malformed sessions.

Stop before edits if secrets are currently stored insecurely or auth state is split across competing sources. Explain the risk and propose a focused migration path before adding more auth behavior.

## Process

1. Define the session model: credential input, issued tokens or session object, expiry, refresh behavior, user identity, role claims, and logout semantics.
2. Define the storage boundary. Store secrets only through SecureStore or the project's secure abstraction. Store non-sensitive preferences separately.
3. Create or update an auth service for login, refresh, restore, and logout operations. Keep raw token handling out of UI components.
4. Create or update a small auth state boundary for client-only session status. Do not mirror server profile data into global auth state unless it is required for routing or headers.
5. Wire API auth headers through the central API client. Handle unauthorized responses deliberately, including refresh attempts, forced logout, or user-facing errors.
6. Protect routes at a coherent navigation boundary. Coordinate with `rn-navigation-flow` guidance when the change reshapes routes or layouts.
7. On logout or identity switch, clear user-specific query cache, persisted cache, and sensitive local state.
8. Handle app startup explicitly: loading/restoring session, authenticated, unauthenticated, expired, and error states.
9. Avoid logging secrets, including request headers, token payloads, magic links, and one-time codes.
10. Run relevant validation and fix issues introduced by the auth change. Complete when session restore, authenticated access, unauthorized access, refresh, and logout paths are accounted for.

## Standards

Follow `../_shared/react-native-modern-standard.md`.

Auth-specific conventions:

- Use SecureStore or an established secure storage wrapper for secrets.
- Use AsyncStorage only for non-sensitive preferences or cache.
- Keep auth API calls in services and auth state in a small focused boundary.
- Keep protected navigation gates centralized.
- Clear user-specific server state when the authenticated identity changes.

## Validation checklist

- Existing auth, storage, API client, and navigation gate patterns were inspected before edits.
- Secrets are not stored in AsyncStorage, plain JSON files, logs, or route params.
- Login, restore, refresh or expiry, unauthorized response, and logout paths are explicit.
- API auth headers are attached through a central client or established request helper.
- Protected routes cannot flash private screens before session restore completes.
- Logout or identity switch clears user-specific cache and sensitive client state.
- Auth state does not duplicate server profile data unnecessarily.
- TypeScript, lint, tests, or app start/build checks were run when configured, or skipped with a concrete reason.

## Avoid

- Do not store access tokens, refresh tokens, API keys, or secrets in AsyncStorage.
- Do not pass tokens or secret values through route params.
- Do not log credentials, tokens, auth headers, one-time codes, or magic links.
- Do not scatter auth checks across many screens when a route/layout boundary can own them.
- Do not leave stale query cache available after logout or identity switch.
- Do not add a new auth SDK or storage dependency when the project has a coherent existing solution.
