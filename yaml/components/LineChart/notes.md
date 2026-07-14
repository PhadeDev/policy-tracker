# Line Chart

Source: https://www.powerappsui.com/components/line-chart
(YAML pulled from the site's static asset at https://www.powerappsui.com/components/line-chart.yaml)

## What it does

A single-series line chart rendered entirely as an inline SVG data URI inside one `Image` control
— there is no native Power Apps charting control involved. The component builds the SVG string at
runtime with Power Fx (`With`/`ForAll`/`Concat`/string concatenation), including axis lines,
optional dashed grid lines, a smoothed (Catmull-Rom-style cubic Bezier) line path, a gradient fill
under the line, optional point value labels, and an optional CSS `@keyframes`-based draw-in /
fade-in animation. Handles the "no data" case with a placeholder message.

## This is a real Power Apps Component

This is a genuine Canvas Component (`ComponentDefinitions: cmpLineChart`, `DefinitionType:
CanvasComponent`, `AccessAppScope: true`) with declared Input custom properties — not a
screen-level control group. It must be created via Power Apps Studio's **Insert > Custom > New
Component** (or imported as a component in Code View), not pasted onto a screen directly. This
project has not used the Components workflow before; every prior control set has been
screen-level groups, so this requires the different import/build path.

## Control types used

- `Image@2.2.3` — the only child control; its `Image` property is set to a computed
  `"data:image/svg+xml," & EncodeUrl(...)` string containing the full chart SVG

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `Animate` | Boolean | Enable draw-in/fade-in animation (default `true`) |
| `AnimationDuration` | Number | Animation length in seconds (default `1.2`) |
| `ChartData` | Table | `{x, y, label}` rows — the plotted series (default is a 12-month sample) |
| `ChartTitle` | Text | Title text shown in the top-left of the chart |
| `LineColor` | Text | Hex color string for the line/gradient (default `"#3B82F6"`) |
| `ShowGrid` | Boolean | Show horizontal dashed grid lines |
| `ShowPointLabels` | Boolean | Show a value label above each data point |
| `ShowTitle` | Boolean | Show/hide the chart title |
| `Theme` | Text | `"Light"` or `"Dark"` — swaps background/axis/text colors |
| `XLabelMode` | Text | `"Month"` (formats index as a month name), `"FromLabel"` (uses row `label`), or index number |

## Output properties / Events

None — this component has no declared Output or Event custom properties.

## Setup requirements

- No "Modern controls" dependency — only uses classic `Image` control.
- All chart rendering is SVG generated in Power Fx and URL-encoded into the `Image` property; no
  external chart library, PCF control, or image asset is required.
- Component `Height`/`Width` are fixed formulas (`=400`, `=App.Width`) at the component level —
  adjust `Height`/`Width` properties on the instance if a different size is needed (viewBox inside
  the SVG is fixed at 700x320 and will scale to fit).
