# Loading Overlay Component (cmpLoadingSpinner)

**Source URL:** https://www.powerappsui.com/components/loading-overlay
**Raw YAML source:** https://www.powerappsui.com/components/loading-overlay.yaml
**Fetched:** 2026-07-14

## What it does

A full-screen loading overlay (`cmpLoadingSpinner`) with a semi-transparent (optionally blurred) background and a centered card containing a spinner and a text message. Intended to be placed once, stretched to cover the whole screen, and shown/hidden (or driven by `Visible`) while an async operation runs; bind `LoadingText` to a variable for multi-step progress feedback.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, `AccessAppScope: true`, with Input `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `HtmlViewer@2.1.0` — background overlay (opacity + optional CSS `backdrop-filter` blur)
- `GroupContainer@1.4.0` (2 instances) — root wrapper and the centered card
- `Spinner@1.4.6` — the loading spinner control
- `Text@0.0.51` — the loading message label

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `LoadingText` | Text | Message displayed below the spinner (default `"Loading..."`) |
| `OverlayOpacity` | Number | Background overlay darkness, 0 (transparent) to 1 (solid black) (default `0.3`) |
| `ShowBlur` | Boolean | Apply CSS `backdrop-filter` blur to the overlay (default `true`) |
| `Theme` | Text | `"Light"` or `"Dark"` — controls card background and text color |

## Output properties

None — no output properties or events on this component.

## Setup requirements

- Modern controls and themes should be enabled in Studio (Settings > Updates > Preview > Modern controls and themes) — requires `Spinner@1.4.6` and `Text@0.0.51`.
- The component's overlay/card is meant to be stretched to cover the whole screen (like the Dialog component's overlay pattern already archived in this folder) — not sized like a normal small control.
- `HtmlViewer`'s `backdrop-filter: blur` depends on the host browser/webview supporting that CSS property.
