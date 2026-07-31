# 06 — Content Elements: Badge

**Status:** Shipped  
**Subsystem:** StockLive Content Elements (CSS library for Nextlot / Froala)  
**Namespace:** `sl-ce` (StockLive Content Elements)  
**Rich companion:** [06-badge.html](06-badge.html) (live HTML preview)  
**Reference:** [RapidRailsUI Badge](https://rapidrails.cc/docs/badge)  
**Repo / demo:** https://github.com/StockLive-Group/sl-ce · https://stocklive-group.github.io/sl-ce/lot-combined.html

## Purpose

Compact status / metadata chips for lot accreditations (LPA, MSA, Organic, Vendor bred) and inline table status. RRUI-aligned variants, StockLive tokens.

## Decisions

| Decision | Choice |
|----------|--------|
| Static label | `<span class="sl-ce-badge …">` |
| Link badge | `<a class="sl-ce-badge …" target="_blank" rel="noopener noreferrer">` |
| Group | `.sl-ce-badge-group` flex wrap (sit under `lot__title`) |
| Defaults | solid · primary · xs · pill |

## Author markup contract

```html
<div class="sl-ce-badge-group" aria-label="Lot accreditations">
  <a
    class="sl-ce-badge sl-ce-badge--soft sl-ce-badge--primary sl-ce-badge--sm sl-ce-badge--pill"
    href="https://example.com"
    target="_blank"
    rel="noopener noreferrer"
  >Vendor bred</a>
  <span class="sl-ce-badge sl-ce-badge--outline sl-ce-badge--success sl-ce-badge--sm sl-ce-badge--pill">Organic</span>
</div>
```

### Variants

| Axis | Classes |
|------|---------|
| Style | `--solid` · `--outline` · `--ghost` · `--soft` · `--link` |
| Color | `--primary` · `--secondary` · `--success` · `--danger` · `--warning` · `--info` |
| Size | `--xs` · `--sm` · `--base` · `--md` · `--lg` |
| Shape | `--pill` · `--rounded` · `--square` |

### Author must

- Use `rel="noopener noreferrer"` on external link badges
- Prefer soft/outline/pill for lot accreditation rows
- Keep label text short (one or two words)

### Author must not

- Use badges as primary CTAs (use [button](07-button.md))
- Nest interactive controls inside a badge

## Accessibility

- Link badges are real anchors (keyboard + Enter)
- `:focus-visible` ring on links
- Group may use `aria-label` when it is a labelled set

## Success criteria

- Static and link badges share chrome
- Badge group wraps cleanly under lot titles
- Composes with traits rank chips (`.sl-ce-traits__rank`)
