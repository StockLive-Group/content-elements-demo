# 04 — Content Elements: Traits (percentile profile)

**Status:** Shipped  
**Subsystem:** StockLive Content Elements (CSS library for Nextlot / Froala)  
**Namespace:** `sl-ce` (StockLive Content Elements)  
**Rich companion:** [04-traits.html](04-traits.html) (live HTML preview)  
**Siblings:** [01-tabs.md](01-tabs.md) · [02-accordion.md](02-accordion.md) · [03-table.md](03-table.md)  
**Repo / demo:** https://github.com/StockLive-Group/sl-ce · https://stocklive-group.github.io/sl-ce/

## Purpose

Compact **EBV + percentile** profile for lot cards and accordion panels. This is **not** a table — row layout is a list with a progress bar, Top% badge, and percentile label.

Composes with [badge](06-badge.md) for the rank chip and often sits inside [accordion](02-accordion.md) or [expand](05-expand.md).

## Context

| Role | Responsibility |
|------|----------------|
| StockLive | Ship CSS + partials; document tones and percentile contract |
| Nextlot | Load CSS; Froala authors paste HTML |
| Content author | Set `--sl-ce-percentile` (0–100) per item; keep Top% text ≈ `100 − percentile` |

## Constraints (hard)

- HTML and CSS only — no JavaScript
- No `<style>` / `<script>` tags
- Namespaced BEM under `sl-ce-*`
- Markup simple enough for Froala authors
- **Exception:** per-item `style="--sl-ce-percentile: N"` is the supported way to set bar width (custom property only — not layout/color hacks)

## Decisions

| Decision | Choice |
|----------|--------|
| Structure | `<ul>` of `.sl-ce-traits__item` (not `<table>`) |
| Bar fill | CSS `width` from `--sl-ce-percentile` on the item |
| Rank chip | `.sl-ce-badge` (solid / sm / pill) + `.sl-ce-traits__rank` |
| Tones | Modifier classes for breed-group colour |
| Density | Optional `--dense` |
| Packaging | Same CSS bundle |

## Architecture

```text
sl-ce
  css/traits.css              # .sl-ce-traits*
  partials/traits/{default,compact,full}.html
```

## Author markup contract

```html
<div class="sl-ce-traits sl-ce-traits--dense">
  <h3 class="sl-ce-traits__title">Trait profile</h3>
  <p class="sl-ce-traits__description">Percentile rank with EBV.</p>
  <ul class="sl-ce-traits__list">
    <li class="sl-ce-traits__item sl-ce-traits__item--calving" style="--sl-ce-percentile: 90">
      <div class="sl-ce-traits__head">
        <div class="sl-ce-traits__label">
          <span class="sl-ce-traits__name">Calving Ease Direct</span>
          <span class="sl-ce-traits__ebv">EBV +4.2</span>
        </div>
        <span class="sl-ce-badge sl-ce-badge--solid sl-ce-badge--sm sl-ce-badge--pill sl-ce-traits__rank">Top 10%</span>
      </div>
      <div class="sl-ce-traits__track" role="img" aria-label="90th percentile">
        <span class="sl-ce-traits__fill"></span>
      </div>
      <p class="sl-ce-traits__perc">90th percentile</p>
    </li>
  </ul>
</div>
```

### Author must

- Set `--sl-ce-percentile` (0–100) on each item
- Keep badge text consistent with percentile (`Top (100 − N)%`)
- Give the track an `aria-label` describing the percentile
- Pick a tone modifier when colour-coding groups

### Tone modifiers

| Class | Use |
|-------|-----|
| `--calving` | Calving ease / birth |
| `--growth` | Weight / growth |
| `--maternal` | Maternal |
| `--milk` | Milk |
| `--fertility` | Fertility |
| `--other` | Misc |
| `--carcase` | Carcase / EMA |
| `--structure` | Structure |
| `--index` | Indexes / IMF |

### Author must not

- Model traits as `.sl-ce-table` rows
- Omit the fill span inside `__track`
- Use arbitrary inline styles beyond `--sl-ce-percentile`

## CSS behaviour

| Rule | Behaviour |
|------|-----------|
| Fill width | `.sl-ce-traits__fill` width = `var(--sl-ce-percentile)%` |
| Rank chip | Inherits tone via `--sl-ce-trait-color` |
| Dense | Tighter gap between items |
| Group label | Optional `.sl-ce-traits__group` between sections |

## Accessibility

- List semantics (`ul` / `li`)
- Track exposed as `role="img"` + `aria-label`
- Rank text is visible (not colour-only)

## Out of scope

- Computed percentiles / live EBV APIs
- Sorting / filtering traits
- Replacing ASBV PDF / registry links (use [button](07-button.md))

## Success criteria

- Bars render at the authored percentile
- Badge + bar share the tone colour
- Fits comfortably in accordion panels and lot cards
- Namespace holds (`sl-ce` / `--sl-ce-*`)
