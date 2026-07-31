# 03 — Content Elements: Table

**Status:** Shipped  
**Subsystem:** StockLive Content Elements (CSS library for Nextlot / Froala)  
**Namespace:** `sl-ce` (StockLive Content Elements)  
**Rich companion:** [03-table.html](03-table.html) (live HTML preview)  
**Siblings:** [01-tabs.md](01-tabs.md) · [02-accordion.md](02-accordion.md) · [04-traits.md](04-traits.md)  
**Reference:** [RapidRailsUI Table](https://rapidrails.cc/docs/table)  
**Repo / demo:** https://github.com/StockLive-Group/sl-ce · https://stocklive-group.github.io/sl-ce/

## Purpose

Third content-element component. Same constraints, tokens, and packaging: HTML + CSS only for Froala authors on Nextlot.

Visual language follows RapidRailsUI Table **bordered + rounded + hoverable** defaults, re-skinned with StockLive brand tokens — not a Stimulus / collection-mode port.

**Percentile / EBV profiles are not tables** — use [04 — Traits](04-traits.md) (`.sl-ce-traits`).

## Context

| Role | Responsibility |
|------|----------------|
| StockLive | Author HTML patterns, build/ship CSS, document snippets, preview/verify |
| Nextlot | Load the CSS bundle globally; Froala authors paste HTML only |
| Content author | Copy-paste markup; keep `data-label` on cells when using cards/stack |

## Constraints (hard)

- HTML and CSS only — no JavaScript
- No inline styles, no `<style>` tags, no `<script>` tags
- No form controls used as CSS state hacks
- CSS is namespaced (BEM under `sl-ce-*`); no styling of bare HTML elements
- Responsive; reasonably accessible given CSS-only limits
- Markup must be simple enough for non-technical Froala authors
- **Browser baseline:** evergreen browsers (semantic table + overflow scroll)

## Decisions

| Decision | Choice |
|----------|--------|
| Markup | Semantic `<table>` inside `.sl-ce-table__scroll` |
| Visual default | Bordered + rounded + hoverable + base density |
| Brand | Shared `--sl-ce-*` tokens |
| Responsive default | Horizontal scroll on `.sl-ce-table__scroll` |
| Cards / stack | Optional modifiers; require `data-label` on cells |
| Sorting / selection / bulk / pagination | **Out of scope** (needs JS) |
| Packaging | Same standalone CSS file as tabs / accordion |

## Architecture

```text
sl-ce (demo / pattern source)
  css/
    tokens.css
    table.css                 # .sl-ce-table*
    content-elements.css      # shippable @import entry
  partials/table/*.html
  → stocklive-content-elements.css (Nextlot)

Nextlot page
  <link rel="stylesheet" href="…/stocklive-content-elements.css">
  Froala HTML body contains .sl-ce-table markup
```

## Author markup contract

```html
<div class="sl-ce-table sl-ce-table--bordered sl-ce-table--rounded sl-ce-table--hoverable sl-ce-table--base">
  <div class="sl-ce-table__header">
    <h3 class="sl-ce-table__title">Sale results</h3>
    <p class="sl-ce-table__description">Optional visible caption.</p>
  </div>
  <div class="sl-ce-table__scroll">
    <table class="sl-ce-table__grid">
      <thead>
        <tr>
          <th scope="col" class="sl-ce-table__th">Lot</th>
          <th scope="col" class="sl-ce-table__th">Animal</th>
          <th scope="col" class="sl-ce-table__th sl-ce-table__th--right">Price</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="sl-ce-table__td" data-label="Lot">#1</td>
          <td class="sl-ce-table__td" data-label="Animal">Sara Park Entice V89</td>
          <td class="sl-ce-table__td sl-ce-table__td--right" data-label="Price">$12,500</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
```

### Author must

- Wrap the table in `.sl-ce-table` and put the grid in `.sl-ce-table__scroll` → `.sl-ce-table__grid`
- Use `scope="col"` on header cells
- Prefer `--bordered --rounded --hoverable --base` for the primary look
- Add `data-label="…"` on each `<td>` when using `--cards` or `--stack`

### Author must not

- Rely on sorting, row selection, or pagination without JS
- Use bare `<table>` without the `sl-ce-table` wrappers (no library styles)
- Put percentile bars in a table — use `.sl-ce-traits` instead

### Variants

| Modifier | Effect |
|----------|--------|
| `--bordered` | Outer + cell borders (primary) |
| `--rounded` / `--square` | Corner shape |
| `--striped` / `--striped-columns` | Alternating washes |
| `--hoverable` | Row hover highlight |
| `--sticky-header` | Sticky thead in scroll box |
| `--cards` / `--stack` | Mobile responsive layouts |
| `--xs` … `--xl` | Cell padding / type size |
| `__th--right` / `__td--right` | Numeric alignment |
| `__badge--success` etc. | Inline status chips in cells |

## Accessibility

- Semantic `<table>`, `<th scope="col">`
- Scroll region is keyboard-scrollable when focused
- Cards/stack expose labels via `data-label` (CSS `::before`) — keep labels short and matching headers

## Out of scope (table v1)

- Sorting, selectable rows, bulk actions, toolbar filters, pagination
- Stimulus `table` controller / Turbo / formatters
- Percentile trait rows (see traits)

## Success criteria

- Paste snippet into Froala with CSS loaded → readable bordered table
- Narrow viewport: horizontal scroll without page-wide overflow
- Cards/stack variants stack with visible labels when `data-label` present
- Namespace holds (`sl-ce` / `--sl-ce-*`)

## Open assumptions

- Froala allows `table`, `thead`, `tbody`, `tr`, `th`, `td`, `scope`, `data-label`, `class`
- If sanitiser strips any of these, Nextlot must allowlist them
