# UI Surface Contract

Use `data-ui-*` attributes as lightweight communication anchors for layout, nesting, and targeted
UI refinements in the active browser extension product.

These anchors are for humans and coding agents. They are not a replacement for accessibility IDs,
testing strategy, or semantic HTML.

## Attribute Set

- `data-ui-surface` — a major user-facing region with a cohesive business or workflow purpose
- `data-ui-slot` — a named placement zone inside a surface
- `data-ui-action` — a user-triggered control owned by a surface
- `data-ui-item` — the repeated row/card/item type inside a repeated collection
- `data-ui-key` — a stable per-item identity when a repeated item needs precise targeting

## Naming Rules

- Use kebab-case.
- Name anchors by business or workflow purpose, not by visual styling.
- Good examples:
  - `project-audit-toolbar`
  - `header-actions`
  - `refresh-runs`
  - `pairing-row`
- Avoid names like:
  - `left-column`
  - `blue-button`
  - `second-panel`

Use positional names only when layout is the durable intent, for example `header-actions` or
`sidebar-summary`.

## What To Annotate

Annotate only meaningful UI seams:

- page roots
- section roots with clear workflow meaning
- headers and toolbars
- sidebars and filter rails
- results panels and summary strips
- dialogs, drawers, and popovers when they own meaningful workflow actions
- repeated row/card roots when the user may want to target one repeated item shape

## What Not To Annotate

Do not annotate every wrapper or low-level primitive.

Avoid adding `data-ui-*` to:

- purely visual spacer wrappers
- every `div` in a layout stack
- every text node container
- shared UI primitives whose local structure is not a communication target

The goal is a stable surface tree, not DOM exhaustiveness.

## Relationship To Existing Attributes

- Keep `id` for accessibility wiring, labels, and true document-unique anchors.
- Do not replace existing `id`/`htmlFor` pairs with `data-ui-*`.
- Do not overload shared `data-slot` usage from shared UI primitives.
- `data-ui-*` is the product-level communication namespace for layout and structure.

## React And TypeScript Guidance

- Native JSX elements can receive `data-ui-*` attributes without extra helper code.
- Prefer placing `data-ui-*` on native elements or on shared components that clearly forward props.
- If a custom component does not forward unknown props, annotate the nearest native wrapper instead
  of widening types immediately.
- Add shared typing only if repeated prop forwarding becomes a real maintenance burden.

Example optional shared type if it later becomes useful:

```ts
type UiSurfaceProps = {
  "data-ui-surface"?: string
  "data-ui-slot"?: string
  "data-ui-action"?: string
  "data-ui-item"?: string
  "data-ui-key"?: string
}
```

Do not introduce a typing layer before there is a concrete need.

## Repeated Items

For repeated content, anchor the repeated shape and its stable key separately.

```tsx
<tr data-ui-item="pairing-row" data-ui-key={pairing.targetKey}>
  <td data-ui-slot="target-summary">...</td>
  <td data-ui-slot="review-selection">...</td>
  <td>
    <Button data-ui-action="open-pairing-details">Details</Button>
  </td>
</tr>
```

Do not use document-wide `id` values for repeated rows or repeated actions.

## Spec Integration

When UI layout, nesting, or targeted interaction changes are part of the scope, add a
`UI Surface Contract` section to the spec.

Suggested shape:

```md
## UI Surface Contract

Surface: `project-audit-toolbar`
- slot: `title-area`
- slot: `primary-actions`
- slot: `filters`
- action: `refresh-runs`

Surface: `project-audit-results`
- slot: `header-actions`
- slot: `results-table`

## UI Adjustment Notes

- Move `refresh-runs` from `project-audit-toolbar/primary-actions` to `project-audit-results/header-actions`
- Split `filters` into `date-filters` and `severity-filters`
```

Use surface and slot names instead of positional instructions like "move the second element to the
right."

## Small UI Adjustment Path

A new spec is not required when the change is all of the following:

- layout-only or narrowly presentational
- scoped to one active feature
- inside an existing, already-understood surface contract
- does not change navigation structure
- does not change persistence or schema
- does not change orchestration or domain behavior in a material way

If the adjustment changes workflow meaning, interactivity, data flow, or cross-feature behavior,
amend the active spec or create the next spec.

## Suggested Workflow

1. Annotate missing `data-ui-*` anchors on the page or component.
2. Generate a surface map from the anchored code.
3. Write UI-heavy requirements or spec deltas against named surfaces, slots, actions, and repeated items.
4. Implement the change using those anchors.

## Styling Guardrails

These structure anchors do not replace the shared UI styling rules:

- prefer Tailwind utilities and `cn(...)` for component styling work
- avoid class-string constants when direct utility composition is sufficient
- derive complex styling booleans before `cn(...)`
- prefer existing `src/components/ui` variants/props over custom classes
- avoid custom button classes when an existing `Button` `variant`/`size` already fits
