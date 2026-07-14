# Sidebar

Source: https://www.powerappsui.com/components/sidebar
YAML source (fetched directly, same file served by the page's "Copy YAML" button): https://www.powerappsui.com/components/cmpSidebar.yaml

## What it is

This is a genuine Power Apps **Component** (`ComponentDefinitions: cmpSidebar`, `DefinitionType: CanvasComponent`), not a screen-level control group. It must be created via Studio's **Insert > Custom > New component**, then the Code View YAML pasted into that new component — it cannot be pasted directly onto a screen. This project (policy-tracker) has not used the real Component workflow before; everything else so far has been screen-level control groups, so treat this differently.

A full-height collapsible left-hand app navigation sidebar: header with logo mark + app name, optional search box, a nested tree of nav items (sections, parent/child items, badges/dots, icons from an SVG library table), and a footer with user avatar/name/email and an optional context menu (Settings/Help/Theme toggle). Supports expand/collapse toggle, light/dark theme, and role-based item visibility.

## Control types used

Classic/Button@2.2.0, Classic/Icon@2.5.0, Classic/TextInput@2.3.2, Gallery@2.15.0, GroupContainer@1.5.0, Image@2.2.3, Label@2.5.1, Rectangle@2.3.0

(All classic/legacy control variants — this component does not require modern controls to be enabled.)

## Input properties

| Property | Type | Purpose |
|---|---|---|
| AccentColor | Color | Primary accent for selected items, badges, highlights. |
| AppLogo | Image | Header logo image; when blank shows first letter of AppName. |
| AppName | Text | First word of app name in header (expanded state). |
| AppNameAccent | Text | Second word of app name, rendered in AccentColor. |
| Expanded | Boolean | Initial expanded/collapsed state on load; pairs with `_cmpNavIsExpanded` app variable. |
| Icons | Table | SVG icon library, `{Name, SVG}` pairs; SVGs use `stroke='white'`; referenced from `Items.Icon` by name string. Ships with 15 built-in icons (Grid, Layers, File, User, Settings, Home, Dashboard, Bell, Chart, Users, Database, Box, Shield, Code, Help). |
| Items | Table | Nav data. Schema: `{ID:Number, Title:Text, Icon:Image, Letter:Text, Badge:Number, BadgeColor:Color, BadgeDot:Boolean, IsSection:Boolean, ParentID:Number, Visible:Boolean}`. `IsSection=true` for divider/label rows; `Badge=0` hides the badge; `BadgeDot=true` shows a dot instead of a number; `ParentID=-1` for root level; `Visible=false` hides an item (sections auto-hide when all children are hidden). |
| ShowFooterMenu | Boolean | Show a context menu on the footer (Settings/Help/Theme toggle). |
| ShowHeader | Boolean | Show logo + app name header. |
| ShowSearch | Boolean | Show search bar below header (visible only when expanded). |
| ShowToggle | Boolean | Show the expand/collapse toggle pill. |
| ShowUser | Boolean | Show current user initials/name at bottom; hides entire footer when false. |
| Theme | Text | "Dark" or "Light"; controls all background/text/border/hover colors. |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| IsExpanded | Boolean | Mirrors Expanded / `_cmpNavIsExpanded`; use to offset screen content, e.g. `X = cmpSidebar.Width`. |
| SelectedID | Number | ID of the most recently tapped nav item; initialize `_cmpNavSelID` on screen OnVisible for default. |

## Events

| Event | Purpose |
|---|---|
| OnNavSelect | Fires when a leaf nav item is tapped; read `SelectedID` to navigate. |
| OnToggle | Fires when expand/collapse toggle pressed; flip your `Expanded`/gNavExpanded variable here. |
| OnFooterSettings | Fires when Settings tapped in footer menu. |
| OnHelp | Fires when Help tapped in footer menu. |
| OnThemeToggle | Fires when Theme toggle tapped in footer menu; toggle your theme variable here. |

## Setup requirements

- On each screen's `OnVisible`, set three app variables/collections:
  - `Set(_cmpNavIsExpanded, true)`
  - `Set(_cmpNavSelID, 11)`
  - `ClearCollect(colNavExpanded, {ID: 1})`
- Position the component at `X = 0`, `Y = 0`, `Height = App.Height`.
- Bind adjacent screen content to `X = cmpSidebar.Width` and `Width = App.Width - cmpSidebar.Width` to avoid overlap.
- `AccessAppScope: true` is set on the component (required — it reads/writes app-scoped variables directly, e.g. `_cmpNavIsExpanded`, `_cmpNavSelID`, `colNavExpanded`).
- Import via Components tab → New component → Import from code, paste the YAML, save.
- Does not require modern controls (uses Classic/* control variants).
