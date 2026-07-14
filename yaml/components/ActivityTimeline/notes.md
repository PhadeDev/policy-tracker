# Activity Timeline

Source: https://www.powerappsui.com/components/activity-timeline
YAML source (fetched directly, same file served by the page's "Copy YAML" button): https://www.powerappsui.com/components/activity-timeline.yaml

## What it is

This is a genuine Power Apps **Component** (`ComponentDefinitions: cmpActivityTimeline`, `DefinitionType: CanvasComponent`), not a screen-level control group. It must be created via Studio's **Insert > Custom > New component**, then imported/pasted as Code View YAML into that new component — it cannot be pasted directly onto a screen. This project (policy-tracker) has not used the real Component workflow before; everything else so far has been screen-level control groups, so treat this differently.

A vertical audit-trail / activity feed. Renders a scrollable gallery of timeline entry cards with a colored dot marker, icon, title, description, author, and timestamp, connected by a vertical line. Supports a category filter dropdown, expandable "sub-detail" panels on certain entries (e.g. email To/CC/Priority/Message), loading skeleton state, and toolbar buttons for export/print. Card icon and dot color are derived automatically from each entry's `Category` via an `IconMap` lookup table.

## Control types used

Classic/Button@2.2.0, Gallery@2.15.0, GroupContainer@1.5.0, HtmlViewer@2.1.0, Image@2.2.3, Label@2.5.1, ModernCombobox@1.0.0, Rectangle@2.3.0

Note: uses `ModernCombobox`, which requires modern controls to be enabled (see Setup below).

## Input properties

| Property | Type | Purpose |
|---|---|---|
| CardHeight | Number | Height of a standard entry card with no sub-detail. Default 92. |
| FilterOptions | Table | `{Value:Text}` rows for the filter dropdown; values must match `Category` values in `Items`/`IconMap`. |
| HideHeader | Boolean | Hides the toolbar (filter, entry count, action buttons) when true. |
| IconMap | Table | Maps `Category` to icon type, dot color, and card icon color. Case-insensitive match; include a `Category="default"` fallback row. IconType values: comment, email, document, person, checkbadge, check, workflow, folder. |
| IsLoading | Boolean | Shows skeleton placeholder cards instead of data. |
| Items | Table | Audit trail entries. Schema: EventTitle, EventDescription, Author, DisplayDateTime, Category, HasSubDetail, SubTo, SubCC, SubPriority (High/Normal), SubMessage. IconType is derived automatically via IconMap, not stored on the row. |
| ShowScrollbar | Boolean | Shows the scrollbar on the timeline gallery. |
| SubDetailCardHeight | Number | Height of an entry card that includes the sub-detail panel. |
| Theme | Text | "Light" or "Dark". |
| ThemeConfig | Record | Light/Dark theme color token sets; override to customize. |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| ActiveFilter | Text | Currently selected filter value. |
| SelectedItem | Record | Last tapped audit trail record. |

## Events

| Event | Purpose |
|---|---|
| OnExport | Fires when the export button is tapped. |
| OnItemSelect | Fires when an audit trail card is tapped. |
| OnPrint | Fires when the print button is tapped. |

## Setup requirements

- Modern controls must be enabled in Studio (Settings → Updates → Preview → "Modern controls and themes") because the component uses `ModernCombobox`.
- Import via Components tab → New component → Import from code, paste the YAML, save.
- `AccessAppScope: true` is set on the component definition.
