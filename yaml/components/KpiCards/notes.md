# KPI Cards

Source: https://www.powerappsui.com/components/kpi-cards
(YAML pulled from the site's static asset at https://www.powerappsui.com/components/cmpKPI.yaml;
internal component name is `cmpKPI`)

## What it does

A responsive gallery of KPI stat cards (metric value, label, icon, percent-change indicator,
optional sparkline). Supports five visual styles (`Standard`, `Compact`, `Minimal`, `Filled`,
`Chart`), a loading/skeleton-placeholder state, gallery transition animations (Pop/Push/None), and
a responsive column count driven by `ColumnsLayout` breakpoints keyed off `App.SizeBreakpoints`.
Icons are supplied as a bundled SVG library (`Icons` table with a `COLOR` placeholder token that
gets substituted per-card) rather than external image assets. Styling (colors, spacing, radius,
typography, thresholds) is centralized in one `StyleConfig` record rather than scattered across
individual properties.

## This is a real Power Apps Component

This is a genuine Canvas Component (`ComponentDefinitions: cmpKPI`, `DefinitionType:
CanvasComponent`, `AccessAppScope: true`) with declared Input/Event custom properties — not a
screen-level control group. It must be created via Power Apps Studio's **Insert > Custom > New
Component** (or imported as a component in Code View), not pasted onto a screen directly. This
project has not used the Components workflow before; every prior control set has been
screen-level groups, so this requires the different import/build path.

## Control types used

- `GroupContainer@1.5.0` — card wrapper, sparkline container, icon badge, etc.
- `Gallery@2.15.0` — renders the `Data` table as a grid of cards, with configurable transition
  effect and responsive column count
- `Classic/Button@2.2.0` — card click hit target (`OnCardClick`)
- `Image@2.2.3` — per-card icon (SVG string with `COLOR` substituted) and sparkline (inline SVG)
- `ModernText@1.0.0` — value/label/percent-change text

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `AnimationEffect` | Text | Gallery transition: `"Pop"` (scale in), `"Push"` (slide), `"None"` (instant); default `"Pop"` |
| `ColumnsLayout` | Record | `{Mobile, Tablet, Desktop, MobileBreakpoint, TabletBreakpoint, DesktopBreakpoint}` — responsive column counts and breakpoints, defaults read from `App.SizeBreakpoints` |
| `Data` | Table | The KPI rows: `{ID, Icon, Value, Label, PercentChange, PercentLabel, IconBg, IconColor, SparklineData ("comma,separated,numbers"), isHidden}` |
| `Icons` | Table | `{Name, SVG}` — bundled icon library; `SVG` contains a `COLOR` token replaced at render time |
| `IsLoading` | Boolean | When true, renders skeleton placeholder cards instead of `Data` |
| `Style` | Text | `"Standard"` (value + sparkline footer), `"Compact"` (dense, icon right), `"Minimal"` (dense, no icon), `"Filled"` (colored background), `"Chart"` (sparkline hero, no icon) |
| `StyleConfig` | Record | Centralized style tokens: `colors`, `space`, `radius`, `type` (typography sizes), `heights`, `abbreviateThreshold` |

## Output properties

None declared.

## Events

| Event | Purpose |
|---|---|
| `OnCardClick` | Fires when a KPI card is clicked; passes parameter `ClickedItem` (Record) — the clicked KPI's data row |

## Setup requirements

- Uses `ModernText@1.0.0`, which requires the Modern controls preview feature enabled in Studio
  (Settings > Upcoming features / Preview) to paste/render correctly.
- Sparklines are shown automatically only in `Standard` and `Chart` styles, driven by the
  `SparklineData` comma-separated string per row (empty string hides the sparkline for that card).
- Responsive column counts rely on `App.SizeBreakpoints` being configured in the app (falls back
  to 640/1024/1366 via `IfError` if not set).
- This is the largest of the six YAML files fetched in this batch (~52KB) — review carefully
  before first paste, and expect a noticeable delay in Studio while it parses.
