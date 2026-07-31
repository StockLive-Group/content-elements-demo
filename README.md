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
  expand.css              # .sl-ce-expand* (show more)
  content-elements.css    # shippable @import entry
  demo.css                # preview chrome only (not shipped)
partials/
  tabs/default.html
  accordion/{default,bordered,separated,flush,ghost,chevron-left}.html
  table/{default,caption,striped,square,sticky,footer,cards,empty,percentile}.html
index.html                # stacked variants gallery
lot-tabs.html             # lot-row context
lot-accordion.html        # lot-row context (bordered)
lot-table.html            # lot-row context (ASBV + percentile)
lot-combined.html         # lot-row with tabs + accordion + table
```

## Pages

| Page                                                                          | What                                                           |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------- |
| [Variants](https://stocklive-group.github.io/sl-ce/)                          | All variants stacked                                           |
| [Lot · Tabs](https://stocklive-group.github.io/sl-ce/lot-tabs.html)           | Auction lot shell + tabs                                       |
| [Lot · Accordion](https://stocklive-group.github.io/sl-ce/lot-accordion.html) | Auction lot shell + bordered accordion                         |
| [Lot · Table](https://stocklive-group.github.io/sl-ce/lot-table.html)         | Auction lot shell + ASBV + percentile table                    |
| [Lot · Combined](https://stocklive-group.github.io/sl-ce/lot-combined.html)   | Composed lot: tables in tabs/accordion + expand + percentile   |

## Accordion variants

| Class                           | Look                             |
| ------------------------------- | -------------------------------- |
| `sl-ce-accordion--default`      | Dividers only                    |
| `sl-ce-accordion--bordered`     | Outer border (primary)           |
| `sl-ce-accordion--separated`    | Gapped item cards                |
| `sl-ce-accordion--flush`        | No borders                       |
| `sl-ce-accordion--ghost`        | Hairline / transparent           |
| `sl-ce-accordion--chevron-left` | Chevron on the left (combinable) |

## Table variants

| Class                               | Look                                                              |
| ----------------------------------- | ----------------------------------------------------------------- |
| `sl-ce-table--bordered`             | Outer + cell borders (primary)                                    |
| `sl-ce-table--rounded` / `--square` | Corner shape                                                      |
| `sl-ce-table--striped`              | Alternating row wash                                              |
| `sl-ce-table--striped-columns`      | Alternating column wash                                           |
| `sl-ce-table--hoverable`            | Row hover highlight                                               |
| `sl-ce-table--sticky-header`        | Sticky thead in scroll box                                        |
| `sl-ce-table--cards` / `--stack`    | Mobile responsive layouts (`data-label` on cells)                 |
| `sl-ce-table--percentile`           | Trait-profile bars (100← · 50 · →0) via `--sl-ce-percentile`      |
| `sl-ce-table--xs` … `--xl`          | Cell padding / type size                                          |

Bar tone modifiers on `.sl-ce-table__bar`: `--calving`, `--growth`, `--maternal`, `--milk`, `--fertility`, `--other`, `--carcase`, `--structure`, `--index`.

Sorting, selection, bulk actions, and pagination need JS — out of scope for Content Elements.

## Expand (show more)

| Class                    | Look                                    |
| ------------------------ | --------------------------------------- |
| `sl-ce-expand`           | Text “Show more / Show less” disclosure |
| `sl-ce-expand--bordered` | Bordered card shell                     |
| `sl-ce-expand--button`   | Pill button trigger                     |
| `sl-ce-expand--footer`   | Trigger sits under the revealed panel   |

Native `<details>` / `<summary>` — nest tables, accordion, or copy inside `.sl-ce-expand__panel`.

## Spec

- [01 — Tabs](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/01-tabs.md)
- [02 — Accordion](https://kuickr.co/stocklive/ctp-docs/plans/content-elements/02-accordion.md)
- [03 — Table](https://kuickr.co/hugh-gordon/inbox/plans/content-elements/03-table.md) (Inbox draft — move into ctp-docs when ready)

## Local

Open `index.html` in a browser, or serve the folder with any static file server.
