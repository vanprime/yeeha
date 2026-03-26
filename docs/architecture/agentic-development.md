# Agentic Development Model

This document captures the global architecture and working model for coding agents in this repository.

## Source Of Truth Hierarchy

1. `AGENTS.md`
2. `docs/index.md`
3. feature-local `AGENTS.md` and `CURRENT_SPEC.md`
4. active feature spec files and implementation runbooks
5. repo-backed custom skill definitions under `.codex/skills/`
6. `.agents/rules/` routing modules

`.agents/rules/` are helpers for prompt assembly. They are not the canonical source of truth when repository files disagree.

Custom skills should be committed under `.codex/skills/` so cloud runs can read the same workflow
definitions that local Codex installs expose from `$CODEX_HOME/skills`.

## Active Feature Strategy

- `one-shot-analysis` and `migrate-issues` are the active feature domains that should use the versioned-spec workflow.
- `jira-issue-importer` is a shared subsystem and should stay reusable and feature-agnostic.
- `data-analysis` is deprecated and should not be the dependency target for new product work.

## Global Layering Rules

Use these as the target model for new or refactored non-trivial features:

- `app/` — feature entrypoints, route wiring, providers
- `ui/` or `views/` — React screens, components, presenters
- `domain/` or `core/` — pure business rules, types, calculations
- `application/` or orchestrator modules — workflows, coordination, state transitions
- `integrations/` or `api/` — Jira APIs, browser APIs, storage adapters
- `testing/` — fixtures, harness adapters, tests

## Pure Logic Rules

Pure domain logic must stay free of:

- React imports
- DOM access
- network fetches
- storage I/O
- side effects
- uncontrolled time calls in core logic when the time should come from context or inputs

## State Ownership Rules

- Durable workflow state belongs in the feature's canonical state boundary and persistence layer.
- Local component state is for transient UI concerns, not the durable truth of complex workflows.
- Type files define the entity contracts. Read them before changing persistent state shapes.

## Shared Seams

- `jira-issue-importer` is the preferred shared seam for Jira issue loading and enrichment.
- shared extension configuration should remain feature-agnostic
- feature-specific mapping or interpretation should stay inside the feature

## Subagent Rules

When delegated/subagent work is in play:

- keep spec authoring and edits parent-owned
- keep `CURRENT_SPEC.md` parent-owned
- keep milestone completion decisions parent-owned
- keep all `git add` and `git commit` operations parent-owned
- allow subagents to do read-heavy discovery, evidence collection, surface mapping, isolated verification, or explicitly assigned disjoint write-scope implementation slices
- do not let multiple agents edit the same authoritative file or share a write scope unless the spec explicitly serializes that handoff

## UI Styling Guardrails

When implementing or refining UI:

- prefer Tailwind utilities composed inline with `cn(...)`
- do not extract class-string constants for ordinary component styling
- when styling logic is non-trivial, derive booleans or small semantic flags before the `cn(...)` call instead of nesting heavy logic inside class composition
- prefer existing variants and props from `src/components/ui/*` before adding custom classes or wrappers
- do not add custom button classes when `src/components/ui/button.tsx` already exposes a fitting `variant` and `size`

## Verification Defaults

- `pnpm exec tsc --noEmit`
- `pnpm test:run`
- `pnpm build`
- `pnpm test:run:full` only when the task explicitly requires full-repository or legacy coverage
