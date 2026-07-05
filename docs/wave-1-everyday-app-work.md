# Wave 1 Everyday App Work

Wave 1 adds the high-return React Native workflows that appear in most product feature work. These skills are model-invoked, distinct from the broad `add-react-native-feature` workflow, and point back to the shared skill constitution and React Native standard.

## Delivered Skills

### `rn-navigation-flow`

Use for adding or changing routes, tabs, stacks, deep links, auth gates, route params, modal flows, and navigation-driven screen focus behavior.

Boundary:

- Owns navigation-specific design and changes.
- Does not own broad feature implementation unless navigation is the main risk.
- Coordinates with auth and data workflows when protected routes or focus refetch behavior are involved.

### `rn-api-data`

Use for API clients, service functions, TanStack Query hooks, mutations, cache keys, optimistic updates, runtime response validation, and React Native online/focus data behavior.

Boundary:

- Owns server-state and external-data boundaries.
- Does not own token/session lifecycle; `rn-auth-secure-storage` owns that.
- Does not own form field behavior unless the form submit/data contract is the main risk.

### `rn-forms-validation`

Use for forms, edit/create screens, React Hook Form controllers, Zod schemas, field errors, submit states, keyboard behavior, and form-driven mutations.

Boundary:

- Owns form state, validation, submit behavior, and keyboard/input ergonomics.
- Does not own API cache policy beyond coordinating with mutation hooks.
- Does not own auth session persistence even when the form is a login form.

### `rn-auth-secure-storage`

Use for login, logout, token refresh, session persistence, protected routes, SecureStore usage, auth headers, cache clearing, and secret storage.

Boundary:

- Owns session lifecycle and secure secret handling.
- Coordinates with navigation when auth gates reshape routes.
- Coordinates with data when auth headers, unauthorized responses, or query cache clearing are touched.

## Invocation Guidance

Use `add-react-native-feature` as the owning skill for broad product features that include several concerns. Use a Wave 1 skill directly when that concern is the main risk or the user asks for that workflow specifically.

Examples:

- "Add a settings form that saves notification preferences" should usually use `add-react-native-feature`, with `rn-forms-validation` and `rn-api-data` guidance applied where relevant.
- "Fix our protected route redirect loop" should use `rn-navigation-flow` and likely `rn-auth-secure-storage`.
- "Add optimistic updates to the comments mutation" should use `rn-api-data`.
- "Move refresh tokens out of AsyncStorage" should use `rn-auth-secure-storage`.

## Wave 1 Completion Criteria

Wave 1 is complete when:

- `rn-navigation-flow` exists and covers route, params, deep link, gate, and focus behavior.
- `rn-api-data` exists and covers services, query hooks, mutations, validation, cache, online, and focus behavior.
- `rn-forms-validation` exists and covers forms, schemas, errors, submit state, mutation coordination, keyboard, and accessibility behavior.
- `rn-auth-secure-storage` exists and covers session lifecycle, secure storage, API headers, protected routes, cache clearing, and logout behavior.
- Each skill follows the constitution's required shape.
- Each skill references shared standards rather than duplicating them.
- README and the roadmap reflect Wave 1 as complete.
