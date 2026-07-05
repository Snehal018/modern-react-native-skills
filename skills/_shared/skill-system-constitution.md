# Skill System Constitution

This constitution governs the React Native skill system in this repository. It is the source of truth for why the system exists, how new skills should be shaped, and what every skill must preserve across sessions.

## Mission

The skill system exists to make agent-driven React Native development predictable. A future agent should choose the right workflow, inspect before changing code, preserve existing project intent, and produce focused, type-safe work that can be reviewed.

Skills should improve process reliability. They should not become package notes, broad motivation, or a dumping ground for preferences.

## Scope

This constitution applies to:

- Every skill under `skills/`
- Every shared reference under `skills/_shared/`
- Every future roadmap item that becomes an actual skill

It does not replace task-specific standards such as `react-native-modern-standard.md`. Those standards sit under this constitution and provide domain rules for React Native app code.

## Core Principles

### Predictability Over Coverage

Add a skill only when it gives agents a repeatable process they would otherwise perform inconsistently. Prefer a small number of high-signal skills over a broad catalog that future agents cannot reliably choose from.

### Inspect Before Acting

Every implementation or review skill must begin by inspecting the target project, feature, diff, or files. Skills should make agents preserve existing conventions unless those conventions conflict with the user's goal or the shared standards.

### Single Source Of Truth

Shared rules belong in shared references. Individual skills should point to those references and add only the process that is unique to the skill.

Do not duplicate the same standard across multiple `SKILL.md` files. If a rule needs to be changed in more than one place, it probably belongs in `skills/_shared/`.

### Right-Sized Granularity

Create a new skill when the work has a distinct trigger and a distinct sequence. Do not create a skill for every library, package, or styling preference.

Use shared references for library-specific standards unless the library changes the agent's workflow enough to justify a skill.

### Model Invocation Is A Cost

Use model-invoked skills for core workflows future agents must discover automatically. Use user-invoked or router-mediated skills for niche workflows.

When the skill catalog grows large enough that selection becomes ambiguous, create or update a router skill instead of relying on every agent to remember the catalog.

### Operational Writing

Skills are instructions for agents, not essays. Use direct, checkable language. Prefer ordered steps, preconditions, validation criteria, and avoid lists over background explanation.

## Required Skill Shape

Every model-invoked skill must have:

- YAML frontmatter with only `name` and `description`, unless the repo later adopts a broader manifest convention.
- A description that names the leading workflow and lists distinct trigger branches without synonym padding.
- A `Purpose` section.
- A `Use this skill when` section.
- A `Do not use this skill when` section.
- A `Preconditions` section that forces inspection before action.
- A `Process` section with ordered steps and checkable completion criteria where risk is meaningful.
- A `Standards` section that points to shared references instead of duplicating them.
- Expected file/folder conventions when the skill creates or reviews files.
- A validation checklist.
- An `Avoid` section.

User-invoked reference skills may be lighter, but they must still be clear about how they should be used.

## Description Rules

Descriptions are always loaded into context for model-invoked skills, so they must be short and deliberate.

- Front-load the workflow name or leading word.
- Include distinct triggers only.
- Do not repeat the body.
- Do not list every package mentioned by the skill unless each package is a real trigger.
- Include "do not use" constraints only when they prevent dangerous or common misfires.

## Shared Reference Rules

Use `skills/_shared/` for standards that multiple skills need.

Shared references should:

- Be named by the thing they govern.
- Be linked from each skill that must apply them.
- Keep related rules together under one heading.
- Avoid task-specific process unless the process is truly shared.

The React Native app-code standard lives at `skills/_shared/react-native-modern-standard.md`.

## Validation Rules

Before a new or edited skill is considered done:

- Confirm the skill has a distinct purpose and does not duplicate another skill.
- Confirm shared rules were referenced rather than copied.
- Confirm the process begins with inspection.
- Confirm risky steps have completion criteria.
- Confirm validation commands or skip criteria are named.
- Confirm the skill tells agents what to avoid.
- Confirm there are no placeholders, vague motivational sections, or unused resource folders.
- Validate YAML frontmatter with the available skill validation tool or a local parser.

## Contribution Gate

Every future skill change must pass this short gate before it lands:

1. Invocation: the skill has a distinct trigger, a clear non-use case, and does not overlap an existing skill without explaining the boundary.
2. Inspection: the process starts by inspecting the user's project, files, diff, or runtime state before edits or judgment.
3. Standards: shared rules are linked from `skills/_shared/` instead of copied into the skill body.
4. Scope: the change adds only behavior needed for this workflow and does not introduce sample clutter, package notes, or broad rewrites.
5. Validation: the skill names concrete validation commands or skip criteria, and frontmatter parses as YAML.

If a proposed skill fails the invocation gate, make it a shared reference, expand an existing skill, or defer it until real project demand proves the workflow is distinct.

## Anti-Patterns

Avoid:

- Skill sprawl: many tiny skills with weak triggers.
- Package-note skills: documentation summaries without a distinct workflow.
- Duplicate standards copied into every skill.
- Vague directives such as "be careful" without concrete behavior.
- Skills that encourage broad rewrites by default.
- Sample clutter that future agents will cargo-cult into apps.
- Validation checklists that cannot be verified.
- Adding dependencies to app projects just because a skill mentions them.

## Amendment Rules

Update this constitution when the skill system's operating rules change. Do not bypass it by adding conflicting rules to individual skills.

When a new principle is specific to React Native app code, add it to `react-native-modern-standard.md` instead. When it governs how skills are created, invoked, split, reviewed, or maintained, add it here.
