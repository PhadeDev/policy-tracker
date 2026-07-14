# Notification Badge Component (Badge)

**Source URL:** https://www.powerappsui.com/components/badge
**Raw YAML source:** https://www.powerappsui.com/components/badge.yaml
**Fetched:** 2026-07-14

## What it does

A small bell-icon notification indicator (`Badge`) sized `Size` x `Size` px (default 45x45). Shows a theme-aware rounded-square background container with a bell SVG icon (custom SVG accepted via `IconSvg`, custom stroke color via `IconColor`). When `HasNotifications` is true and `BadgeCount` is 0, a small red pulsing dot animates in the top-right corner (via a `Timer`-driven opacity/size animation). When `BadgeCount` > 0 (or `HasNotifications` is true), a solid count badge is shown instead, capped at displaying "99+". Tapping the whole icon fires `OnSelect`.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, `AccessAppScope: true`, with Input/Event `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `GroupContainer@1.5.0` (`ManualLayout`) — outer bell container, pulse-dot container, count-badge container
- `Image@2.2.3` — bell icon (data-URI SVG, default bell or custom `IconSvg`)
- `Timer@2.1.0` — drives the pulse animation (1000ms, repeating, hidden)
- `Text@0.0.51` — the numeric count label inside the badge
- `Classic/Button@2.2.0` — transparent full-size overlay hit target that fires `OnSelect`

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `BadgeColor` | Color | Background color of the count badge (default `RGBA(220,38,38,1)`) |
| `BadgeCount` | Number | Number to display; caps display at "99+"; `0` combined with `HasNotifications` triggers pulse-only mode |
| `HasNotifications` | Boolean | Controls whether the badge/pulse is shown at all |
| `HoverText` | Text | Tooltip / accessible label text |
| `IconColor` | Text | Hex color override (e.g. `"#2563EB"`) for the SVG icon stroke |
| `IconSvg` | Text | Raw SVG markup; falls back to a default bell icon if blank |
| `Size` | Number | Component width and height in pixels (default 45) |
| `Theme` | Text | `"Light"` or `"Dark"` |

## Output properties

None.

## Events

- `OnSelect` — fires when the bell icon is tapped

## Setup requirements

- Modern controls and themes should be enabled in Studio (Settings > Updates > Preview > Modern controls and themes).
- Mixes modern versioned controls (`GroupContainer`, `Image`, `Text`, `Timer`) with one classic control (`Classic/Button@2.2.0`) used purely as an invisible hit-target overlay.
