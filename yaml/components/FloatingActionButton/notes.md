# Floating Action Button (FAB)

Source: https://www.powerappsui.com/components/floating-action-button
(YAML pulled from the site's static asset at
https://www.powerappsui.com/components/cmpFloatingActionButton.yaml; internal component name is
`cmpFloatingActionButton`)

## What it does

A Material-Design-style floating action button. Supports a plain icon-only "standard" style or an
"extended" style (icon + text label on the button itself), three sizes, disabled state, and a
Material 3 "speed dial" mode: tapping the FAB with `SpeedDialItems` populated opens a vertical
stack of labeled action pills (each with its own icon/label/color) instead of firing `OnSelect`
directly. Tracks open/closed state and the last-tapped speed dial item as outputs.

## This is a real Power Apps Component

This is a genuine Canvas Component (`ComponentDefinitions: cmpFloatingActionButton`,
`DefinitionType: CanvasComponent`, `AccessAppScope: true`) with declared Input/Output/Event custom
properties — not a screen-level control group. It must be created via Power Apps Studio's
**Insert > Custom > New Component** (or imported as a component in Code View), not pasted onto a
screen directly. This project has not used the Components workflow before; every prior control set
has been screen-level groups, so this requires the different import/build path.

## Control types used

- `GroupContainer@1.5.0` — FAB root container, speed-dial row/pill containers, body wrapper
- `Classic/Button@2.2.0` — backdrop tap-to-close button, speed-dial hit targets, main FAB hit
  target
- `Gallery@2.15.0` — renders `SpeedDialItems` as the speed-dial pill list
- `Text@0.0.51` — speed-dial pill labels, extended-mode FAB label
- `Image@2.2.3` — speed-dial pill icons, main FAB icon
- `Rectangle@2.3.0` — disabled-state overlay

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `Color` | Color | FAB background color (default blue `RGBA(37,99,235,1)`) |
| `Disabled` | Boolean | Disables the FAB |
| `Icon` | Text | Named icon or SVG string; named set includes Plus, Edit, Share, Camera, Search, Check, X, Heart, Star, Upload, Download, Settings, Menu, Send (default `"Plus"`) |
| `IconColor` | Text | Icon/text color as hex string (default `"#FFFFFF"`) |
| `ItemSize` | Number | Height of each speed dial pill in px (min 44 recommended for touch; default `52`) |
| `Label` | Text | Text shown on the FAB when `Style = "extended"` (default `"New Report"`) |
| `Size` | Text | `"small"` (40px), `"regular"` (56px), `"large"` (96px) |
| `SpeedDialItems` | Table | `{Icon, Label, Color (optional)}` rows — speed dial actions; Material 3 recommends 3-6 items |
| `Style` | Text | `"standard"` = icon only, `"extended"` = icon + label on the FAB itself |
| `Theme` | Text | `"Light"` or `"Dark"` |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| `IsOpen` | Boolean | Whether the speed dial menu is currently open |
| `SelectedSpeedDialItem` | Record | The last-selected speed dial item record |

## Events

| Event | Purpose |
|---|---|
| `OnSelect` | Fires when the FAB itself is tapped, but only when `SpeedDialItems` is empty |
| `OnSpeedDialSelect` | Fires when a speed dial item is tapped |

## Setup requirements

- No explicit "Modern controls" flag; this component uses classic `Text@0.0.51`, `Classic/Button`,
  `Gallery`, `GroupContainer`, `Image`, `Rectangle` — all standard/classic Studio controls.
- Minimum 44px recommended for `ItemSize` (touch target sizing) per Material Design guidance
  embedded in the property description.
- `SelectedSpeedDialItem`/`IsOpen` are read via component `Properties` bindings from an
  internal selection variable — behaves like the Segmented Control's internal-state pattern.
