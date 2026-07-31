# Content Elements demo

Public preview of StockLive **Content Elements** (`sl-ce`) — CSS-only components for Froala / Nextlot.

**Live:** https://stocklive-group.github.io/content-elements-demo/

## Structure

Mirrors the Kuickr plan architecture (`plans/content-elements/`):

```text
css/
  tokens.css              # :root { --sl-ce-* }
  tabs.css                # .sl-ce-tabs*
  accordion.css           # .sl-ce-accordion* + variants
  content-elements.css    # shippable @import entry
  demo.css                # preview chrome only (not shipped)
partials/
  tabs/default.html
  accordion/{default,bordered,separated,flush,ghost,chevron-left}.html
index.html                # stacked variants gallery
lot-tabs.html             # lot-row context
lot-accordion.html        # lot-row context (bordered)
```

## Pages

| Page | What |
|------|------|
| [Variants](https://stocklive-group.github.io/content-elements-demo/) | All accordion variants + tabs, stacked |
| [Lot · Tabs](https://stocklive-group.github.io/content-elements-demo/lot-tabs.html) | Auction lot shell + tabs |
| [Lot · Accordion](https://stocklive-group.github.io/content-elements-demo/lot-accordion.html) | Auction lot shell + bordered accordion |

## Accordion variants

| Class | Look |
|-------|------|
| `sl-ce-accordion--default` | Dividers only |
| `sl-ce-accordion--bordered` | Outer border (primary) |
| `sl-ce-accordion--separated` | Gapped item cards |
| `sl-ce-accordion--flush` | No borders |
| `sl-ce-accordion--ghost` | Hairline / transparent |
| `sl-ce-accordion--chevron-left` | Chevron on the left (combinable) |

## Spec

- [01 — Tabs](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/01-tabs.md)
- [02 — Accordion](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/02-accordion.md)

## Local

Open `index.html` in a browser, or serve the folder with any static file server.
