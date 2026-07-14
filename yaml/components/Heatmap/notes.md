# Heatmap Component

**Source URL:** https://www.powerappsui.com/components/heatmap
**Raw YAML source:** https://www.powerappsui.com/components/heatmap.yaml
**Fetched:** 2026-07-14

## What it does

A day-of-week × hour-of-day heatmap grid (`cmpHeatmap`), rendered entirely as a computed inline SVG data URI inside an `Image` control (no native charting control). For each (day, hour) cell it looks up a `Value` from the data table, buckets the intensity into one of 6 fixed indigo shades (or an "empty" color when there's no data for that cell), and draws colored rounded-rect cells in an SVG grid with row (time) and column (day) axis labels plus an optional title. Example data models "issues opened" counts by day/hour.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, `AccessAppScope: true`, with Input `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `Image@2.2.3` — the sole visual control; displays a computed `data:image/svg+xml` URI containing the full grid (cells, axis labels, title)

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `ChartData` | Table | `DayOfWeek` (0-6), `Hour`, `Value` — one row per populated cell; missing (day, hour) combos render as empty |
| `ChartTitle` | Text | Title text shown above the grid |
| `DayLabels` | Text | Comma-separated 7-value string for day-of-week column headers (e.g. `"M,T,W,T,F,S,S"`) |
| `TimeLabels` | Table | `Hour`, `Label` — defines which hour rows appear and their display label (e.g. `6` → `"6am"`) |
| `BaseColor` | Text | Base hex color for the heatmap's color ramp (declared but the actual 6-step ramp is hardcoded in the SVG formula, not driven off this value) |
| `EmptyColor` | Text | Hex color used for cells with no matching data row |
| `Theme` | Text | `"Light"` or `"Dark"` — affects background/title/label colors only |
| `ShowTitle` | Boolean | Show/hide the chart title |

## Output properties

None documented — pure display component, no Output or Event custom properties.

## Setup requirements

- No SharePoint/data connectors needed — purely computed SVG.
- Row/column count is driven by the number of rows in `TimeLabels` and entries in `DayLabels`; changing those directly resizes the SVG canvas (`vW`/`vH` formulas scale with counts).
- No Modern-controls-specific requirement noted beyond the versioned `Image@2.2.3` control itself.
- Note: `BaseColor` input exists but the intensity color ramp (`#C7D2FE` … `#3730A3`) is hardcoded via a `Switch` in the image formula, not actually derived from `BaseColor` — worth knowing if a re-theme is attempted by only changing that property.
