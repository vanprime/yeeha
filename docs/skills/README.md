# Skill Usage

Use the spec skills with the smallest possible prompt.

## Availability

- Canonical custom skill definitions live in `../.codex/skills/`.
- Cloud runs should resolve `$jira-extension-*` prompts from those repository files.
- Imported impeccable design skills should resolve from the same repo-backed location under `$impeccable-*`.
- Local Codex installs can link the same repo-backed skills into `$CODEX_HOME/skills` with `scripts/install-codex-skills.sh`.
- If `$CODEX_HOME/skills` already contains older copied Jira Extension skill folders, use `scripts/install-codex-skills.sh --backup-existing` for the one-time migration.

## Impeccable Design Skills

- The downloaded impeccable package is mirrored into `../.codex/skills/impeccable-*` for repo-local and cloud use.
- Use `$impeccable-teach-impeccable` once per project when design context is missing and capture that context in `.impeccable.md` when appropriate.
- Use `$impeccable-frontend-design` for broad frontend design work, then layer narrower skills such as `$impeccable-arrange`, `$impeccable-polish`, `$impeccable-audit`, `$impeccable-clarify`, `$impeccable-typeset`, or `$impeccable-harden` for focused passes.
- The imported impeccable skills were adapted to Codex conventions, including direct user questioning instead of non-existent AskUserQuestion tooling.

## Refine Requirements

- Use `$jira-extension-refine-requirements` before spec creation when the request is rough, hides assumptions, or needs product-level questioning
- The skill checks the current implementation first, asks only the highest-leverage questions, and produces spec-ready requirements
- For UI-heavy requests, describe desired layout changes in terms of named surfaces, slots, actions, or repeated items when possible

## Skill Orchestration

- `refine-requirements -> create-spec -> implement-spec`
  Artifact chain: refined requirements packet -> versioned spec path -> completed implementation
- `create-refactor-spec -> implement-spec`
  Artifact chain: evidence-backed refactor findings -> versioned refactor spec path -> completed implementation
- `annotate-ui-surfaces -> map-ui-surfaces -> create-spec`
  Artifact chain: anchored UI file -> surface map -> UI-aware versioned spec
- `map-ui-surfaces -> apply-ui-adjustment`
  Artifact chain: surface map -> anchored local UI refinement

When subagents are used, the parent agent should keep ownership of the spec path, `CURRENT_SPEC.md`, milestone completion decisions, and git operations.

## Create A Refactor Spec

- Use `$jira-extension-create-refactor-spec` when the user wants a ruthless code audit for duplication, dead code, redundancies, code smell, or tautologies turned directly into the next versioned spec
- The skill audits current code first, drops weak or speculative findings, and creates a spec only from confirmed refactor debt
- Example: `Use $jira-extension-create-refactor-spec on src/features/migrate-issues/`

## Surface Workflow

- Use `$jira-extension-annotate-ui-surfaces` to add `data-ui-*` anchors to an existing page or component without changing behavior
- Use `$jira-extension-map-ui-surfaces` to generate a concise surface tree for a page or component
- Use `$jira-extension-apply-ui-adjustment` for a small anchored UI refinement; broader changes should route back into the spec flow
- UI-related skill flows should follow the shared styling guardrails from `docs/architecture/agentic-development.md`: Tailwind utilities with `cn(...)`, derived styling booleans outside `cn(...)`, existing `src/components/ui` variants/props before custom classes, and no custom button classes when existing `variant`/`size` fits
- When a spec moves pages or layouts, preserve existing `data-ui-*` anchors as the files move; the surface workflow complements the page/view/component contract and does not replace it
- Example annotate prompt: `Use $jira-extension-annotate-ui-surfaces on src/features/one-shot-analysis/app/pages/RunDetailPage.tsx`
- Example map prompt: `Use $jira-extension-map-ui-surfaces on src/features/migrate-issues/pages/sync-sets/lineage-run-id/SyncSetDetailPage.tsx`
- Example adjustment prompt: `Use $jira-extension-apply-ui-adjustment to move refresh-runs from project-audit-toolbar/primary-actions to project-audit-results/header-actions`

## Implement A Spec

- Preferred: attach or reference the spec file and run the implementation skill on it
- Example: `/skill @src/features/one-shot-analysis/osa-v3.15-spec.md`
- Explicit skill path fallback for cloud-sensitive runs: `.codex/skills/jira-extension-implement-spec/SKILL.md`
- The implementation skill infers the feature from the spec path
- The implementation skill should finish the full active spec in one uninterrupted run unless it hits a real blocker; milestone commits are checkpoints, not default handoff points
- Milestone commits should stage with `git add` first and only run `git commit` after staging completes; do not overlap those commands
- UI implementation work should prefer Tailwind utilities with `cn(...)`, derive complex styling booleans before `cn(...)`, and reuse `src/components/ui` variants/props before custom classes
- The implementation skill enforces the structure contract during feature refactors: route entries under `pages/`, route-local composition under the owning route's `views/`, shared UI under `components/`, and layout wrappers under `layouts/`
- Ordinary feature-local milestones use focused verification; final or wider-risk milestones use the full integration gate
- For UI-heavy specs, add a `UI Surface Contract` section and use `data-ui-*` anchors instead of positional instructions

## Create A Feature Spec

- Use `$jira-extension-create-spec` with requirements for one feature
- The new spec should live under `src/features/<feature>/`
- The created spec should be explicit enough for smaller models and delegated subagents: no vague cleanup verbs, clear keep-outs, precise milestone completion assertions, and explicit parallel-safe slices when applicable

## Create A Product Spec

- Use `$jira-extension-create-spec` when the work spans multiple features or shared UI/shell behavior
- Product specs live under `specs/product/`
- Then implement them the same way: `/skill @specs/product/<name>.md`
