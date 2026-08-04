# Froala / Nextlot constraints (Content Elements)

**Status:** Active  
**Audience:** StockLive + Nextlot engineers, content authors  
**Repo:** https://github.com/StockLive-Group/sl-ce  
**Related:** [Author guide](author-guide.md) · [01 tabs](../plans/content-elements/01-tabs.md)

## Why this note exists

Content Elements must render inside Nextlot lot descriptions (Froala `fr-view`). Staging tests (Aug 2026) showed the sanitiser and host CSS break patterns that work fine in the static demo gallery.

This documents **what was stripped**, **why we changed the library**, and **what Nextlot still needs to allow**.

## What Froala / Nextlot strips

Observed on staging lot HTML after catalogue upload:

| Stripped | Effect on Content Elements |
|----------|----------------------------|
| `id` attributes | `:target` + hash-link tabs never activate |
| `<nav>`, `<section>` (unwrapped) | Classes on those wrappers disappear; tab list/panel chrome vanishes; all panel content shows |
| `<label>`, `<input>` | Radio / checkbox CSS tabs cannot work |
| `:root`-only custom properties (effectively) | Vars often fail to resolve in the lot description tree |

Still available (and used): `div`, `span`, `a`, `p`, `h4`, `table…`, **`details` / `summary`**, `class`, `href`, `style` (limited — traits percentile only), `name` / `open` on details (needed for exclusive tabs).

## Decisions (and why)

### 1. Tokens on component roots + `:root`

**Why:** Host pages may isolate or ignore `:root` custom properties in the Froala body. Defining `--sl-ce-*` on `.sl-ce-tabs`, `.sl-ce-accordion`, `.sl-ce-table`, etc. makes vars inherit from the component itself.

### 2. `css/host.css` loaded last

**Why:** Nextlot native rules (`a`, `table`, `summary`, backgrounds) override single-class CE selectors. Host isolation raises specificity and reasserts track/surface colours with hex fallbacks.

### 3. Tabs = exclusive `<details name>`

**Why:** After `:target` and radios were ruled out by the sanitiser, native exclusive details (same tags as accordion/expand) is the only CSS-only switcher that survives Froala.

- Unique `name="lot-N-tabs"` per group → one open at a time (modern evergreen browsers)
- `open` on the default item
- Layout: inactive pills on the track; open item expands full-width with its panel below

**Trade-off:** Visual model is “segmented track + disclosure panel”, not classic hash tabs. Acceptable for lot descriptions; accordion remains available for multi-open stacks.

### 4. Avoid `nav` / `section` / form controls in paste snippets

**Why:** Even if Froala’s default allowlist includes them, Nextlot’s configuration does not keep them intact. Author snippets use only tags proven to survive staging.

## Catalogue / hand-off

| File | Purpose |
|------|---------|
| `catalogue/example-catalogue.xlsx` | 23 demo lots for staging upload (`Lot #`, `Lot Name`, `Description`, `Quantity`) |
| `catalogue/stocklive-content-elements.css` | Comment-free single-file bundle (tokens → components → host) |

## Ask of Nextlot (if anything still breaks)

1. Allow `name` and `open` on `<details>` (exclusive tabs).
2. Prefer not to strip `class` on wrappers we keep (`div` / `details` / `summary`).
3. Load `stocklive-content-elements.css` on lot pages that render Froala HTML.
4. Optional later: allowlist `id` / `<input type="radio">` if we ever want classic tab patterns back.

## History

| Date | Change |
|------|--------|
| 2026-08 | Staging: `:target` tabs broken (ids / nav / section stripped); host CSS washes backgrounds |
| 2026-08 | Tokens on component roots; `host.css` isolation |
| 2026-08 | Radio tabs attempted — `<label>` / `<input>` also stripped |
| 2026-08 | Tabs switched to exclusive `<details name>` |
