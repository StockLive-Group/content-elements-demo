# 02 — Content Elements: Accordion

**Status:** Shipped  
**Subsystem:** StockLive Content Elements (CSS library for Nextlot / Froala)  
**Namespace:** `sl-ce` (StockLive Content Elements)  
**Rich companion:** [02-accordion.html](02-accordion.html) (live HTML preview)  
**Siblings:** [01-tabs.md](01-tabs.md) · [03-table.md](03-table.md) · [04-traits.md](04-traits.md) · [05-expand.md](05-expand.md)  
**Reference:** [RapidRailsUI Accordion](https://rapidrails.cc/docs/accordion)  
**Repo / demo:** https://github.com/StockLive-Group/sl-ce · https://stocklive-group.github.io/sl-ce/lot-accordion.html

## Purpose

Second content-element component after tabs. Same constraints, tokens, and packaging: HTML + CSS only for Froala authors on Nextlot.

Visual language follows RapidRailsUI Accordion variants, re-skinned with StockLive brand tokens (navy / terracotta) — not a Stimulus port. Primary lot look: **`--bordered`**.

For a single show-more control (not a multi-item stack), use [05 — Expand](05-expand.md).

## Context

| Role | Responsibility |
|------|----------------|
| StockLive | Author HTML patterns, build/ship CSS, document snippets, preview/verify |
| Nextlot | Load the CSS bundle globally; Froala authors paste HTML only |
| Content author | Copy-paste markup; use `open` for default-expanded items; keep titles short |

## Constraints (hard)

- HTML and CSS only — no JavaScript
- No inline styles, no `<style>` tags, no `<script>` tags
- No form controls used as CSS state hacks (`input`, `select`, `textarea`, hidden checkbox/radio)
- CSS is namespaced (BEM under `sl-ce-*`); no styling of bare HTML elements
- Responsive; reasonably accessible given CSS-only limits
- Markup must be simple enough for non-technical Froala authors
- **Browser baseline:** evergreen browsers that support `<details>` / `<summary>` (universal in modern browsers). No `:has()` required for accordion behaviour.

## Decisions

| Decision | Choice |
|----------|--------|
| Interaction model | Native `<details>` / `<summary>` disclosure |
| Why not `:target` (like tabs) | Accordion is multi-open disclosure; `:target` is single-hash and a poor fit |
| Why not radio exclusive | Form-control CSS state is banned; exclusive mode needs radio or JS |
| Exclusive (one open) | **Out of scope v1** — multiple items may be open (RRUI `exclusive: false`) |
| Visual default | `--bordered` (shared outer border, item dividers, rotating chevron) |
| Variants | `--default` · `--bordered` · `--separated` · `--flush` · `--ghost` · `--chevron-left` |
| Brand | StockLive (navy `#021C45`, terracotta `#CD7854`, existing tokens) |
| Default open | HTML `open` attribute on `<details>` |
| Chevron | CSS-only span (no Lucide / no SVG dependency) |
| Panel padding | Top + sides + bottom so content is not flush under the trigger |
| Token scope | Shared `--sl-ce-*` on `:root` with tabs |
| Packaging | Same standalone CSS file as tabs |

## Architecture

```text
sl-ce (pattern / demo source)
  css/
    tokens.css
    accordion.css             # .sl-ce-accordion* + variants
    content-elements.css
  partials/accordion/*.html
  → stocklive-content-elements.css (Nextlot)

Nextlot page
  <link rel="stylesheet" href="…/stocklive-content-elements.css">
  Froala HTML body contains .sl-ce-accordion markup
```

## Author markup contract

```html
<div class="sl-ce-accordion sl-ce-accordion--bordered">
  <details class="sl-ce-accordion__item" open>
    <summary class="sl-ce-accordion__trigger">
      <span class="sl-ce-accordion__title">Overview</span>
      <span class="sl-ce-accordion__chevron" aria-hidden="true"></span>
    </summary>
    <div class="sl-ce-accordion__panel">
      <!-- arbitrary HTML — tables, traits, etc. -->
    </div>
  </details>

  <details class="sl-ce-accordion__item">
    <summary class="sl-ce-accordion__trigger">
      <span class="sl-ce-accordion__title">Pedigree</span>
      <span class="sl-ce-accordion__chevron" aria-hidden="true"></span>
    </summary>
    <div class="sl-ce-accordion__panel">
      <!-- arbitrary HTML -->
    </div>
  </details>
</div>
```

### Author must

- Wrap items in `.sl-ce-accordion` (+ variant modifier)
- Use `<details class="sl-ce-accordion__item">` + `<summary class="sl-ce-accordion__trigger">`
- Put the visible label in `.sl-ce-accordion__title`
- Include the decorative `.sl-ce-accordion__chevron` with `aria-hidden="true"`
- Put panel body in `.sl-ce-accordion__panel`
- Mark default-open sections with the HTML `open` attribute (zero or more)

### Variants

| Class | Look |
|-------|------|
| `sl-ce-accordion--default` | Dividers only |
| `sl-ce-accordion--bordered` | Outer border (primary) |
| `sl-ce-accordion--separated` | Gapped item cards |
| `sl-ce-accordion--flush` | No borders |
| `sl-ce-accordion--ghost` | Hairline / transparent |
| `sl-ce-accordion--chevron-left` | Chevron on the left (combinable) |

### Author must not

- Add “active” / “open” classes manually (browser owns `[open]`)
- Use inline styles, `<style>`, or `<script>`
- Nest interactive form controls inside `<summary>` (breaks click / a11y)
- Rely on exclusive one-at-a-time behaviour

### Invalid / mistaken markup (defined behaviour)

| Mistake | What the user sees |
|---------|-------------------|
| Missing `summary` | Browser-default / broken disclosure |
| Missing chevron span | Title still works; no rotate indicator |
| `open` on every item | All start expanded (valid; intentional) |
| Panel content outside `__panel` | May still show; spacing/tokens may look off |

## CSS behaviour

| Rule | Behaviour |
|------|-----------|
| Container | Variant-dependent chrome (bordered = outer border + radius) |
| Trigger | Full-width flex row; title left, chevron right; hover track tint |
| Marker | Native disclosure triangle hidden (`::-webkit-details-marker` / `::marker`) |
| Open state | `[open]` tints trigger; chevron rotates; terracotta accent on chevron |
| Panel | Padding on all sides including top |
| Focus | `:focus-visible` inset ring on summary |
| Motion | Short chevron rotate; `prefers-reduced-motion: reduce` disables |
| Isolation | Selectors under `.sl-ce-accordion` / `.sl-ce-*` only |

### Multi-group behaviour

Independent of page hash. Multiple accordion groups on one page keep their own open/closed state (unlike tabs’ single `:target`).

## Accessibility

**Supported without JS**

- Native disclosure keyboard (Enter / Space on summary)
- Tab / Shift+Tab through summaries
- Browser exposes expanded/collapsed state
- Decorative chevron `aria-hidden="true"`
- `:focus-visible` ring

**Not in v1**

- Exclusive accordion (one open) — needs radio or JS
- Leading Lucide icons / subtitles (RRUI item options) — keep markup minimal for Froala
- Smooth height animation beyond browser defaults

## Out of scope (accordion v1)

- JavaScript / Stimulus (`rui--accordion`)
- Exclusive / single-open mode
- Form-control state hacks
- Turbo Frame lazy panels
- Icon / subtitle header slots

## Success criteria

- Paste snippet into Froala with CSS loaded → items expand/collapse with **no JS**
- Multiple items can be open independently
- Open chevron rotates; focus-visible ring present
- Two accordion groups on one page do not interfere
- Namespace holds (`sl-ce` / `--sl-ce-*`)
- Preview / Kuickr companion matches the shipped approach

## Open assumptions

- Nextlot can load the external CSS on Froala-rendered pages
- Froala allows `details`, `summary`, `div`, `span`, `class`, `open`, `aria-hidden`
- If sanitiser strips any of these, Nextlot must allowlist them
