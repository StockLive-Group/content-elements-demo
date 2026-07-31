# 01 — Content Elements: Tabs

**Status:** Shipped  
**Subsystem:** StockLive Content Elements (CSS library for Nextlot / Froala)  
**Namespace:** `sl-ce` (StockLive Content Elements)  
**Rich companion:** [01-tabs.html](01-tabs.html) (live HTML preview)  
**Siblings:** [02-accordion.md](02-accordion.md) · [03-table.md](03-table.md) · [04-traits.md](04-traits.md) · [05-expand.md](05-expand.md) · [06-badge.md](06-badge.md) · [07-button.md](07-button.md)  
**Repo / demo:** https://github.com/StockLive-Group/sl-ce · https://stocklive-group.github.io/sl-ce/lot-tabs.html

## Purpose

StockLive owns a reusable **HTML + CSS-only** component library for content authors. Nextlot loads the CSS globally and authors paste HTML snippets into Froala (which strips JavaScript, inline `<style>`, and executable code).

Tabs are the first component. The same namespace, tokens, and packaging model support siblings (accordion, table, traits, expand, badge, button).

## Context

| Role | Responsibility |
|------|----------------|
| StockLive | Author HTML patterns, build/ship CSS, document snippets, preview/verify |
| Nextlot | Load the CSS bundle globally; Froala authors paste HTML only |
| Content author | Copy-paste markup; unique IDs on fixed targets; keep target / tab / panel order aligned; mark one default |

## Constraints (hard)

- HTML and CSS only — no JavaScript
- No inline styles, no `<style>` tags, no `<script>` tags
- No form controls used as CSS state hacks (`input`, `select`, `textarea`, hidden checkbox/radio)
- CSS is namespaced (BEM under `sl-ce-*`); no styling of bare HTML elements
- Responsive; reasonably accessible given CSS-only limits
- Markup must be simple enough for non-technical Froala authors
- **Browser baseline:** evergreen browsers with CSS `:has()` (Chrome/Edge 105+, Safari 15.4+, Firefox 121+). Active-tab chrome and panel switching both use `:has()` / `:target`. No polyfill in v1.

## Decisions

| Decision | Choice |
|----------|--------|
| Interaction model | `:target` + in-page anchor links |
| Scroll-jump mitigation | Hash targets live in `.sl-ce-tabs__targets` with **`position: fixed`** (always in viewport) so hash changes do not scroll the page |
| Panel pairing | **Order match:** Nth target ↔ Nth tab ↔ Nth panel (no per-id CSS rules). Library CSS supports up to **8** tabs per group |
| Visual style | Segmented control (pill track, elevated active tab) |
| Brand | StockLive (navy `#021C45`, terracotta `#CD7854`, existing card/shadow language) |
| Token scope | CSS custom properties on `:root`, namespaced `--sl-ce-*` |
| Mobile tab list | Single row, `overflow-x: auto` (horizontal scroll) |
| Multi-group behaviour | Only one page `:target`; other groups fall back to default |
| Packaging | Standalone CSS file StockLive builds and Nextlot loads |
| Pattern source | [sl-ce](https://github.com/StockLive-Group/sl-ce) demo repo (`css/tabs.css`) |

## Architecture

```text
sl-ce (pattern / demo source)
  css/
    tokens.css
    tabs.css
    content-elements.css      # shippable @import entry
  partials/tabs/default.html
  → stocklive-content-elements.css (Nextlot)

Nextlot page
  <link rel="stylesheet" href="…/stocklive-content-elements.css">
  Froala HTML body contains .sl-ce-tabs markup
```

### Units

1. **Tokens** — `--sl-ce-*` on `:root`.
2. **Tabs component** — BEM `.sl-ce-tabs`; fixed targets; `:target` + `:has()` + `nth-child` pairing.
3. **Author documentation** — [docs/content-elements/author-guide.md](../../content-elements/author-guide.md).
4. **Preview surface** — https://stocklive-group.github.io/sl-ce/ · Kuickr companion.

## Author markup contract

```html
<div class="sl-ce-tabs">
  <div class="sl-ce-tabs__targets" aria-hidden="true">
    <span id="lot-overview"></span>
    <span id="lot-details"></span>
    <span id="lot-pricing"></span>
  </div>

  <nav class="sl-ce-tabs__list" aria-label="Tabs">
    <a class="sl-ce-tabs__tab sl-ce-tabs__tab--default" href="#lot-overview">Overview</a>
    <a class="sl-ce-tabs__tab" href="#lot-details">Details</a>
    <a class="sl-ce-tabs__tab" href="#lot-pricing">Pricing</a>
  </nav>

  <div class="sl-ce-tabs__panels">
    <section class="sl-ce-tabs__panel sl-ce-tabs__panel--default">
      <!-- arbitrary HTML -->
    </section>
    <section class="sl-ce-tabs__panel">
      <!-- arbitrary HTML -->
    </section>
    <section class="sl-ce-tabs__panel">
      <!-- arbitrary HTML -->
    </section>
  </div>
</div>
```

### Author must

- Put unique `id`s on the **targets** (not on panels)
- Keep **the same order** for targets, tab links, and panels (1st target ↔ 1st tab ↔ 1st panel)
- Mark exactly one panel with `sl-ce-tabs__panel--default` and the matching tab with `sl-ce-tabs__tab--default`
- Stay within **8 tabs** per group (library CSS cap)

### Author must not

- Add “active” classes manually
- Use inline styles, `<style>`, or `<script>`
- Reuse the same target `id` twice on one page
- Put `id`s on panels (that reintroduces scroll-to-panel)

### Invalid / mistaken markup (defined behaviour)

| Mistake | What the user sees |
|---------|-------------------|
| No `--default` panel / tab | When the group has no `:target`, **no panel** (and no elevated tab) |
| Multiple `--default` panels | All default-marked panels visible when idle (stacked) |
| Order mismatch (target vs panel) | Wrong panel shows for a tab click |
| Tab `href` with no matching target `id` | Hash updates; group stays in default-state behaviour |
| Duplicate `id` on the page | Undefined / browser-dependent |
| More than 8 tabs | Tabs beyond 8 never activate via library CSS |

## CSS behaviour

| Rule | Behaviour |
|------|-----------|
| Targets | `.sl-ce-tabs__targets` is `position: fixed; top:0; left:0; width:0; height:0; opacity:0; pointer-events:none` so hash navigation does not scroll |
| Default | Group with no descendant target `:target` shows `--default` panel and `--default` tab chrome |
| Active panel | `:has(.sl-ce-tabs__targets > :nth-child(n):target)` reveals `.sl-ce-tabs__panel:nth-child(n)` |
| Active tab | Same `nth-child(n)` styles the matching `.sl-ce-tabs__tab` |
| Hover / focus | Hover and `:focus-visible` on tabs |
| Layout | Segmented track; no wrap; `overflow-x: auto` on small screens |
| Motion | Short transition on tab chrome; `prefers-reduced-motion: reduce` disables it |
| Isolation | Selectors under `.sl-ce-tabs` / `.sl-ce-*` only |

### Multi-group edge case

Activating a tab in group B changes the page hash. Group A returns to its `--default`. Documented; no JS in v1.

### Deep links

A page TOC or external `#lot-overview` link still activates the matching tab group (same mechanism). Document for authors.

## Visual design

- Segmented control: muted track `#edf2f7`, white elevated active pill, navy text
- Subtle border/shadow aligned with StockLive cards
- Radii ~0.5rem track / ~0.375rem pills
- Comfortable padding; panel in a bordered surface below the list

## Accessibility

**Supported without JS**

- Real links → Tab / Shift+Tab / Enter
- `nav` + `aria-label` (customisable)
- `:focus-visible` ring
- Targets `aria-hidden="true"` (decorative hash hooks)

**Not in v1**

- Full WAI-ARIA Tabs pattern (arrow keys, `aria-selected` updates)
- Independent multi-group state without hash conflicts

## Deliverables (StockLive)

1. Pattern CSS in [sl-ce](https://github.com/StockLive-Group/sl-ce) (`css/tabs.css`)
2. Shippable `stocklive-content-elements.css` for Nextlot
3. Preview: https://stocklive-group.github.io/sl-ce/
4. Author guide at `docs/content-elements/author-guide.md`
5. Rich companion `plans/content-elements/01-tabs.html` (Kuickr)

## Out of scope (tabs v1)

- JavaScript enhancement
- Form-control or `<details>` tab switching
- Nextlot brand re-theme (variables allow a later pass)
- Froala sanitiser allowlist changes in Nextlot (dependency called out below)

## Success criteria

- Paste snippet into Froala with CSS loaded → tabs switch with **no scroll jump**
- Active tab elevated chrome + `:focus-visible` ring
- Two groups on one page; switching resets the other to default
- Narrow viewport: horizontal scroll on the tab list
- Namespace holds (`sl-ce` / `--sl-ce-*`)
- Preview / Kuickr companion matches the shipped approach

## Open assumptions

- Nextlot can load the external CSS on Froala-rendered pages
- Froala allows `nav`, `section`, `div`, `span`, `a[href^="#"]`, `class`, `id`, `aria-label`, `aria-hidden`
- If sanitiser strips any of these, Nextlot must allowlist them
- Nextlot browsers meet the `:has()` baseline
