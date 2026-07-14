# Loading Screen

Source: https://www.powerappsui.com/components/loading-screen
YAML source (fetched directly, same file served by the page's "Copy YAML" button): https://www.powerappsui.com/components/cmpLoadingScreen.yaml

## What it is

This is a genuine Power Apps **Component** (`ComponentDefinitions: cmpLoadingScreen`, `DefinitionType: CanvasComponent`), not a screen-level control group. It must be created via Studio's **Insert > Custom > New component**, then the Code View YAML pasted into that new component — it cannot be pasted directly onto a screen. This project (policy-tracker) has not used the real Component workflow before; everything else so far has been screen-level control groups, so treat this differently.

A full-screen app-loading splash overlay: centered logo mark, two-tone wordmark (AppName + AppNameAccent), subtitle, animated progress bar/dots keyed to `LoadingSteps`/`CurrentStep`, a bottom version badge (CompanyName + AppVersion), and error-state text if `ErrorMessage` is set. Uses an internal timer to enforce a minimum display time (`MinDisplayTime`) even if the app's actual `OnStart` finishes faster, then fires `OnReady` / sets `IsReady`.

## Control types used

Gallery@2.15.0, GroupContainer@1.5.0, HtmlViewer@2.1.0, Image@2.2.3, Label@2.5.1, Progress@1.1.34, Rectangle@2.3.0, Timer@2.1.0

(No Modern* controls used; does not require modern controls setting.)

## Input properties

| Property | Type | Purpose |
|---|---|---|
| AccentColor | Color | Applied to logo, wordmark, divider, dots, ring, version label. |
| AppName | Text | First word of the wordmark, default text color. |
| AppNameAccent | Text | Second word of the wordmark, rendered in AccentColor; leave blank for single-word app names. |
| AppSubtitle | Text | Small uppercase subtitle below the wordmark. |
| AppVersion | Text | Version string shown in the bottom badge. |
| CompanyName | Text | Left side of the version badge. |
| CurrentStep | Number | 1-based active loading stage index (0 = not started). |
| ErrorMessage | Text | When non-blank, hides progress bar/dots and shows this text in red — use to surface OnStart failures. |
| LoadingSteps | Table | `{msg:Text}` rows, one per loading stage. |
| LogoLetter | Text | Single character inside the logo mark square. |
| MinDisplayTime | Number | Minimum milliseconds the screen stays visible regardless of how fast OnStart finishes; when elapsed, OnReady fires. |
| Theme | Text | "Dark" or "Light". |

## Output properties

| Property | Type | Purpose |
|---|---|---|
| IsReady | Boolean | True once MinDisplayTime has elapsed; reactive alternative to the OnReady event. |

## Events

| Event | Purpose |
|---|---|
| OnReady | Fires once when the internal timer ends (MinDisplayTime elapsed). Wire to `Set(gAppReady, true)`. |

## Setup requirements

- Set component `Width = App.Width`, `Height = App.Height`, position `X=0, Y=0` for full-screen coverage.
- Component must be placed as the last child in the screen's control tree (bottom of the Tree view / on top visually) so it overlays other content while loading.
- Wire `OnReady` to `Set(gAppReady, true)` and set the host screen's main content `Visible = !gAppReady` (or similar) to reveal content once loading completes.
- `AccessAppScope: true` is set on the component definition.
- Import via Components tab → New component → Import from code, paste the YAML, save.
- No modern-controls requirement and no specific fonts noted.
