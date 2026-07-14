# Chips Component (cmpChips)

**Source URL:** https://www.powerappsui.com/components/chips
**Raw YAML source:** https://www.powerappsui.com/components/chips.yaml
**Fetched:** 2026-07-14

## What it does

A horizontal auto-wrapping row of "chip"/pill controls (`cmpChips`) driven by the `Items` input table (rows: `id`, `label`, `icon`, `color`, `variant`, `selected`). Supports filter-style chips (toggle selection, e.g. status/document-type filters) or other chip types via `ChipType`. Each chip can show a prepend icon and a check icon when selected. Includes a large embedded `CharWidths` lookup table (Segoe UI Semibold per-character widths) used to precisely measure label text for auto-sizing each pill. `Config` controls theme, wrapping, closability, disabled state, corner radius, gap, and max-visible-before-overflow; `CustomColors` lets the palette be overridden per semantic color (primary/secondary/success/warning/error) with hex strings.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, `AccessAppScope: true`, with Input/Output/Event `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `GroupContainer@1.5.0` (`AutoLayout` and other variants) — chip wrapper, per-chip containers
- `Label@2.5.1` — chip text labels
- `Image@2.2.3` — prepend/check icons (data-URI SVG)
- `Classic/Button@2.2.0` — per-chip hit target for selection/close

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `CharWidths` | Table | Character width lookup table (Segoe UI Semibold) used to size pills precisely |
| `ChipType` | Text | Chip behavior/style category (default `"filter"`) |
| `Color` | Text | Semantic color key, e.g. `"secondary"` (default) |
| `Config` | Record | `{Theme, Wrap, Closable, Disabled, Radius, Gap, MaxVisible}` — default `{Theme:"light",Wrap:false,Closable:true,Disabled:false,Radius:99,Gap:8,MaxVisible:10}` |
| `CustomColors` | Record | Theme-level color overrides as hex strings: `{primary, secondary, success, warning, error}` |
| `Icons` | Record | `{Prepend, Check}` — icon glyphs/SVG used for prepend and selected-check indicators |
| `Items` | Table | Chip data rows: `id`, `label`, `icon`, `color`, `variant`, `selected` |
| `Size` | Text | Chip size, e.g. `"default"`; affects component `Height` (x-small=38, small=44, default=50, large=56, x-large=62) |
| `Variant` | Text | Visual style, e.g. `"tonal"` (default) |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| `SelectedItem` | Record | The chip row last selected/toggled |

## Events

- `OnClose` — fires when a closable chip's close (x) is tapped
- `OnOverflow` — fires related to the overflow/"MaxVisible" behavior
- `OnSelect` — fires when a chip is tapped/toggled

## Setup requirements

- Modern controls and themes should be enabled in Studio (Settings > Updates > Preview > Modern controls and themes).
- Mixes modern versioned controls (`GroupContainer`, `Label`, `Image`) with one classic control (`Classic/Button@2.2.0`).
- The YAML is large (~137KB) primarily due to the embedded per-character `CharWidths` default table used for text-measurement-based pill sizing — this is expected, not an extraction error.
