# File Upload Component

**Source URL:** https://www.powerappsui.com/components/file-upload
**Raw YAML source:** https://www.powerappsui.com/components/file-upload.yaml
**Fetched:** 2026-07-14

## What it does

A file-upload dialog panel (`cmpFileUpload`) built from a dashed-border drop zone (click-to-select via an invisible full-size `Button` layered over a native `Attachments` control), a vertical `Gallery` list of selected files showing a file-type icon (PDF/XLSX/PPTX/TXT/DOCX/MSG/generic, each an inline SVG matched by filename extension), file name, a static "Ready" status label, and a per-row remove icon. Duplicate file names are detected and skipped with a warning `Notify()`. Footer has Cancel (clears selection) and Save buttons; Save fires the `OnSave` event. Note: it does not actually perform any SharePoint/Power Automate upload itself — it only stages `Attachments` selections into a local collection (`var_uploadFile_listeAttachment`) and exposes `OnSave` for the caller to wire up the actual upload logic.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, with Input/Event `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `Text@0.0.51` — header, helper text, click-to-select/drag-drop labels, file name/status labels in the gallery
- `GroupContainer@1.4.0` (`ManualLayout` and `AutoLayout` variants) — drop zone, icon circle, file card, icon background, footer action row
- `Classic/Icon@2.5.0` — upload icon, generic document icon (hidden fallback), remove ("Cancel") icon
- `Button@0.0.45` — invisible click-to-select hit target, Cancel button, Save button
- `Attachments@2.3.0` — native attachments picker control (invoked via `Select()`, styled invisible/transparent)
- `Image@2.2.3` — per-file-type icon (data-URI SVG switched by file extension)
- `Gallery@2.15.0` (`Vertical` variant) — list of selected files

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `HeaderText` | Text | Header text shown at the top of the panel (default "Upload Documents") |
| `MaxFileSize` | Number | Maximum file size in MB (default 10) — declared as an input but not actually enforced anywhere in the formulas in this version |
| `MaxFiles` | Number | Maximum number of files, bound to `Attachments.MaxAttachments` (default 3) |

## Output / Event properties

| Property | Kind | Purpose |
|---|---|---|
| `OnSave` | Event | Fires when the Save button is clicked; caller is expected to read the staged files from wherever they wire up the actual upload (component itself does not expose an Output property listing the files) |

## Setup requirements

- No SharePoint connector or Power Automate flow is wired into this component itself — despite an earlier page-preview description mentioning SharePoint/DocumentLibrary properties, the actual YAML has none of that; it is a local staging UI only (`Attachments` + a local collection). Any real upload destination (SharePoint, Dataverse, etc.) must be built by the consumer using `OnSave`.
- Depends on a component-scoped collection variable `var_uploadFile_listeAttachment` created inside `OnAddFile`/`Reset` logic — this is created automatically at runtime via `Collect()`, no manual setup needed, but be aware it's a global-style collection name, not a component-local variable.
- `MaxFileSize` is present as an input property but is not enforced by any validation logic in the current YAML — if size limiting matters, it needs to be added.
