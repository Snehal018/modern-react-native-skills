# Wave 0 Foundation

Wave 0 keeps the React Native skill system usable before more behavior is added. It records how agents should invoke the current skills, whether this repo needs an `agents/openai.yaml`, and the gate every future skill change must pass.

## Ideal Skill Invocation Examples

Use the narrowest skill that matches the user's actual workflow. Do not stack skills just because several mention React Native.

### `setup-react-native`

Use for baseline setup inside an existing Expo or React Native project.

Good user requests:

- "Set up this existing Expo app with the standard architecture."
- "Normalize aliases, providers, linting, and API foundations in this RN repo."
- "Add the baseline React Native project conventions to this initialized app."

Expected agent behavior:

- Inspect the target folder before editing.
- Refuse empty or non-RN folders.
- Preserve existing navigation and architecture when they are coherent.
- Add only foundations that are needed now.

Do not invoke it for:

- Creating a new app from scratch.
- Adding one product feature to an already structured app.
- Reviewing a PR without setup changes.

### `add-react-native-feature`

Use for adding a feature, screen, flow, form, or feature-local module to an existing app.

Good user requests:

- "Add a profile editing screen with validation."
- "Implement the bookings flow in this Expo app."
- "Add an API-backed notifications feature."

Expected agent behavior:

- Inspect existing conventions, routes, shared components, and state patterns.
- Create only the feature folders that contain actual code.
- Keep API logic in services and server state in TanStack Query hooks.
- Keep route files and screen components thin when logic grows.

Do not invoke it for:

- Initial project setup.
- A review-only request.
- A tiny mechanical fix that does not create or reshape feature code.

### `rn-quality-review`

Use for reviewing React Native or Expo code, with optional targeted fixes when the user asks for fixes.

Good user requests:

- "Review this RN feature for architecture and type safety."
- "Audit the current Expo app before release."
- "Do a quality pass and apply small fixes where needed."

Expected agent behavior:

- Identify the review scope before judging code.
- Inspect the surrounding project conventions and relevant diff.
- Lead with bugs, architectural risks, state/data issues, and type-safety problems.
- Apply only small targeted patches when fixes are requested.

Do not invoke it for:

- New feature implementation.
- Initial project setup.
- Generic React advice unrelated to React Native or shared RN app code.

## `agents/openai.yaml` Decision

Do not add `agents/openai.yaml` in Wave 0.

Reasons:

- The current system is only three model-invoked skills plus shared references.
- Skill frontmatter and the constitution already carry the routing rules needed today.
- A provider-specific agent manifest would create another source of truth before there is a concrete runtime requirement.

Reconsider this decision when:

- The skill catalog becomes ambiguous enough to need explicit router metadata.
- Multiple agent runtimes need different invocation behavior that cannot live in skill descriptions.
- The repo adopts a validated manifest schema and CI checks for it.

Until then, keep routing decisions in each skill's `description` and keep system-wide rules in `skills/_shared/skill-system-constitution.md`.

## Wave 0 Completion Criteria

Wave 0 is complete when:

- Ideal invocation examples exist for each current model-invoked skill.
- The `agents/openai.yaml` decision is recorded.
- Future skill additions have a short contribution gate.
- No new app-building behavior or new RN workflow skill is introduced.
