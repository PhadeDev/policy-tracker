# Navigation Menu

Source: https://www.powerappsui.com/components/navigation-menu
(YAML pulled from the site's static asset at https://www.powerappsui.com/components/navigation-menu.yaml)

## What it does

A horizontal top-nav bar with optional mega-menu / dropdown behavior per item. Top-level items
come from `MenuItems` (each optionally flagged `HasDropdown`); items flagged as having a dropdown
open a panel populated from `DropdownItems`, grouped by `Section` and laid out in 1 or 2 columns
(`DropdownColumns`) — i.e. a simple list or a "mega menu" grid, each entry optionally showing an
icon and description. Tracks which menu is open and which item was last selected, and exposes
these as outputs.

## This is a real Power Apps Component

This is a genuine Canvas Component (`ComponentDefinitions: cmpNavigationMenu`, `DefinitionType:
CanvasComponent`, `AccessAppScope: true`) with declared Input/Output/Event custom properties — not
a screen-level control group. It must be created via Power Apps Studio's **Insert > Custom > New
Component** (or imported as a component in Code View), not pasted onto a screen directly. This
project has not used the Components workflow before; every prior control set has been
screen-level groups, so this requires the different import/build path.

## Control types used

- `GroupContainer@1.5.0` — nav bar wrapper, dropdown panel wrapper, per-item/section containers
- `Gallery@2.15.0` (Horizontal variant) — top-level nav item list and dropdown item lists
- `Classic/Button@2.2.0` — hit targets for nav items and dropdown items
- `Image@2.2.3` — icons for dropdown items
- `Text@0.0.51` — labels (classic Text control, not ModernText)

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `ActiveColor` | Color | Accent color for the active/selected menu item (default blue) |
| `DropdownColumns` | Number | 1 = simple list, 2 = mega-menu grid (default `2`) |
| `DropdownItems` | Table | `{MenuID, Section, Icon, Label, Description, Link, Column}` — dropdown content, keyed to a parent `MenuItems` row by `MenuID` |
| `MenuItems` | Table | `{ID, Label, HasDropdown (Boolean), Link}` — top-level nav items |
| `NavAlign` | Text | `"Left"`, `"Center"`, or `"Right"` alignment of the nav bar |
| `ShowDescriptions` | Boolean | Show item descriptions in the dropdown |
| `ShowDropdownScrollbar` | Boolean | Show scrollbar in dropdown galleries |
| `ShowIcons` | Boolean | Show icons in dropdown items |
| `ShowNavScrollbar` | Boolean | Show scrollbar on the nav bar gallery |
| `Theme` | Text | `"Light"` or `"Dark"` |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| `IsOpen` | Boolean | Whether a dropdown is currently open |
| `SelectedItem` | Record | The last selected item record (nav item or dropdown item) |
| `SelectedMenuID` | Text | ID of the currently open or last-selected top-level menu |

## Events

| Event | Purpose |
|---|---|
| `OnItemSelect` | Fires when any menu or dropdown item is clicked |

## Setup requirements

- No explicit "Modern controls" flag noted, and this component actually uses the classic
  `Text@0.0.51` control (not ModernText) alongside `Classic/Button`, `Gallery`, `GroupContainer`,
  `Image` — all standard/classic Studio controls, so no Modern-controls preview feature is
  strictly required for this one (unlike Segmented Control / KPI Cards).
- `MenuItems` and `DropdownItems` are two related tables joined by `MenuID`/`ID` — both need
  populating together for dropdowns to resolve correctly; sample default data models a warehouse
  app's Inventory/Operations/Reports nav.
