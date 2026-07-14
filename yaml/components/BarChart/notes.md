# SVG Bar Chart (Stacked Bar Chart)

Source: https://www.powerappsui.com/components/bar-chart
(YAML pulled from the site's static asset at https://www.powerappsui.com/components/bar-chart.yaml;
internal component name is `cmpStackedBarChart`)

## What it does

A two-series stacked bar chart rendered entirely as an inline SVG data URI inside one `Image`
control — no native Power Apps charting control is involved. For each x-axis label, it stacks two
series' values (looked up from `ChartData` by `Label`+`Series`) into one bar, colored per
`SeriesConfig`. Supports a title, legend, horizontal grid lines, optional stacked-total value
labels above each bar, and animated bar grow-in (SVG `<animate>` elements, not CSS). Handles the
"no data" case with a placeholder message.

## This is a real Power Apps Component

This is a genuine Canvas Component (`ComponentDefinitions: cmpStackedBarChart`, `DefinitionType:
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
| `Animate` | Boolean | Enable animated bar grow-in (default `true`) |
| `AnimationDuration` | Number | Animation length in seconds (default `0.8`) |
| `ChartData` | Table | `{Label, Series, Value}` rows — the raw data points (default sample: Mobile/Desktop by month) |
| `ChartTitle` | Text | Title text shown top-left |
| `Labels` | Table | `{Label}` rows defining the x-axis category order (separate from `ChartData` so categories can be controlled independently) |
| `SeriesConfig` | Table | `{Series, Color}` rows — exactly two series are supported (bottom/top of stack) |
| `ShowGrid` | Boolean | Show horizontal grid lines |
| `ShowLegend` | Boolean | Show the series legend top-right |
| `ShowTitle` | Boolean | Show/hide the chart title |
| `ShowValues` | Boolean | Show total value label above each stacked bar |
| `Theme` | Text | `"Light"` or `"Dark"` |

## Output properties / Events

None — this component has no declared Output or Event custom properties.

## Setup requirements

- No "Modern controls" dependency — only uses classic `Image` control.
- All chart rendering is SVG generated in Power Fx and URL-encoded into the `Image` property; no
  external chart library, PCF control, or image asset is required.
- Only supports exactly **two** series per bar (`SeriesConfig` indexes 1 and 2 explicitly via
  `Index(SeriesCfg, 1)` / `Index(SeriesCfg, 2)`) — a third row in `SeriesConfig` would be ignored.
- Component `Height`/`Width` are fixed formulas (`=400`, `=App.Width`) at the component level;
  internal SVG viewBox is fixed at 700x320.
