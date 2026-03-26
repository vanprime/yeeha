# Product Invariants

These are stable rules for the Jira Extensions browser extension product.

## Product Identity

- This is a browser extension that enhances Jira workflows inside the browser.
- The extension is designed to work with Jira using the user's existing browser session.
- The active product surfaces are `one-shot-analysis`, `migrate-issues`, and their shared seam `jira-issue-importer`.

## Privacy And Security

- No Jira API tokens or credentials are stored by the product.
- Jira access uses browser-session authentication.
- Product data stays in the browser via IndexedDB, `chrome.storage.local`, and local storage where appropriate.
- The product does not add analytics, telemetry, or crash reporting.

## Operational Safety

- Long-running workflows must tolerate sidepanel reloads, navigation changes, and resumptions.
- Persistence changes must preserve upgrade paths for existing browser data.
- High-risk actions should favor additive or preview-first flows before destructive behavior.
- User-facing errors must be surfaced with clear recovery paths instead of silent failure.

## Engineering Boundaries

- Keep product rules in repository files, not hidden chat context.
- Keep feature-specific domain logic owned by the feature unless it becomes a truly shared seam.
- Do not increase coupling to deprecated `data-analysis`.
- Prefer `jira-issue-importer` as the shared Jira fetch/import seam when feature work needs issue-loading behavior.

## Agentic Delivery

- Non-trivial work on active features should use versioned specs with commitable milestones.
- Stable cross-feature rules belong in `docs/`.
- Feature-local `AGENTS.md`, `CURRENT_SPEC.md`, runbooks, and spec files remain the canonical source for feature-specific execution.
