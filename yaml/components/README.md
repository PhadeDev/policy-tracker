# PowerAppsUI Component Archive

23 free components pulled from [powerappsui.com/components](https://www.powerappsui.com/components) on 2026-07-14, one folder each — `<Name>/<Name>.yaml` (the raw Code View YAML, verbatim) plus `<Name>/notes.md` (source URL, description, full Input/Output/Event property tables, control types, setup requirements).

**These are pure reference material, not yet used anywhere in the live app.** Nothing here has been pasted into Studio or verified to actually work in this app's environment.

## Important: these are genuine Power Apps Components, not screen control groups

Every screen-level thing built in this app so far (modals, the toast, the skeleton cards) is a plain `GroupContainer` duplicated per-screen — this project has never used Studio's real **Component** feature (Insert → Custom → New Component, with Input/Output/Event properties) before. Every file here uses that real Component format (`ComponentDefinitions:` / `DefinitionType: CanvasComponent`). Building one of these for real means creating a blank Component in Studio first, then pasting the YAML into that — not a plain screen paste. Treat the first one you actually build as a pilot, not a known-safe pattern, since nothing in this batch has been tested live yet.

## How the site actually delivers the YAML (useful if re-fetching later)

The page HTML never contains the YAML directly — it's Next.js, client-rendered. The "Copy YAML" button on each page does a `fetch()` to a static asset, e.g. `/components/dialog.yaml`, `/components/cmpCalendar.yaml`. Some components' internal file/component names don't match their URL slug (noted per-row below) — found by tracing each page's JS bundle, not guessable from the URL alone.

## Index

| Component | Folder | Internal name | Modern controls required | Notable |
|---|---|---|---|---|
| Activity Timeline | `ActivityTimeline/` | — | Yes (ModernCombobox) | |
| Bar List | `BarList/` | — | Yes (ModernText) | |
| Photo Upload | `PhotoUpload/` | — | Yes | Needs camera permission; outputs via app-scoped collections `_cmpTopPhotos`/`_cmpBottomPhotos`, not formal Output properties |
| Sidebar | `Sidebar/` | — | No (Classic only) | `AccessAppScope: true` — reads/writes app-scoped variables directly |
| Loading Screen | `LoadingScreen/` | — | No (Classic only) | Must be placed as a full-screen top overlay |
| Stepper | `Stepper/` | — | No (Classic only) | Simplest of the batch |
| Segmented Control | `SegmentedControl/` | — | — | |
| Line Chart | `LineChart/` | — | — | |
| Navigation Menu | `NavigationMenu/` | — | — | |
| SVG Bar Chart | `BarChart/` | `cmpStackedBarChart` | — | |
| KPI Cards | `KpiCards/` | `cmpKPI` | — | |
| Floating Action Button | `FloatingActionButton/` | `cmpFloatingActionButton` | — | |
| Notification Badge | `NotificationBadge/` | `Badge` | — | |
| Bottom Navigation Bar | `BottomNavigationBar/` | `cmpBottomNavigation` | — | |
| Chips | `Chips/` | `cmpChips` | — | |
| Date Picker | `DatePicker/` | `cmpCalendar` | — | **Distinct from this app's existing `ModernDatePicker` control** — don't confuse the two |
| Table | `DataTable/` | `cmpMyTable` | — | Largest component in the set |
| Loading Overlay | `LoadingOverlay/` | `cmpLoadingSpinner` | — | |
| Dialog | `Dialog/` | `cmpEnhancedDialog` | Yes | Original reason for this whole archive — candidate to replace the Delete Item confirmation as a first pilot |
| Breadcrumbs | `Breadcrumbs/` | — | — | |
| Pie Chart | `PieChart/` | — | — | |
| Heatmap | `Heatmap/` | — | — | |
| File Upload | `FileUpload/` | — | — | Page description mentions SharePoint/DocumentLibrary properties, but the actual YAML has no SharePoint connector wiring — it only stages files locally via `Attachments` and exposes `OnSave` for the caller to implement upload. Flagged as a real discrepancy between the site's description and its shipped YAML. |

Rows with "—" in Internal name matched their folder name exactly; check that component's own `notes.md` for exact control-level detail either way.

## Not archived

Eight components on the site are listed "Coming Soon" with no path and nothing to fetch yet: Material Design Search Bar, Material Design Slider, Bottom Sheet, Side Sheet, Snackbar (Toast), Top App Bar, PCF LeftNav (Canvas App). Worth checking back on `/components` periodically if any of these would be useful later — Snackbar (Toast) in particular would be worth comparing against the custom toast already built for this app.

## Before using any of these for real

1. Read that component's `notes.md` in full — property names and setup requirements vary a lot component to component.
2. Cross-check any control type it uses against `~/PowerApps/reference/database.html` (the Bible) for known-bad-patterns in this app specifically — a control being fine on powerappsui.com's own site doesn't guarantee it behaves identically here.
3. Build it as an actual Studio Component (not a screen paste) and verify visually before trusting it in place of anything currently working — same "don't delete the working thing until the replacement is confirmed" discipline as everything else in this project.
