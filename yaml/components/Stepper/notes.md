# Stepper

Source: https://www.powerappsui.com/components/stepper
YAML source (fetched directly, same file served by the page's "Copy YAML" button): https://www.powerappsui.com/components/cmpStepper.yaml

## What it is

This is a genuine Power Apps **Component** (`ComponentDefinitions: cmpStepper`, `DefinitionType: CanvasComponent`, component-level `Description: "Progress stepper with bar and circle modes. Labels, connectors, sizes, themes, and jump-to-step navigation built in."`), not a screen-level control group. It must be created via Studio's **Insert > Custom > New component**, then the Code View YAML pasted into that new component — it cannot be pasted directly onto a screen. This project (policy-tracker) has not used the real Component workflow before; everything else so far has been screen-level control groups, so treat this differently.

A multi-step progress indicator supporting two visual modes: a thin horizontal progress `Bar`, or numbered `Step` circles connected by lines with optional labels beneath. Step count is driven automatically by the row count of `StepLabels`. Completed steps show a checkmark (default or a custom SVG via `CheckIcon`). Supports click-to-jump navigation gated by `ClickableSteps`, three sizes, and light/dark theme.

## Control types used

Classic/Button@2.2.0, Gallery@2.15.0, GroupContainer@1.5.0, Image@2.2.3, Label@2.5.1, Rectangle@2.3.0

(No Modern* controls used; does not require modern controls setting.)

## Input properties

| Property | Type | Purpose |
|---|---|---|
| AccentColor | Color | Active and complete step highlight color. |
| CheckIcon | Text | Optional SVG data URI to replace the default checkmark; use `currentColor` as the stroke/fill token. |
| ClickableSteps | Text | "Completed" allows tapping done steps only; "None" disables all tapping; "All" allows jumping to any step. |
| CurrentStep | Number | Active step index, 1-based. |
| Density | Text | "Full" = circles with labels and connectors; "Compact" hides labels; "Minimal" hides labels and connectors. Only applies when Type is "Step". |
| Size | Text | "Small", "Medium", or "Large" — scales circle size, bar height, and component height. |
| StepLabels | Table | `{Label:Text}` rows, one per step; row count drives TotalSteps automatically. |
| Theme | Text | "Light" or "Dark" — controls connector, future-circle, and label colors. |
| Type | Text | "Bar" for a thin progress bar, "Step" for numbered circles with labels. |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| SelectedStep | Number | Step index last tapped by the user. |

## Events

| Event | Purpose |
|---|---|
| OnStepSelect | Fires when a step circle is tapped (subject to ClickableSteps); read `SelectedStep` inside the handler. |

## Setup requirements

- `AccessAppScope: true` is set on the component definition.
- Import via Components tab → New component → Import from code, paste the YAML, save.
- No modern-controls requirement and no specific fonts noted.
