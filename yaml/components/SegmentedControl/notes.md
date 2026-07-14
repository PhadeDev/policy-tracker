# Segmented Control

Source: https://www.powerappsui.com/components/segmented-control
(YAML pulled from the site's static asset at https://www.powerappsui.com/components/segmented-control.yaml)

## What it does

A pill-style segmented control for choosing between mutually exclusive options, similar to an
iOS/macOS segmented picker. Options render as text, icons, or both (icon + label, side by side or
stacked), inside a rounded pill "track" with a highlighted background behind the currently
selected option. Selection state is tracked internally via a component-local variable
(`_scSelected`) and surfaced through the `SelectedValue` output/property.

## This is a real Power Apps Component

This is a genuine Canvas Component (`ComponentDefinitions: cmpSegmentedControl`, `DefinitionType:
CanvasComponent`) with declared Input/Output/Event custom properties — not a screen-level control
group. To use it, it must be created via Power Apps Studio's **Insert > Custom > New Component**
(or imported as a component in Code View), not pasted onto a screen directly. This project has not
used the Components workflow before; every prior control set has been screen-level groups, so this
requires the different import/build path.

## Control types used

- `GroupContainer@1.5.0` (AutoLayout and ManualLayout variants) — outer track wrapper, per-option
  containers
- `Gallery@2.15.0` (Horizontal variant) — renders the option list from the `Options` table
- `Image@2.2.3` — per-option icon (inline SVG data URI, recolored via `Substitute` on
  `currentColor`)
- `ModernText@1.0.0` — per-option label text
- `Classic/Button@2.2.0` — invisible hit-target button per option, handles `OnSelect`

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `DefaultValue` | Text | Initial selected value (default `"list"`) |
| `Disabled` | Boolean | Disables the whole component |
| `DisplayMode` | Text | `"Text"`, `"Icons"`, or `"Both"` |
| `EqualWidth` | Boolean | Force all option buttons to equal width |
| `IconPosition` | Text | When `DisplayMode = "Both"`: `"Left"` or `"Top"` |
| `Options` | Table | `{Value, Label (optional), Icon (optional, SVG data-uri)}` rows defining the options |
| `Size` | Text | `"Small"`, `"Medium"`, `"Large"` |
| `Theme` | Text | `"Light"` or `"Dark"` |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| `SelectedValue` | Text | Currently selected option's `Value` |

## Events

| Event | Purpose |
|---|---|
| `OnChange` | Fires when the selection changes |

## Setup requirements

- No explicit "Modern controls required" note on the page, but the component does use
  `ModernText@1.0.0`, which requires the Modern controls / `ModernControlsEnabled` preview
  feature to be turned on in Studio (Settings > Upcoming features / Preview) for the control to
  render/paste correctly.
- Icons are supplied as inline SVG data URIs directly in the `Options` table (no external icon
  font/library dependency).
- Component sizing (`Height`, `Width`) is computed dynamically from `Size` and
  `DisplayMode`/`EqualWidth` via component-level `Properties` formulas — no manual sizing needed
  after placement.
