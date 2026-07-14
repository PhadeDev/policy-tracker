# Dialog Component (cmpEnhancedDialog)

**Source URL:** https://www.powerappsui.com/components/dialog
**Raw YAML source:** https://www.powerappsui.com/components/cmpEnhancedDialog.yaml
**Fetched:** 2026-07-14 (re-fetched for consistency with the rest of this batch; same extraction method as the other 4 components)

## What it does

A full-screen modal dialog component (`cmpEnhancedDialog`) covering `App.Width` x `App.Height` with a blurred background overlay (`HtmlViewer` using `backdrop-filter: blur`), a centered dialog card, and three dialog "types": Alert (centered icon+message, single button row), Confirmation (header+message+two buttons), and Form (adds a required/optional multiline text input). Header shows a colored icon badge (Warning/Error/Success/Info/Restricted/NoAccess/Danger/None, each an inline SVG, theme-aware stroke color) plus a title. Footer renders a dynamic horizontal `Gallery` of buttons driven by the `Buttons` input table, with per-button `ButtonType` (Primary/Secondary/Outline/Subtle/Transparent) mapped to `ButtonCanvas.Appearance`. Primary buttons in Form mode auto-disable until required text input is filled. Optional close (X) icon and click-outside-to-close, both gated by `ShowCloseIcon`.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, `AccessAppScope: true`, with Input/Output/Event `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `GroupContainer@1.4.0` (`ManualLayout` and `AutoLayout` variants) — main wrapper, dialog card, header, footer, content area, icon circle, close-icon container
- `HtmlViewer@2.1.0` — background blur overlay
- `Button@0.0.45` — background click-to-close hit target, dynamic footer buttons (Gallery template)
- `Gallery@2.15.0` (`Horizontal` variant) — renders the `Buttons` input table as footer buttons
- `Rectangle@2.3.0` — divider lines (header/footer)
- `Text@0.0.51` — header title, message body, required-field label/asterisk
- `TextInput@0.0.54` (Multiline mode) — the Form-type text input
- `Image@2.2.3` — header status icon (data-URI SVG switched by `IconName`)
- `Classic/Icon@2.5.0` — close ("Cancel") icon
- `Classic/Button@2.2.0` — invisible hit target behind the close icon

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `DialogType` | Text | `"Alert"`, `"Confirmation"`, or `"Form"` — drives layout/sizing/button justification |
| `Buttons` | Table | `Label`, `ButtonType`, `Action`, `Visible` per footer button |
| `HeaderText` | Text | Dialog title |
| `MessageText` | Text | Body message text |
| `PlaceholderText` | Text | Placeholder/label text for the Form text input |
| `IsTextInputRequired` | Boolean | Marks the Form text input required; disables Primary buttons until filled |
| `IconName` | Text | `Warning`/`Error`/`Success`/`Info`/`Restricted`/`NoAccess`/`Danger`/`None` header icon |
| `DialogHeight` | Number | Fixed height in px; `0` = auto-size to content |
| `DialogWidth` | Number | Fixed width in px; `0` = auto width per `DialogType` (Alert 380 / Confirmation 400 / Form 500, capped to `Parent.Width - 32`) |
| `BorderRadius` | Number | Corner radius for the dialog card and footer |
| `OverlayBlurColor` | Text | Background overlay color as a CSS-style RGBA string, e.g. `"RGBA(0, 0, 0, 0.4)"` |
| `ShowCloseIcon` | Boolean | Shows the X close icon and enables click-outside-to-dismiss |
| `Theme` | Text | `"Light"` or `"Dark"` |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| `ContentHeight` | Number | Live height of the Form content area (for callers needing to react to size) |
| `ContentWidth` | Number | Live width of the Form content area |
| `InputText` | Text | Current value of the Form text input |
| `SelectedButton` | Record | The `Buttons` row (Label/ButtonType/Action/Visible) of whichever button was last selected |

## Events

- `OnButtonSelect` — fires when any footer button is tapped; read `SelectedButton` to see which one
- `OnCloseSelect` — fires when the close (X) icon or background overlay is tapped (only when `ShowCloseIcon` is true)

## Setup requirements

- Modern controls and themes should be enabled in Studio (Settings > Updates > Preview) — component mixes modern versioned controls (`GroupContainer`, `Gallery`, `TextInput`, `Text`, `Button`, `Image`, `Rectangle`) with two classic controls (`Classic/Icon@2.5.0`, `Classic/Button@2.2.0`).
- The component's own `Height`/`Width` are bound to `App.Height`/`App.Width`, i.e. it is meant to be placed once and stretched to fill the whole screen so the overlay covers everything — not sized like a normal small control.
- `HtmlViewer`'s `backdrop-filter: blur` depends on the host browser/webview supporting that CSS property.
