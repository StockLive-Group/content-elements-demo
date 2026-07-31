# Content Elements demo

Public preview of StockLive **Content Elements** (`sl-ce`) — CSS-only components for Froala / Nextlot.

**Live:** https://stocklive-group.github.io/sl-ce/

## Structure

Mirrors the Kuickr plan architecture (`plans/content-elements/`):

```text
css/
  tokens.css              # :root { --sl-ce-* }
  tabs.css                # .sl-ce-tabs*
  accordion.css           # .sl-ce-accordion* + variants
  table.css               # .sl-ce-table* + variants
  traits.css              # .sl-ce-traits* (EBV percentile profile)
  expand.css              # .sl-ce-expand* (show more)
  badge.css               # .sl-ce-badge* (RRUI-aligned)
  button.css              # .sl-ce-button* (RRUI-aligned)
  content-elements.css    # shippable @import entry
  demo.css                # preview chrome only (not shipped)
partials/
  tabs/default.html
  accordion/{default,bordered,separated,flush,ghost,chevron-left}.html
  table/{default,caption,striped,square,sticky,footer,cards,empty}.html
  traits/{default,compact,full}.html
  expand/default.html
  badge/{default,lot-accreditations}.html
  button/default.html
index.html                # stacked variants gallery
lot-*.html                # lot-row context demos
```

## Pages

| Page | What |
|------|------|
| [Variants](https://stocklive-group.github.io/sl-ce/) | All variants stacked |
| [Lot · Tabs](https://stocklive-group.github.io/sl-ce/lot-tabs.html) | Auction lot shell + tabs |
| [Lot · Accordion](https://stocklive-group.github.io/sl-ce/lot-accordion.html) | Auction lot shell + bordered accordion |
| [Lot · Table](https://stocklive-group.github.io/sl-ce/lot-table.html) | Auction lot shell + ASBV table + traits |
| [Lot · Combined](https://stocklive-group.github.io/sl-ce/lot-combined.html) | Composed lot: tabs / accordion / expand / badges / buttons |

## Accordion variants

| Class | Look |
|-------|------|
| `sl-ce-accordion--default` | Dividers only |
| `sl-ce-accordion--bordered` | Outer border (primary) |
| `sl-ce-accordion--separated` | Gapped item cards |
| `sl-ce-accordion--flush` | No borders |
| `sl-ce-accordion--ghost` | Hairline / transparent |
| `sl-ce-accordion--chevron-left` | Chevron on the left (combinable) |

## Table variants

| Class | Look |
|-------|------|
| `sl-ce-table--bordered` | Outer + cell borders (primary) |
| `sl-ce-table--rounded` / `--square` | Corner shape |
| `sl-ce-table--striped` | Alternating row wash |
| `sl-ce-table--striped-columns` | Alternating column wash |
| `sl-ce-table--hoverable` | Row hover highlight |
| `sl-ce-table--sticky-header` | Sticky thead in scroll box |
| `sl-ce-table--cards` / `--stack` | Mobile responsive layouts (`data-label` on cells) |
| `sl-ce-table--xs` … `--xl` | Cell padding / type size |

## Traits (percentile profile)

Compact list — not a table. Each row: trait name, EBV, Top% badge, progress bar, percentile label.

| Class | Look |
|-------|------|
| `sl-ce-traits` | Trait profile list |
| `sl-ce-traits--dense` | Tighter vertical rhythm |
| `sl-ce-traits__item--calving` … `--index` | Tone for bar + badge |

Set `--sl-ce-percentile` (0–100) on each `.sl-ce-traits__item`. Top% badge ≈ `100 − percentile`.

## Expand (show more)

| Class | Look |
|-------|------|
| `sl-ce-expand` | Text “Show more / Show less” disclosure |
| `sl-ce-expand--bordered` | Bordered card shell |
| `sl-ce-expand--button` | Pill button trigger |
| `sl-ce-expand--footer` | Trigger sits under the revealed panel |

## Badge

RRUI-aligned ([docs](https://rapidrails.cc/docs/badge)). Use `<span>` for static labels or `<a target="_blank" rel="noopener noreferrer">` for link badges.

| Class | Look |
|-------|------|
| `sl-ce-badge--solid` / `--outline` / `--ghost` / `--soft` / `--link` | Variants |
| `sl-ce-badge--primary` … `--info` | Colors |
| `sl-ce-badge--xs` … `--lg` | Sizes |
| `sl-ce-badge--pill` / `--rounded` / `--square` | Shapes |
| `sl-ce-badge-group` | Flex wrap row (sit under `lot__title`) |

## Button

RRUI-aligned ([docs](https://rapidrails.cc/docs/button)). Prefer `<a>` for navigation with `target="_blank" rel="noopener noreferrer"` for external destinations.

| Class | Look |
|-------|------|
| `sl-ce-button--solid` / `--outline` / `--ghost` / `--soft` / `--link` | Variants |
| `sl-ce-button--primary` … `--info` | Colors |
| `sl-ce-button--xs` … `--xl` | Sizes |
| `sl-ce-button--rounded` / `--pill` / `--square` | Shapes |
| `sl-ce-button--full` | Full width |
| `sl-ce-button-group` | Flex wrap row |

## Spec

- [01 — Tabs](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/01-tabs.md) · [rich](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/01-tabs.html)
- [02 — Accordion](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/02-accordion.md) · [rich](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/02-accordion.html)
- [03 — Table](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/03-table.md) · [rich](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/03-table.html)
- [04 — Traits](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/04-traits.md) · [rich](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/04-traits.html)
- [05 — Expand](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/05-expand.md) · [rich](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/05-expand.html)
- [06 — Badge](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/06-badge.md) · [rich](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/06-badge.html)
- [07 — Button](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/07-button.md) · [rich](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/07-button.html)

## Local

Open `index.html` in a browser, or serve the folder with any static file server.
