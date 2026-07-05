---
name: rn-quality-review
description: Review React Native or Expo code against modern TypeScript architecture, state, component, hook, API, navigation, form, performance, and readability standards. Use when the user asks for a review, audit, quality pass, refactor recommendations, or targeted fixes for RN code.
---

# rn-quality-review

## Purpose

Review a React Native codebase, feature, or PR-style change against the shared project standards. Review first; apply targeted fixes only when the user asks for fixes or the request clearly includes implementation.

Read `../_shared/skill-system-constitution.md` before editing this skill or creating adjacent review skills.

Read `../_shared/react-native-modern-standard.md` before reviewing.

## Use this skill when

- The user asks for a code review, quality review, audit, cleanup plan, or refactor recommendations.
- The user wants an RN feature checked against architecture and type-safety standards.
- The user asks to apply small targeted fixes after review.

## Do not use this skill when

- The user wants initial setup; use `setup-react-native`.
- The user wants a new feature implemented; use `add-react-native-feature`.
- The code is not React Native, Expo, or shared code used by an RN app.
- The user explicitly asks for a narrow mechanical change without review.

## Preconditions

Before reviewing:

- Identify the review scope: full repo, changed files, a feature folder, or specific files.
- Inspect existing architecture and conventions.
- Check relevant package dependencies and scripts.
- Determine navigation pattern.
- Determine state, data fetching, form, validation, and persistence patterns.
- If reviewing changes, inspect the diff against the requested base.

## Process

1. Inspect the requested scope and enough surrounding code to understand conventions.
2. Review against the shared standard and the project's existing choices.
3. Prioritize behavioral bugs, architecture risks, state/data problems, type-safety issues, and missing user states.
4. Reference actual files, symbols, or patterns. Avoid generic advice.
5. For TanStack Query code, apply the TanStack Query React Native branch in the shared standard before judging online, focus, screen subscription, or devtools choices.
6. Separate critical issues from recommended improvements and nice-to-have improvements.
7. Propose a practical refactor plan when issues span multiple files.
8. If applying fixes, make small targeted patches that preserve behavior.
9. Do not add dependencies unless clearly justified.
10. Run relevant validation after fixes. Complete when findings are file-specific and validation passes, or every skipped command has a concrete reason.

## Standards

Follow `../_shared/react-native-modern-standard.md`.

Expected file/folder conventions:

Review against the feature-based structure in the shared standard, while respecting a project's established equivalent when it is consistent and maintainable.

Review categories:

1. Architecture
2. State management
3. Type safety
4. Components
5. Hooks
6. API/data layer
7. Navigation
8. Forms and validation
9. Performance
10. Error/loading/empty states
11. Duplication
12. Comments/readability

Check for:

- API calls directly inside components.
- Server state stored in Zustand unnecessarily.
- Global store overuse.
- Missing loading, error, empty, and success states.
- `any` usage without strong reason.
- Unsafe `unknown` usage without narrowing.
- Duplicated API logic.
- Duplicated UI patterns that should be shared.
- Components that are too large and mixed with logic.
- Components that are too tiny and fragmented without value.
- Hooks that do too many unrelated things.
- Missing cleanup in effects/subscriptions.
- Incorrect dependency arrays.
- Expensive work during render.
- Large arrays rendered without proper list components.
- Passing large objects through navigation params.
- Forms without validation.
- Missing Zod schemas where external data is untrusted.
- Excessive comments or obvious comments.
- Missing comments where non-obvious decisions need a brief explanation.

## Output format

Produce this structure unless the user requests a different format:

```txt
Summary
Critical issues
Recommended improvements
Nice-to-have improvements
Suggested refactor plan
Files likely affected
```

For each issue, include:

- File or pattern reference.
- Why it matters.
- Concrete recommendation.

If there are no critical issues, say so explicitly.

## Applying fixes

When the user asks for fixes:

- Prefer small, targeted patches.
- Preserve behavior.
- Avoid huge rewrites unless requested.
- Do not introduce new dependencies unless clearly justified.
- Keep code style consistent with the repo.
- Explain tradeoffs briefly.
- Validate after changes.

## Validation checklist

- Review is specific, not generic.
- Issues reference actual files, symbols, or patterns.
- Recommendations are practical.
- Critical findings are separated from preferences.
- Fixes preserve app behavior.
- Type safety is improved or risks are clearly identified.
- Duplication is reduced where fixes were applied.
- Components remain balanced.
- State management is simpler, not more complex.
- Relevant validation commands were run or skipped with a clear reason.

## Avoid

- Do not rewrite everything by default.
- Do not present broad style preferences as critical defects.
- Do not ignore the repo's existing conventions when they are sound.
- Do not recommend Zustand for server state without a strong reason.
- Do not recommend new dependencies when local code or existing packages are sufficient.
- Do not give generic React Native advice without tying it to the reviewed code.
