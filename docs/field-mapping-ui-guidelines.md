# Field Mapping UI Guidelines (Draft)

## Scope
This document defines visual hierarchy and token usage for the Field Mapping UI (Validate Step). It is based on the current code structure (`FieldMappingRow`, `CheckboxCard`, sub-config sections) and the screenshot review.

## 1) Current issues (from screenshot + code)
- **Hierarchy collapse:** Row background, sub-config background, and nested controls all read as the same surface. The eye has no clear levels.
- **Over-accented selection:** Blue outline on selected rows is too strong and competes with primary actions.
- **Mixed color system:** Direct `text-slate-*` / `bg-white` / `border-slate-*` usage appears alongside semantic tokens.
- **Sublabels + helper text feel equally weighted:** `text-slate-500` everywhere makes it hard to prioritize label > sublabel > helper.
- **Badges / status chips feel visually detached:** Badge colors are not consistent with the surface language and state priority.

## 2) Principles
- **Semantic tokens only:** Avoid raw palette classes (`text-slate-500`, `bg-white`, etc.). Use semantic tokens: `text-foreground`, `text-muted-foreground`, `text-accent`, `text-info`, `text-success`, `text-warning`, `text-danger`, `bg-card`, `bg-muted`, `border-border`, etc.
- **Clear surface levels:** Page → row → sub-config → control. Each level should feel 1 step deeper than the last.
- **Quiet selection:** Use subtle background + border emphasis for selected rows, not a saturated outline.
- **Readable density:** Labels are primary, sublabels secondary, helper text tertiary. Enforce size + color scale accordingly.

## 3) Semantic token map (recommended)
### Existing tokens (already in config)
- `text-foreground`, `bg-background`, `bg-card`, `bg-muted`, `border-border`, `text-muted-foreground`, `text-primary`, `text-accent`, `text-destructive`, `text-secondary`.

### New tokens to add (Phase 4+)
Add to `tailwind.config.js` (CSS vars in `src/style.css`):
- `info`: `--info`, `--info-foreground`, `--info-muted`, `--info-border`
- `success`: `--success`, `--success-foreground`, `--success-muted`, `--success-border`
- `warning`: `--warning`, `--warning-foreground`, `--warning-muted`, `--warning-border`
- `danger` (alias `destructive` if you prefer): `--danger`, `--danger-foreground`, `--danger-muted`, `--danger-border`

### Mapping table (useful refactor guide)
| Current class | Replace with |
| --- | --- |
| `text-slate-900` | `text-foreground` |
| `text-slate-600` / `text-slate-500` | `text-muted-foreground` |
| `bg-white` | `bg-card` (row) / `bg-background` (page) |
| `bg-slate-50` / `bg-slate-50/50` | `bg-muted/40` or `bg-muted` |
| `border-slate-200` | `border-border` |

## 4) Surface & container rules
### Surfaces
- **Page background:** `bg-background` (already on the section container).
- **Row container:** `bg-card` + `border-border`.
- **Selected row:** `bg-accent/10` + `border-accent/40` (no bright outline).
- **Sub-config panel:** `bg-muted/40` + `border-border` with a subtle left border or inset divider.
- **Inputs/Selects:** Keep `bg-background` or `bg-card` depending on density.

### Borders
- Use only `border-border` for default structure.
- Use `border-accent/40` for selection.
- Use `border-info/40`, `border-warning/40`, etc. for status emphasis.

## 5) Typography hierarchy
- **Primary label:** `text-sm font-medium text-foreground`
- **Sublabel/ID:** `text-[11px] font-mono text-muted-foreground`
- **Helper text:** `text-xs text-muted-foreground`
- **Section titles:** `text-xs font-semibold uppercase tracking-wide text-muted-foreground`

## 6) Interaction states
- **Row hover:** `hover:bg-muted/30` (subtle)
- **Row selected:** `bg-accent/10 border-accent/40`
- **Focus ring:** `focus-visible:ring-accent/40` or `focus-visible:ring-ring/50`
- **Disabled:** `opacity-60 pointer-events-none`

## 7) Badges & status chips
- Use semantic variants only. Example intent:
  - **Success:** `bg-success-muted text-success border-success-border`
  - **Warning:** `bg-warning-muted text-warning border-warning-border`
  - **Danger:** `bg-danger-muted text-danger border-danger-border`
  - **Info:** `bg-info-muted text-info border-info-border`
- Badge size: `text-[10px] px-1.5 py-0.5` with `rounded-full` or `rounded-md`.

## 8) Field Mapping component rules
### Optional mapping rows (CheckboxCard)
- **Card layout:**
  - `bg-card` + `border-border`
  - `data-state=checked`: add `bg-accent/10` and `border-accent/40`
- **Badge placement:** under label, aligned with sublabel.
- **Select area:** in `CheckboxCardAction`, use neutral input background; avoid strong outlines.
- **Chevron:** muted icon; rotate on expand.

### Required / fixed rows (FieldMappingRow)
- **Default row:** `bg-card` + `border-border`
- **Locked/info row:** `bg-muted/40` + dashed border if needed for visual differentiation.
- **Sub-config panel:** use `FieldMappingRowContent` with `bg-muted/40` and left border.

### Sub-config sections
- Use a muted container (`bg-muted/40`) with slightly smaller text.
- Prefer `text-muted-foreground` for instructions and help text.

## 9) Implementation checklist (per file)
- Replace direct `text-slate-*`, `bg-white`, `border-slate-*` with semantic tokens.
- Standardize row backgrounds to `bg-card`.
- Standardize sub-config backgrounds to `bg-muted/40`.
- Replace blue selection outline with accent background + border.

## 10) Example class patterns (reference)
### Row container (selected)
```
className="rounded-md border border-accent/40 bg-accent/10"
```

### Row container (default)
```
className="rounded-md border border-border bg-card"
```

### Sub-config panel
```
className="rounded-b-md border border-border bg-muted/40"
```

### Primary/secondary text
```
Label: text-sm font-medium text-foreground
Sublabel: text-[11px] font-mono text-muted-foreground
Helper: text-xs text-muted-foreground
```

---

## Next steps
- Agree on semantic tokens (`info`, `success`, `warning`, `danger`) and add CSS vars + tailwind config entries.
- Apply these rules to FieldMappingsSection first, then expand to other Validate Step sections.
