# Content Elements — Author guide (Nextlot / Froala)

StockLive ships **`stocklive-content-elements.css`**. Nextlot loads it on pages that render Froala HTML. Authors paste the snippets below — no JavaScript, no `<style>` / `<script>` tags.

**Namespace:** `sl-ce`  
**Demo gallery:** https://stocklive-group.github.io/sl-ce/  
**Repo:** https://github.com/StockLive-Group/sl-ce  
**Specs:** [01 tabs](../plans/content-elements/01-tabs.md) · [02 accordion](../plans/content-elements/02-accordion.md) · [03 table](../plans/content-elements/03-table.md) · [04 traits](../plans/content-elements/04-traits.md) · [05 expand](../plans/content-elements/05-expand.md) · [06 badge](../plans/content-elements/06-badge.md) · [07 button](../plans/content-elements/07-button.md)

---

## Tabs

### Copy-paste

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
      <p>Overview content…</p>
    </section>
    <section class="sl-ce-tabs__panel">
      <p>Details content…</p>
    </section>
    <section class="sl-ce-tabs__panel">
      <p>Pricing content…</p>
    </section>
  </div>
</div>
```

### Rules

1. **Unique IDs** on the tiny targets (prefix them, e.g. `sale-42-overview`).
2. **Same order** for targets, tab links, and panels (1st ↔ 1st ↔ 1st).
3. Mark **one** default tab and **one** default panel with `--default`.
4. **Max 8 tabs** per group.
5. Do **not** put `id`s on panels (that causes scroll jump).
6. Do not add “active” classes — CSS handles that.

### Gotchas

| Situation | What happens |
|-----------|----------------|
| Two tab groups on one page | Fine — use unique IDs. Activating group B resets group A to its default. |
| Link elsewhere to `#lot-overview` | Opens that tab (same hash mechanism). |
| Mobile, many labels | Tab row scrolls horizontally. |
| More than 8 tabs | Extra tabs will not switch via the shared CSS. |

---

## Accordion

### Copy-paste

```html
<div class="sl-ce-accordion sl-ce-accordion--bordered">
  <details class="sl-ce-accordion__item" open>
    <summary class="sl-ce-accordion__trigger">
      <span class="sl-ce-accordion__title">Overview</span>
      <span class="sl-ce-accordion__chevron" aria-hidden="true"></span>
    </summary>
    <div class="sl-ce-accordion__panel">
      <p>Overview content…</p>
    </div>
  </details>
  <details class="sl-ce-accordion__item">
    <summary class="sl-ce-accordion__trigger">
      <span class="sl-ce-accordion__title">Pedigree</span>
      <span class="sl-ce-accordion__chevron" aria-hidden="true"></span>
    </summary>
    <div class="sl-ce-accordion__panel">
      <p>Pedigree content…</p>
    </div>
  </details>
</div>
```

### Variants

`sl-ce-accordion--default` · `--bordered` (primary) · `--separated` · `--flush` · `--ghost` · `--chevron-left` (combinable)

Multiple items may be open. Exclusive one-open needs JS — not supported.

---

## Table

### Copy-paste

```html
<div class="sl-ce-table sl-ce-table--bordered sl-ce-table--rounded sl-ce-table--hoverable sl-ce-table--base">
  <div class="sl-ce-table__scroll">
    <table class="sl-ce-table__grid">
      <thead>
        <tr>
          <th scope="col" class="sl-ce-table__th">Lot</th>
          <th scope="col" class="sl-ce-table__th sl-ce-table__th--right">Price</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="sl-ce-table__td" data-label="Lot">#1</td>
          <td class="sl-ce-table__td sl-ce-table__td--right" data-label="Price">$12,500</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
```

Add `data-label` on every cell when using `--cards` or `--stack`. Sorting / selection need JS — out of scope.

For EBV percentiles, use **Traits** below — not a table.

---

## Traits (percentile profile)

### Copy-paste

```html
<div class="sl-ce-traits sl-ce-traits--dense">
  <h3 class="sl-ce-traits__title">Trait profile</h3>
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

Set `--sl-ce-percentile` (0–100) per item. Top% ≈ `100 − percentile`. Tones: `--calving` · `--growth` · `--maternal` · `--milk` · `--fertility` · `--other` · `--carcase` · `--structure` · `--index`.

---

## Expand (show more)

```html
<details class="sl-ce-expand sl-ce-expand--bordered sl-ce-expand--footer">
  <summary class="sl-ce-expand__trigger">
    <span class="sl-ce-expand__label-more">Show more details</span>
    <span class="sl-ce-expand__label-less">Show less</span>
    <span class="sl-ce-expand__chevron" aria-hidden="true"></span>
  </summary>
  <div class="sl-ce-expand__panel">
    <p>Secondary content…</p>
  </div>
</details>
```

Variants: `--bordered` · `--button` · `--footer`.

---

## Badge

```html
<div class="sl-ce-badge-group" aria-label="Lot accreditations">
  <a class="sl-ce-badge sl-ce-badge--soft sl-ce-badge--primary sl-ce-badge--sm sl-ce-badge--pill"
     href="https://example.com" target="_blank" rel="noopener noreferrer">LPA</a>
</div>
```

Styles: `--solid` · `--outline` · `--ghost` · `--soft` · `--link`. Always use `rel="noopener noreferrer"` on external links.

---

## Button

```html
<div class="sl-ce-button-group">
  <a class="sl-ce-button sl-ce-button--solid sl-ce-button--primary sl-ce-button--sm sl-ce-button--rounded"
     href="https://example.com" target="_blank" rel="noopener noreferrer">View ASBVs</a>
</div>
```

Prefer `<a>` in Froala. Styles match badge axes (solid/outline/ghost/soft/link + colours + sizes).

---

## Do / don’t

| Do | Don’t |
|----|--------|
| Paste the snippet and edit labels / IDs / panel HTML | Inline `style="…"` (except traits `--sl-ce-percentile`) or `<style>` / `<script>` |
| Keep target / tab / panel order aligned | Reuse the same `id` twice |
| Use `aria-label` that describes the set | Rely on form checkboxes for state |
| Open external links with `rel="noopener noreferrer"` | Put `id`s on tab panels |
