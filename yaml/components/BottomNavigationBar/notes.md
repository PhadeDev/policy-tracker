# Bottom Navigation Bar Component (cmpBottomNavigation)

**Source URL:** https://www.powerappsui.com/components/bottom-navigation-bar
**Raw YAML source:** https://www.powerappsui.com/components/bottom-navigation-bar.yaml
**Fetched:** 2026-07-14

## What it does

A mobile-style bottom tab bar (`cmpBottomNavigation`) that renders a horizontal `Gallery` of navigation items driven by the `Items` input table (each row has `Icon`, `Label`, `Badge` fields). Each tab shows an icon (theme/state-colored via hex codes), an optional label (controlled by `ShowLabels`: always/selected/never), an optional notification badge count, and a selection indicator in one of four styles (`ActiveIndicator`: pill/dot/underline/bar). Tapping a tab updates `SelectedItemIndex` and fires `OnSelect`.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, `AccessAppScope: true`, with Input/Output/Event `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `Gallery@2.15.0` (Horizontal) — renders the `Items` table as tabs
- `GroupContainer@1.5.0` — per-tab container, active-indicator container, badge container
- `Image@2.2.3` — per-tab icon (inline SVG via data URI, colored by `ActiveColorHex`/`InactiveColorHex`)
- `Text@0.0.51` — tab label, badge count text
- `Classic/Button@2.2.0` — per-tab transparent hit target

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `Items` | Table | Navigation tabs; rows have `Icon`, `Label`, `Badge` fields |
| `SelectedIndex` | Number | Currently selected tab index (1-based, default 1) |
| `ShowLabels` | Text | `"always"`, `"selected"`, or `"never"` |
| `ActiveIndicator` | Text | `"pill"`, `"dot"`, `"underline"` (default), or `"bar"` |
| `ActiveColor` | Color | Color for selected item/indicator (default `RGBA(59,130,246,1)`) |
| `ActiveColorHex` | Text | Hex color (no `#`) for the selected tab's icon SVG (default `"3B82F6"`) |
| `InactiveColorHex` | Text | Hex color (no `#`) for unselected tab icons (default `"6B7280"`) |
| `BadgeColor` | Color | Badge background color (default `RGBA(220,38,38,1)`) |
| `Theme` | Text | `"Light"` or `"Dark"` |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| `SelectedItemIndex` | Number | Index of the currently selected/last-clicked tab |

## Events

- `OnSelect` — fires when any tab is tapped

## Setup requirements

- Modern controls and themes should be enabled in Studio (Settings > Updates > Preview > Modern controls and themes).
- Mixes modern versioned controls (`Gallery`, `GroupContainer`, `Image`, `Text`) with one classic control (`Classic/Button@2.2.0`) used as an invisible per-tab hit target.
