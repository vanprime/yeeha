# Jira Extensions Documentation Index

This is the documentation entrypoint for humans and coding agents working on the active browser
extension product.

## Fast Path

Default read order for normal active-feature work:

- `../AGENTS.md`
- `process/versioned-spec-workflow.md`
- the target feature's `AGENTS.md`
- the target feature's `CURRENT_SPEC.md`
- the active spec file
- the target feature's `implement.md`

Open the other docs in this folder only when the task changes product-wide contracts, architecture,
or feature routing.

## Product-Level Docs

- `process/versioned-spec-workflow.md` — repository-wide workflow for versioned, milestone-based feature work
- `features/index.md` — canonical entrypoints for active features and shared seams
- `product/invariants.md` — open only when the task changes product-wide rules or safety boundaries
- `architecture/agentic-development.md` — open only when the task changes repository-wide architecture or layering
- `architecture/ui-surface-contract.md` — open when the task is driven by layout, structure, targeted interactivity, or post-visual UI refinement

## Reusable Skills

- `$jira-extension-refine-requirements` — verify assumptions, ask focused product questions, and turn rough requirements into spec-ready requirements
- `$jira-extension-create-refactor-spec` — audit a scope for confirmed refactor debt and turn the findings into the next versioned refactor spec
- `$jira-extension-create-spec` — turn requirements into the next versioned feature spec
- `$jira-extension-implement-spec` — implement an existing versioned spec while following repo and feature docs
- `$jira-extension-annotate-ui-surfaces` — add `data-ui-*` anchors to important UI surfaces without changing behavior
- `$jira-extension-map-ui-surfaces` — generate a concise surface tree for discussing layout and structure changes
- `$jira-extension-apply-ui-adjustment` — implement a small anchored UI adjustment or route broader changes back into the spec flow

The committed skill definitions live under `../.codex/skills/`. Use those repository copies as
the source of truth for cloud runs and for any local skill install/sync flow.

## Skill Usage

- `skills/README.md` — very short guide for using the spec skills and choosing feature vs product specs

## Reference Docs

- `README.md` — product-facing install and release usage guide
- `field-mapping-ui-guidelines.md` — migrate-issues validation UI design reference
- `migrate-issues-troubleshooting.md` — migrate-issues troubleshooting reference

## Product Scope Notes

- `one-shot-analysis` and `migrate-issues` are active, high-value features and should follow the current versioned-spec workflow.
- `jira-issue-importer` is a shared technical seam used by both active features.
- multi-feature UI overhauls and shared simplification work should use product specs under `specs/product/`
- `data-analysis` is deprecated. Read it only when maintaining or extracting legacy behavior, not when designing new product work.
