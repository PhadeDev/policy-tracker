# Pie Chart Component

**Source URL:** https://www.powerappsui.com/components/pie-chart
**Raw YAML source:** https://www.powerappsui.com/components/pie-chart.yaml
**Fetched:** 2026-07-14

## What it does

A pie/donut chart (`cmpPieChart`) rendered entirely as a single inline SVG data URI inside an `Image` control — there is no native charting control involved. It computes slice angles and paths from a data table using Power Fx math (`Cos`/`Sin`, cumulative angle sums via `ForAll`/`Filter`/`Sum`), builds the SVG `<path>` elements as strings, and optionally renders a legend and percentage/value labels. Supports light/dark theme, donut mode with adjustable ring thickness, optional CSS-keyframe entrance animation, and falls back to a "No data" placeholder SVG when the data table is empty or totals to zero.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, `AccessAppScope: true`, with Input `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `Image@2.2.3` — the sole visual control; displays a computed `data:image/svg+xml` URI containing the entire chart (slices, labels, legend, title)

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `ChartData` | Table | `Label`, `Value`, optional `Color` (hex) per slice |
| `ChartTitle` | Text | Title text shown above the chart |
| `DefaultColors` | Table | Fallback color palette (`Color`) cycled through when a data row has no `Color` |
| `Theme` | Text | `"Light"` or `"Dark"` |
| `ShowTitle` | Boolean | Show/hide chart title |
| `ShowLabels` | Boolean | Show in-slice labels (only for slices ≥5% of total) |
| `ShowPercentages` | Boolean | Show percentage values in labels/legend |
| `ShowValues` | Boolean | Show raw values in the legend |
| `ShowLegend` | Boolean | Show/hide the legend column |
| `DonutMode` | Boolean | Render as a donut (ring) instead of a solid pie |
| `DonutThickness` | Number | Ring thickness ratio, 0.3-0.9, when `DonutMode` is true |
| `Animate` | Boolean | Enable CSS keyframe grow/fade animation on slices and labels |
| `AnimationDuration` | Number | Animation duration in seconds |

## Output properties

None. No Output or Event custom properties are defined — this is a pure display component (no `OnSelect`/click interactivity captured as an output).

## Setup requirements

- No SharePoint/data connectors needed — purely computed SVG.
- Relies on inline `<style>`/CSS `@keyframes` inside the SVG for animation; this depends on the host rendering engine supporting SVG CSS animation (works in the Power Apps player/embedded browser).
- `Height`/`Width` on the component default to `400` / `App.Width` — designed to span full app width by default; adjust as needed for the target layout.
- No Modern-controls-specific requirement noted beyond the versioned `Image@2.2.3` control itself.
