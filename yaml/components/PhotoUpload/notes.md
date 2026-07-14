# Photo Upload

Source: https://www.powerappsui.com/components/photo-upload
YAML source (fetched directly, same file served by the page's "Copy YAML" button): https://www.powerappsui.com/components/cmpPhotoUpload.yaml

## What it is

This is a genuine Power Apps **Component** (`ComponentDefinitions: cmpPhotoUpload`, `DefinitionType: CanvasComponent`), not a screen-level control group. It must be created via Studio's **Insert > Custom > New component**, then the Code View YAML pasted into that new component — it cannot be pasted directly onto a screen. This project (policy-tracker) has not used the real Component workflow before; everything else so far has been screen-level control groups, so treat this differently.

A photo capture/upload widget for mobile forms, split into a top and bottom labeled section (e.g. "Top Photos — Report Header" / "Bottom Photos — Report Footer"), each with a grid of numbered slots wrapping at `MaxColumns` per row. Tapping an empty slot opens the device camera; filled slots show the captured image with remove/replace controls. Includes optional Cancel/Save action buttons. Uses the `Camera` and `Badge` PCF-style controls in addition to standard canvas controls.

## Control types used

Badge@0.0.24, Button@0.0.45, Camera@2.4.0, Classic/Icon@2.5.0, Gallery@2.15.0, GroupContainer@1.5.0, Image@2.2.3, ModernText@1.0.0, Rectangle@2.3.0, Text@0.0.51

Note: uses `Camera` (device camera capture control), `Badge`, `Text@0.0.51`, `Button@0.0.45` and `ModernText` — these are newer/modern control variants and require modern controls to be enabled (see Setup below). `Camera` also requires the app to have camera permission granted on device/web.

## Input properties

| Property | Type | Purpose |
|---|---|---|
| TopSectionLabel | Text | Header label for the top photo section. |
| BottomSectionLabel | Text | Header label for the bottom photo section. |
| MaxTopPhotos | Number | Total photo slots in the top section — wraps at MaxColumns per row. |
| MaxBottomPhotos | Number | Total photo slots in the bottom section — wraps at MaxColumns per row. |
| MaxColumns | Number | Slots per row before wrapping (default 4). |
| MaxFileSizeMB | Number | Informational max file size shown in helper text only (not enforced). |
| ShowActions | Boolean | Show/hide the Cancel / Save buttons; set false when the host screen handles actions itself. |
| StyleConfig | Record | Color and radius tokens (colors + radius sub-records); override to theme the component. |

## Output properties

None declared as a plain Output property in CustomProperties — output is via two component-scoped collections (see below).

## Events

| Event | Purpose |
|---|---|
| OnSave | Fires when the Save Photos button is tapped. |

## Output collections (via AccessAppScope, not formal Output properties)

- `_cmpTopPhotos` — schema `{SlotIndex: Number, Photo: Image, Name: Text}`
- `_cmpBottomPhotos` — schema `{SlotIndex: Number, Photo: Image, Name: Text}`

## Setup requirements

- Modern controls must be enabled in Studio (Settings → Updates → Preview → "Modern controls and themes").
- Host screen's `OnVisible` must initialize (ClearCollect/Clear) both `_cmpTopPhotos` / `_cmpBottomPhotos` collections and four variables: `_cmpShowOverlay`, `_cmpOverlaySection`, `_cmpOverlayMode`, `_cmpViewSlot`.
- Import via Components tab → New component → Import from code, paste the YAML, save.
- `AccessAppScope: true` is set on the component definition (required for the app-scoped collections/variables above to work).
- Camera capture requires the app to have camera access permission.
