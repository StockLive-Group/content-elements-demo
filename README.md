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
  content-elements.css    # shippable @import entry
  demo.css                # preview chrome only (not shipped)
partials/
  tabs/default.html
  accordion/{default,bordered,separated,flush,ghost,chevron-left}.html
  table/{default,caption,striped,square,sticky,footer,cards,empty}.html
index.html                # stacked variants gallery
lot-tabs.html             # lot-row context
lot-accordion.html        # lot-row context (bordered)
lot-table.html            # lot-row context (ASBV table)
lot-combined.html         # lot-row with tabs + accordion + table
```

## Pages

| Page | What |
|------|------|
| [Variants](https://stocklive-group.github.io/sl-ce/) | All variants stacked |
| [Lot · Tabs](https://stocklive-group.github.io/sl-ce/lot-tabs.html) | Auction lot shell + tabs |
| [Lot · Accordion](https://stocklive-group.github.io/sl-ce/lot-accordion.html) | Auction lot shell + bordered accordion |
| [Lot · Table](https://stocklive-group.github.io/sl-ce/lot-table.html) | Auction lot shell + ASBV table |
| [Lot · Combined](https://stocklive-group.github.io/sl-ce/lot-combined.html) | Auction lot shell + tabs + accordion + pedigree table |

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

Sorting, selection, bulk actions, and pagination need JS — out of scope for Content Elements.

## Spec

- [01 — Tabs](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/01-tabs.md)
- [02 — Accordion](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/02-accordion.md)
- [03 — Table](https://kuickr.co/hugh-gordon/inbox/plans/content-elements/03-table.md) (Inbox draft — move into ctp-docs when ready)

## Local

Open `index.html` in a browser, or serve the folder with any static file server.
