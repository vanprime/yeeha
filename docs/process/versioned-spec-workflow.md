# Versioned Spec Workflow

This is the repository-wide workflow for non-trivial work on active features.

## Features Using This Workflow

- `src/features/one-shot-analysis/`
- `src/features/migrate-issues/`

Each of these features should expose:

- `AGENTS.md`
- `CURRENT_SPEC.md`
- an active or latest versioned spec file
- a stable implementation runbook
- a historical checkpoint log

## Spec Placement

- Feature spec: place it under `src/features/<feature>/`
- Product spec: place it under `specs/product/` when the work spans multiple features, shared shell/navigation, or repository-wide simplification

Use product specs for work such as:

- shared UI overhauls
- cross-feature navigation or sidepanel shell changes
- shared subsystem refactors that affect multiple active features
- repository-wide simplifications with milestone ownership across features

## When A Versioned Spec Is Required

If the incoming requirements are ambiguous, solution-biased, or rely on assumptions about current
behavior, refine them first before writing the spec. Use `$jira-extension-refine-requirements` for
that step.

If the request is specifically to audit the current code for duplication, dead code, redundancies,
tautologies, or similar refactor debt and then turn the confirmed findings into a spec, use
`$jira-extension-create-refactor-spec`.

Create or activate a versioned spec before coding when the change affects:

- user-facing flows
- persisted data contracts
- orchestration boundaries
- navigation structure
- analytical or migration semantics
- cross-feature integrations

## Small Change Exception

A new spec may be skipped only when the change is clearly:

- a narrow bug fix
- a test-only improvement
- a documentation cleanup
- an implementation detail that does not alter product or architectural contracts

For UI work, a new spec may also be skipped only when the change is all of the following:

- layout-only or narrowly presentational
- scoped to one active feature
- inside an existing, already-understood surface contract
- not a change to navigation, persistence, schema, orchestration, or cross-feature behavior

If there is doubt, write the next spec first.

## Prompt Economy

Default read path for active implementation work:

- `AGENTS.md`
- the target feature's `AGENTS.md`
- the target feature's `CURRENT_SPEC.md`
- the active spec
- the target feature's `implement.md`
- `docs/architecture/ui-surface-contract.md` only when the task is driven by layout, structure, or targeted UI refinement

Read global product docs only when the task changes repository-wide contracts.
Read historical ledgers such as `documentation.md`, `FEATURE_SPEC.md`, and `plans.md` only when
resuming interrupted work, reconciling shipped state, or clarifying a contract that the active
spec does not already define.

If the spec path is under `src/features/<feature>/`, infer the feature from the path.
If the spec path is under `specs/product/`, treat it as a product spec and load only the docs for
the feature or shared seam owned by the current milestone.

## Subagent Rules

When work is delegated to subagents:

- the parent agent owns the spec file, `CURRENT_SPEC.md`, milestone completion decisions, and all git operations
- subagents may collect evidence, map surfaces, run isolated verification, or implement explicitly assigned disjoint write scopes
- if a milestone is safe to split, the spec should say so with explicit parallel-safe slices and keep-out boundaries
- if a milestone is not safe to split, the spec should say so instead of leaving concurrency implicit

## UI-Driven Work

Use the product `data-ui-*` surface contract for UI-heavy work that depends on named regions,
placement zones, repeated items, or action anchors.

Read `docs/architecture/ui-surface-contract.md` when:

- the request is about moving or regrouping existing UI
- the request is easier to express in terms of named regions than code structure
- a spec needs stable UI anchors for layout or targeted interaction work

For UI-heavy specs, add:

- `## UI Surface Contract` for named surfaces, slots, actions, and repeated items
- `## UI Adjustment Notes` for explicit moves, regrouping, or placement changes

Example instructions should target anchors such as:

- move `refresh-runs` from `project-audit-toolbar/primary-actions` to `project-audit-results/header-actions`
- split `filters` into `date-filters` and `severity-filters`

Do not use document-wide `id` as the default business anchor. Keep `id` for accessibility and
true unique anchors.

## Required Shape

Every active spec should define:

- status, baseline, dependency, and focus
- motivation and user stories
- design decisions
- non-negotiables
- implementation notes covering canonical terms, invariants, or keep-out seams when they matter
- ordered milestones (`M0`, `M1`, `M2`, ...)
- milestone verification
- likely files or surfaces affected by each milestone
- explicit out-of-scope boundaries
- documentation sync expectations
- commit boundary or commit-message guidance for each milestone

Each milestone should also define:

- goal
- entry criteria or dependencies when they are not obvious
- in-scope files or surfaces
- do-not-touch boundaries
- ordered tasks
- test touchpoints
- completion assertions (`Done when`)
- allowed parallel work or an explicit `none`

For UI-heavy specs, also define:

- a `UI Surface Contract` when named layout surfaces or action anchors are required for precise communication
- `UI Adjustment Notes` when the user needs explicit surface-to-surface movement or regrouping instructions

Product specs should also define:

- scope: `Product`
- primary owner
- affected features
- shared seams
- milestone owners when milestones shift between features or shared layers

## Verification Levels

Use the smallest gate that still proves the milestone safely:

- Focused milestone gate:
  - feature-scoped `pnpm typecheck:*`
  - matching `pnpm test:run:*`
- Full integration gate:
  - `pnpm typecheck`
  - `pnpm test:run`
  - `pnpm build`

Use the full integration gate for:

- final milestones
- cross-feature milestones
- shared-shell or routing changes
- persistence/schema changes with wider blast radius
- any milestone where the spec explicitly requires it

## Milestone Authoring Heuristics

- Prefer milestones that cluster work by seam, surface, or file family.
- Prefer milestones that let a file or surface land once instead of being reopened in multiple later milestones.
- Avoid cross-cutting "touch everything a little" milestones unless contract work truly has to land first.
- A good milestone should be independently reviewable, verifiable, and committable.
- Write milestone blocks so a smaller model can execute them without rediscovering hidden keep-outs, success criteria, or file ownership.

## Execution Rules

- implement milestones in order
- keep milestones commitable
- keep the default execution target as the full active spec, not a single milestone
- run the focused milestone gate for ordinary feature-local milestones
- run the full integration gate for final or cross-feature milestones
- sync the feature's checkpoint log after each milestone
- keep authoritative state single-owner: the parent agent owns the spec file, `CURRENT_SPEC.md`, milestone status, and git operations
- create a Git commit after each milestone passes the QA gate and required doc sync
- run `git add` and `git commit` sequentially when closing a milestone; do not overlap them
- do not start the next milestone until the current milestone is committed
- after that commit, continue directly to the next milestone in the same run unless the user asked to stop or a real blocker requires interruption
- update the feature's `CURRENT_SPEC.md` when active status changes

## Verification Defaults

- Focused:
  - `pnpm typecheck:osa`
  - `pnpm typecheck:migrate-issues`
  - `pnpm typecheck:jira-issue-importer`
  - matching `pnpm test:run:*`
- Integration:
  - `pnpm typecheck`
  - `pnpm test:run`
  - `pnpm build`
- `pnpm test:run:full` when a spec explicitly requires full-suite coverage
