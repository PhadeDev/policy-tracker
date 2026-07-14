# Table Component (cmpMyTable)

**Source URL:** https://www.powerappsui.com/components/data-table
**Raw YAML source:** https://www.powerappsui.com/components/data-table.yaml
**Fetched:** 2026-07-14

## What it does

A data-grid component (`cmpMyTable`) that can render its `Items` data source in one of three view modes (`ViewMode`: table / card / list — toggleable at runtime if `ShowViewToggle` is on) using a `TabList`/`TabListDataField` control pair plus a `Gallery` for the actual rows/cards. Supports status and priority badge pills (colors/keys configured via `StatusConfig`/`PriorityConfig` tables), optional progress-segment columns (rows need `CompletedSteps`/`TotalSteps` numeric fields), row hover effects, and a per-row context menu (`ContextMenuItems`, with `OnMenuItemSelect` output). Row selection is exposed via `SelectedItem`/`OnRowSelect`. Like the Chips component, it embeds a large `CharWidths` lookup table (Segoe UI Semibold) to precisely size pill/tag widths.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, `AccessAppScope: true`, with Input/Output/Event `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `Gallery@2.15.0` — row/card rendering
- `GroupContainer@1.5.0` — row containers, cells, badge/progress wrappers
- `Label@2.5.1` — cell text, badge text
- `Image@2.2.3` — icons (context menu item icons, sort/view-toggle icons)
- `Rectangle@2.3.0` — dividers, progress-segment bars
- `TabList@2.2.31` / `TabListDataField@1.5.0` — the table/card/list view-mode toggle
- `Classic/Button@2.2.0` — row/menu hit targets

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `CharWidths` | Table | Character width lookup table (Segoe UI Semibold); sizes list-view tag pills precisely |
| `ContextMenuItems` | Table | Per-row context menu items (`ItemKey`, `ItemDisplayName`, `ItemIconSvg`, ...) |
| `EnableHover` | Boolean | Enable row hover effects |
| `EnableMenu` | Boolean | Enable the per-row context menu |
| `Items` | Table | Data source for the table; progress columns require `CompletedSteps`/`TotalSteps` number fields on each row |
| `PriorityConfig` | Table | Priority badge config: `PriorityKey`, `Background`, `TextColor` per priority level |
| `ProgressActiveColor` | Text | Hex (no `#`) color for completed progress segments |
| `ProgressInactiveColor` | Text | Hex (no `#`) color for incomplete progress segments |
| `ShowHeader` | Boolean | Show column headers (table view only) |
| `ShowViewToggle` | Boolean | Show the table/card/list view-mode toggle buttons |
| `StatusConfig` | Table | Status badge config: `StatusKey`, `Background`, `TextColor` per status |
| `ThemeConfig` | Record | Theme/style record — container, header and other section styling (background/border/radius per section) |
| `ViewMode` | Text | `"table"`, `"card"`, or `"list"` (default `"table"`) |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| `SelectedItem` | Record | Currently selected row/item record |
| `SelectedMenuItem` | Record | The context-menu item record last selected |
| `SelectedViewMode` | Text | Current view mode: table, card, or list |

## Events

- `OnMenuItemSelect` — fires when a context-menu item is selected
- `OnRowSelect` — fires when a row/card/list item is selected
- `OnViewChange` — fires when the view mode toggle changes

## Setup requirements

- Modern controls and themes should be enabled in Studio (Settings > Updates > Preview > Modern controls and themes).
- Mixes modern versioned controls (`Gallery`, `GroupContainer`, `Label`, `Image`, `Rectangle`, `TabList`, `TabListDataField`) with one classic control (`Classic/Button@2.2.0`).
- Large component (~114KB YAML) — much of the size is the embedded `CharWidths` default table, same as the Chips component.
