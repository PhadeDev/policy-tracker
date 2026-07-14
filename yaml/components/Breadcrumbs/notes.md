# Breadcrumbs Component

**Source URL:** https://www.powerappsui.com/components/breadcrumbs
**Raw YAML source:** https://www.powerappsui.com/components/breadcrumbs.yaml
**Fetched:** 2026-07-14

## What it does

A horizontal breadcrumb navigation bar (`cmpBreadcrumbs`). Renders up to 5 explicit item slots (`cntItem1`-`cntItem5`) plus an ellipsis/"show all" collapse control when the item count exceeds `Config.MaxItems`. Each item can show a leading icon, a text label, and a trailing "append" icon, separated by a configurable separator glyph (chevron/slash/dot). Supports a light/dark theme, a distinct color for the current (last) item vs. earlier link items, and an optional "back button" mode that replaces the whole trail with a single Back label/icon. Label widths are computed precisely using a per-character width lookup table (`CharWidths`) rather than relying on native auto-width, so text doesn't clip or overlap.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, with Input/Output/Event `CustomProperties`), not a screen-level control group. It must be installed via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code), then the YAML pasted in — it is not a plain screen paste. This project has not used the real Components workflow before; this is one of the first true Components brought in.

## Control types used

- `GroupContainer@1.4.0` (both `AutoLayout` and `ManualLayout` variants) — item wrappers, content rows, ellipsis container
- `Image@2.2.3` — icons, append icons, separators (data-URI SVGs, color-substituted at runtime)
- `Label@2.5.1` — item text labels, ellipsis label
- `Classic/Button@2.2.0` — invisible full-size hit targets over each item and the ellipsis, for click/tap

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `Items` | Table | Breadcrumb entries: `id`, `label`, optional `icon` (SVG string or data URI), optional `appendIcon` |
| `Config` | Record | Behavior: `Theme` ("light"/"dark"), `SeparatorIcon` ("chevron"/"slash"/"dot"), `Gap`, `Size` (font/icon size), `MaxItems` (0 = no collapse), `ItemsBeforeCollapse`, `ItemsAfterCollapse`, `ActiveLastItem`, `BackButton` (bool) |
| `CurrentColor` | Text | Hex color for the last/current breadcrumb item |
| `LinkColor` | Text | Hex color for earlier, clickable breadcrumb items |
| `Icons` | Record | Inline SVG strings for `Home`, `Chevron`, `Slash`, `Dot`, `Back`, `ChevronDown` |
| `CharWidths` | Table | Per-character relative width lookup (`CharFont`, `CharWeight`, `Char`, `Size`) for Segoe UI Semibold, used to size labels precisely |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| `SelectedItem` | Record | The full record (id/label/icon/appendIcon) of whichever breadcrumb item was last clicked |

## Events

- `OnSelect` — fires when any breadcrumb item (or the ellipsis-expanded item) is clicked
- `OnBack` — fires when the component is in `Config.BackButton = true` mode and the back control is clicked

## Setup requirements

- Modern controls and themes should be enabled in Studio (Settings > Updates > Preview) since the component uses modern control variants (`GroupContainer`, `Label`, `Image` versioned controls) alongside one classic control (`Classic/Button@2.2.0` for hit targets).
- Font dependency: Segoe UI Semibold — the `CharWidths` table is tuned to that specific font/weight; changing font without updating `CharWidths` will make label widths wrong.
- Component supports up to 5 visible breadcrumb items in this version (item slots are hardcoded 1-5); collapsing beyond that relies on `Config.MaxItems`/ellipsis, not additional slots.
