# 01 — Content Elements: Tabs

**Status:** Shipped (Froala-safe revision)  
**Subsystem:** StockLive Content Elements (CSS library for Nextlot / Froala)  
**Namespace:** `sl-ce` (StockLive Content Elements)  
**Rich companion:** [01-tabs.html](01-tabs.html) (live HTML preview)  
**Siblings:** [02-accordion.md](02-accordion.md) · [03-table.md](03-table.md) · [04-traits.md](04-traits.md) · [05-expand.md](05-expand.md) · [06-badge.md](06-badge.md) · [07-button.md](07-button.md)  
**Repo / demo:** https://github.com/StockLive-Group/sl-ce · https://stocklive-group.github.io/sl-ce/lot-tabs.html  
**Constraints note:** [froala-nextlot-constraints.md](../../content-elements/froala-nextlot-constraints.md)

## Purpose

StockLive owns a reusable **HTML + CSS-only** component library for content authors. Nextlot loads the CSS globally and authors paste HTML snippets into Froala (which strips JavaScript, inline `<style>`, and executable code).

Tabs are the first component. The same namespace, tokens, and packaging model support siblings (accordion, table, traits, expand, badge, button).

## Context

| Role | Responsibility |
|------|----------------|
| StockLive | Author HTML patterns, build/ship CSS, document snippets, preview/verify |
| Nextlot | Load the CSS bundle globally; Froala authors paste HTML only |
| Content author | Copy-paste markup; unique `name=` per group; put `open` on the default item |

## Constraints (hard)

- HTML and CSS only — no JavaScript
- No inline styles, no `<style>` tags, no `<script>` tags
- **No form-control state hacks** (`input` / `label` are stripped by Nextlot Froala)
- **No `:target` / `id` tabs** (`id`, `<nav>`, `<section>` are stripped or unwrapped)
- CSS is namespaced (BEM under `sl-ce-*`); no styling of bare HTML elements outside components
- Markup must survive Nextlot’s Froala allowlist (`div`, `details`, `summary`, …)
- **Browser baseline:** evergreen browsers. Exclusive open uses `<details name>` (Chrome 120+, Safari 17.2+, Firefox 130+). Without `name` support, items still toggle independently.

## Decisions

| Decision | Choice | Why |
|----------|--------|-----|
| Interaction model | Exclusive `<details name>` + `<summary>` | Survives Froala; same tags as accordion/expand |
| Rejected: `:target` + hash links | — | Nextlot strips `id` and unwraps `nav`/`section` |
| Rejected: radio / checkbox tabs | — | Nextlot strips `label` and `input` |
| Visual style | Segmented track; open item expands full-width with panel | Keeps pill chrome without absolute panels |
| Brand | StockLive navy `#021C45`, terracotta `#CD7854` | Existing language |
| Token scope | `--sl-ce-*` on `:root` **and** component roots | Host may isolate `:root` |
| Host isolation | `css/host.css` last in the bundle | Beat Nextlot `a` / `table` / background rules |
| Packaging | Standalone CSS file Nextlot loads | `catalogue/stocklive-content-elements.css` for hand-off |

## Architecture

```text
sl-ce (pattern / demo source)
  css/
    tokens.css
    tabs.css
    …
    host.css                 # Nextlot isolation (last)
    content-elements.css     # shippable @import entry
  partials/tabs/default.html
  → stocklive-content-elements.css (Nextlot)

Nextlot page
  <link rel="stylesheet" href="…/stocklive-content-elements.css">
  Froala HTML body contains .sl-ce-tabs markup
```

### Units

1. **Tokens** — `--sl-ce-*` on `:root` / `html` / component roots.
2. **Tabs component** — BEM `.sl-ce-tabs`; exclusive details; open-state chrome.
3. **Author documentation** — [author-guide.md](../../content-elements/author-guide.md).
4. **Preview surface** — https://stocklive-group.github.io/sl-ce/

## Author markup contract

```html
<div class="sl-ce-tabs">
  <details class="sl-ce-tabs__item" name="lot-42-tabs" open>
    <summary class="sl-ce-tabs__tab">Overview</summary>
    <div class="sl-ce-tabs__panel">
      <!-- arbitrary HTML -->
    </div>
  </details>
  <details class="sl-ce-tabs__item" name="lot-42-tabs">
    <summary class="sl-ce-tabs__tab">Details</summary>
    <div class="sl-ce-tabs__panel">
      <!-- arbitrary HTML -->
    </div>
  </details>
</div>
```

### Author must

- Use the **same `name=`** on every item in a group (unique per lot / group)
- Put **`open`** on the default item
- Keep panel content inside `.sl-ce-tabs__panel`

### Author must not

- Use `id` / `:target`, `<nav>`, `<section>`, `<label>`, or `<input>` for tabs
- Add “active” classes manually
- Use inline styles, `<style>`, or `<script>`

## CSS behaviour

| Rule | Behaviour |
|------|-----------|
| Track | `.sl-ce-tabs` is a flex wrap row with track background |
| Closed items | `order: 0`; pill summaries only |
| Open item | `order: 1; flex-basis: 100%` — panel sits below the inactive pills |
| Marker | Summary markers hidden (accordion-style) |
| Isolation | Selectors under `.sl-ce-tabs` / host.css reasserts |

## Accessibility

**Supported without JS**

- Native `<details>` / `<summary>` keyboard activation
- `:focus-visible` on summary

**Not in v1**

- Full WAI-ARIA Tabs pattern (arrow keys, `aria-selected`)

## Success criteria

- Paste snippet into Froala with CSS loaded → only one panel visible; clicking summaries switches
- Track / surface backgrounds visible against Nextlot host CSS
- Two groups on one page with unique `name=` values
- Namespace holds (`sl-ce` / `--sl-ce-*`)

## Out of scope

- JavaScript enhancement
- Restoring `:target` or radio tabs unless Nextlot expands the allowlist
- Nextlot brand re-theme (variables allow a later pass)
