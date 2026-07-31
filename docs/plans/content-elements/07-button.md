# 07 — Content Elements: Button

**Status:** Shipped  
**Subsystem:** StockLive Content Elements (CSS library for Nextlot / Froala)  
**Namespace:** `sl-ce` (StockLive Content Elements)  
**Rich companion:** [07-button.html](07-button.html) (live HTML preview)  
**Reference:** [RapidRailsUI Button](https://rapidrails.cc/docs/button)  
**Repo / demo:** https://github.com/StockLive-Group/sl-ce · https://stocklive-group.github.io/sl-ce/lot-combined.html

## Purpose

Action / navigation controls for lot content — e.g. “View ASBVs”, “Catalogue PDF”. RRUI-aligned variants, StockLive tokens. Prefer `<a>` in Froala (no JS handlers).

## Decisions

| Decision | Choice |
|----------|--------|
| Navigation | `<a class="sl-ce-button …">` |
| External | `target="_blank" rel="noopener noreferrer"` |
| Group | `.sl-ce-button-group` flex wrap |
| Defaults | solid · primary · sm · rounded |

## Author markup contract

```html
<div class="sl-ce-button-group">
  <a
    class="sl-ce-button sl-ce-button--solid sl-ce-button--primary sl-ce-button--sm sl-ce-button--rounded"
    href="https://example.com/asbv"
    target="_blank"
    rel="noopener noreferrer"
  >View ASBVs</a>
  <a
    class="sl-ce-button sl-ce-button--outline sl-ce-button--primary sl-ce-button--sm sl-ce-button--rounded"
    href="https://example.com/catalogue.pdf"
    target="_blank"
    rel="noopener noreferrer"
  >Catalogue PDF</a>
</div>
```

### Variants

| Axis | Classes |
|------|---------|
| Style | `--solid` · `--outline` · `--ghost` · `--soft` · `--link` |
| Color | `--primary` · `--secondary` · `--success` · `--danger` · `--warning` · `--info` |
| Size | `--xs` · `--sm` · `--base` · `--md` · `--lg` · `--xl` |
| Shape | `--rounded` · `--pill` · `--square` |
| Width | `--full` |

### Author must

- Prefer `<a>` for navigation in Froala content
- Always set `rel="noopener noreferrer"` on `target="_blank"`
- Keep labels short and verb-led

### Author must not

- Rely on `<button onclick>` (Froala strips scripts)
- Use buttons for static status labels (use [badge](06-badge.md))

## Accessibility

- Real links / native buttons
- `:focus-visible` ring
- Disabled via `disabled` or `aria-disabled="true"` (opacity + no pointer)

## Success criteria

- Solid / outline / ghost / soft / link render distinctly
- Groups wrap under lot titles without page overflow
- External links open in a new tab safely
