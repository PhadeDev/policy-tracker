# Bar List

Source: https://www.powerappsui.com/components/bar-list
YAML source (fetched directly, same file served by the page's "Copy YAML" button): https://www.powerappsui.com/components/bar-list.yaml

## What it is

This is a genuine Power Apps **Component** (`ComponentDefinitions: cmpBarList`, `DefinitionType: CanvasComponent`), not a screen-level control group. It must be created via Studio's **Insert > Custom > New component**, then the Code View YAML pasted into that new component — it cannot be pasted directly onto a screen. This project (policy-tracker) has not used the real Component workflow before; everything else so far has been screen-level control groups, so treat this differently.

A horizontal bar-chart / ranked list widget (like a "top categories by value" list). Renders a gallery of rows, each with a label, optional subtitle, and a proportionally-sized colored bar with its value. Bar colors auto-cycle through a `Palette` table. Supports row tap selection, a loading skeleton state, and an empty-state message when data is empty or all rows are hidden.

## Control types used

Classic/Button@2.2.0, Gallery@2.15.0, GroupContainer@1.5.0, Image@2.2.3, Label@2.5.1, ModernText@1.0.0

Note: uses `ModernText`, which requires modern controls to be enabled (see Setup below).

## Input properties

| Property | Type | Purpose |
|---|---|---|
| Data | Table | Rows: `Label` (text), `Value` (number), `Subtitle` (text, optional), `isHidden` (boolean, optional). |
| EmptyMessage | Text | Shown when Data is empty or all rows are hidden. |
| IsLoading | Boolean | Renders skeleton placeholder rows instead of data. |
| Palette | Table | Auto-cycling bar colors; single `Color` column, hex strings without leading `#`. |
| ShowRowSubtitle | Boolean | Show the per-row Subtitle text beneath each label (increases row height). |
| StyleConfig | Record | Light/dark color tokens, spacing, type scale; override either theme block to customize. |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| SelectedItem | Record | The last tapped row record. |

## Events

| Event | Purpose |
|---|---|
| OnBarClick | Fires when a bar row is tapped. Parameter `ClickedItem` (Record): the row that was clicked, e.g. `{Label, BarValue, SubtitleSafe, BarColor}`. |

## Setup requirements

- Modern controls must be enabled in Studio (Settings → Updates → Preview → "Modern controls and themes") because the component uses `ModernText`.
- Import via Components tab → New component → Import from code, paste the YAML, save.
- `AccessAppScope: true` is set on the component definition.
