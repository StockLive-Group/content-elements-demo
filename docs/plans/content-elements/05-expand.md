# 05 — Content Elements: Expand (show more)

**Status:** Shipped  
**Subsystem:** StockLive Content Elements (CSS library for Nextlot / Froala)  
**Namespace:** `sl-ce` (StockLive Content Elements)  
**Rich companion:** [05-expand.html](05-expand.html) (live HTML preview)  
**Siblings:** [02-accordion.md](02-accordion.md) · [03-table.md](03-table.md) · [04-traits.md](04-traits.md)  
**Repo / demo:** https://github.com/StockLive-Group/sl-ce · https://stocklive-group.github.io/sl-ce/lot-combined.html

## Purpose

Single-item **show more / show less** disclosure for secondary lot content (extra tables, long notes, full trait lists) so the first viewport stays short.

Uses the same native `<details>` / `<summary>` model as accordion, but as a standalone control rather than a multi-item stack.

## Context

| Role | Responsibility |
|------|----------------|
| StockLive | Ship CSS + partial; document variants |
| Nextlot | Load CSS; Froala authors paste HTML |
| Content author | Edit label-more / label-less copy; put secondary HTML in the panel |

## Constraints (hard)

- HTML and CSS only — no JavaScript
- No inline styles, no `<style>` / `<script>` tags
- Namespaced BEM under `sl-ce-*`
- **Browser baseline:** `<details>` / `<summary>` (universal in modern browsers)

## Decisions

| Decision | Choice |
|----------|--------|
| Interaction | Native `<details>` / `<summary>` |
| Labels | Two spans — more / less — swapped via `[open]` CSS |
| Visual default | Text trigger; optional bordered / button / footer |
| Packaging | Same CSS bundle |

## Author markup contract

```html
<details class="sl-ce-expand sl-ce-expand--bordered sl-ce-expand--footer">
  <summary class="sl-ce-expand__trigger">
    <span class="sl-ce-expand__label-more">Show more details</span>
    <span class="sl-ce-expand__label-less">Show less</span>
    <span class="sl-ce-expand__chevron" aria-hidden="true"></span>
  </summary>
  <div class="sl-ce-expand__panel">
    <!-- secondary content -->
  </div>
</details>
```

### Author must

- Include both `__label-more` and `__label-less`
- Include decorative `__chevron` with `aria-hidden="true"`
- Put body content in `__panel`

### Variants

| Modifier | Effect |
|----------|--------|
| (none) | Text link-style trigger |
| `--bordered` | Bordered card shell |
| `--button` | Pill button trigger |
| `--footer` | Trigger sits under the revealed panel |

### Author must not

- Nest form controls inside `<summary>`
- Rely on exclusive behaviour with other expands (each is independent)
- Confuse with accordion multi-item stacks — use accordion for FAQ-style sections

## Accessibility

- Native disclosure keyboard (Enter / Space)
- Browser exposes expanded/collapsed state
- Decorative chevron `aria-hidden="true"`
- `:focus-visible` ring on trigger

## Out of scope

- Smooth height animation beyond browser defaults
- Auto-collapse on scroll / hash
- Exclusive groups

## Success criteria

- Closed: only the “Show more” trigger visible
- Open: panel + “Show less” label
- Composes under tabs / after tables / under traits without JS
