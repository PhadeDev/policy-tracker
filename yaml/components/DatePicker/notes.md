# Date Picker Component (cmpCalendar) — THIRD-PARTY, DISTINCT FROM THIS APP'S ModernDatePicker

**Source URL:** https://www.powerappsui.com/components/date-picker
**Raw YAML source:** https://www.powerappsui.com/components/cmpCalendar.yaml
**Fetched:** 2026-07-14

## Important distinction

**This is a different control from the `ModernDatePicker` control already used elsewhere in this app** (e.g. in the Thursday checklist / ViewItem date fields). `ModernDatePicker` is a single-field Studio-native date input. This `cmpCalendar` component is a full standalone calendar/date-picker **panel component** with its own header, footer, day grid, range/multi-select modes, holiday highlighting, and Cancel/OK buttons — built entirely from primitive controls (no `ModernDatePicker` control inside it). Do not assume these two are interchangeable or that fixing one affects the other.

## What it does

A full popup-style calendar component (`cmpCalendar`) with a month grid (`Gallery`-rendered day cells), header (title + selected date + optional close X), footer (Cancel/OK, "go to today" button), and configurable first-day-of-week. Supports three selection modes: single date, range (start/end), and multi-date — configured via `Settings.SelectRange` / `Settings.SelectMultiple`. Can block/disable weekends, block specific dates, block US federal holidays (auto-computed via the `FederalHolidays` output), and highlight specific dates or holidays with distinct fills. Fully theme-configurable (light/dark, plus per-color-role overrides) via the `Theme` table input.

## Component type

**This is a genuine Power Apps canvas Component** (`DefinitionType: CanvasComponent`, `AccessAppScope: true`, with Input/Output/Event `CustomProperties`), not a screen-level control group. Install via Studio's Insert > Custom > New Component (or Components tab > New component > Import from code) and paste the YAML — this is not a plain screen paste. This project has not used the real Components workflow before.

## Control types used

- `GroupContainer@1.4.0` — outer panel, header, footer, day-grid rows/cells
- `Gallery@2.15.0` — day-grid rendering, legend rendering
- `Label@2.5.1` — day numbers, header title/date, weekday headers, legend text
- `Image@2.2.3` — header icons (close X, prev/next month chevrons, etc.)
- `Rectangle@2.3.0` — divider lines
- `Circle@2.3.0` — selection/today/highlight day markers
- `Classic/Button@2.2.0` — Cancel/OK buttons, close icon hit target, prev/next month, today button

## Input properties

| Property | Type | Purpose |
|---|---|---|
| `DarkMode` | Boolean | Enable dark theme (default `false`) |
| `Selection` | Record | Date defaults/constraints/highlights: `DefaultDate`, `DefaultEndDate`, `DefaultSelection`, `MinDate`, `MaxDate`, `BlockDates`, `DisableWeekends`, `BlockFederalHolidays`, `HighlightDates`, `HighlightFederalHolidays` |
| `Settings` | Record | Display/behavior: `Title`, `DateFormatting`, `FirstDayOfWeek`, `TodayStyle`, `ShowHeader`, `ShowFooter`, `ShowCloseButton`, `ShowPadding`, `ShowTodayButton`, `SelectRange`, `SelectMultiple`, `AllowEmptySelection`, `ShowLegend` |
| `Theme` | Table | Per-mode (Light/Dark) color palette — ~18 color roles (selectionFill, todayBorderColour, containerFill, textColour, highlightFill, federalHolidayFill, hoverFill, etc.) plus icon SVGs |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| `ComponentHeight` | Number | Total rendered height of the component |
| `FederalHolidays` | Table | Computed US federal holidays for the displayed year and adjacent years |
| `SelectedDate` | DateAndTime | Currently selected date (single-select mode) |
| `SelectedDates` | Table | All selected dates (multi-select mode) |
| `SelectedEndDate` | DateAndTime | End date of the selected range (range-select mode) |

## Events

- `OnChange` — fires when a date is selected; passes `SelectedDate` parameter
- `OnConfirm` — fires when the OK button is pressed
- `OnCancel` — fires when the Cancel button is pressed
- `OnClose` — fires when the close (X) button is pressed

## Setup requirements

- Modern controls and themes should be enabled in Studio (Settings > Updates > Preview > Modern controls and themes).
- Mixes modern versioned controls (`GroupContainer`, `Gallery`, `Label`, `Image`, `Rectangle`, `Circle`) with one classic control (`Classic/Button@2.2.0`).
- Component is sizeable (~30 controls, ~79KB YAML) — expect a nontrivial paste/import in Studio.
