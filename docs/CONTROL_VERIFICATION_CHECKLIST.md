# Control Verification Checklist

Purpose: build a ground-truth reference for every control type Microsoft's canvas apps
offer — not just what this app happens to use today, but the full catalogue, so future
features (Progress bar being the immediate example: wanted for `ViewItem`'s approval
pipeline, never added because it was never verified) can be reached for with confidence
instead of getting discovered-broken mid-build the way Combobox/Dropdown/DatePicker
were this session. Verified by looking at and interacting with a real freshly-inserted
control — not by reading Microsoft's docs (repeatedly wrong this session) and not by
exporting YAML (untouched properties aren't written to the export at all, so there's
nothing to read that way).

**Method per control:** insert it fresh on a blank test screen, touch nothing, then:
1. **Look** — screenshot it as-is. Note anything visually unexpected (shadow, border,
   fill colour, corner radius, font) that isn't obviously "blank/neutral."
2. **Poke** (input controls only) — set a real value into whatever its default-value
   property is, click into it, try typing/selecting/clearing, click away, click back.
   Note anything that doesn't behave the way it looks like it should.
3. Send me the screenshot(s) + a one-line note per control ("shadow on", "clear button
   appears on focus", "fine, nothing weird"). I'll turn confirmed findings into
   `known-bad-patterns.md` entries and skip anything that's already fine.

Already-confirmed facts are marked ✅ below — no need to re-test those specific points,
though a fresh look never hurts if something seems off later.

---

## Layout containers

### `GroupContainer@1.5.0` — Variant: ManualLayout
- ✅ `DropShadow` is ON by default (confirmed, now fixed everywhere in `ViewItem_1.yaml`)
- Check: default `Fill` (transparent or white?), default `BorderThickness`/`BorderColor`
  (any border at all when unset?), default `RadiusTopLeft` etc. (any rounding at all?)

### `GroupContainer@1.5.0` — Variant: AutoLayout
- ✅ `DropShadow` is ON by default (same as ManualLayout)
- ✅ `FillPortions` unset behaves as flexible/fill-remaining-space, not fixed — must
  explicitly set `FillPortions: =0` for a container meant to hold a fixed `Height`
- Check: same Fill/Border/Radius questions as ManualLayout above
- Check: does a `ManualLayout` child of this need `AlignInContainer: Stretch` to get
  full width, or does it stretch by default? (We assumed it needs to be explicit —
  worth confirming since we've been setting it everywhere defensively.)

---

## Text / display

### `Label@2.5.1`
- Check: default `Color`, default `Font`, default `Size`, default `Align`/`VerticalAlign`
- Check: does it wrap text by default, or clip/truncate?

### `Text@0.0.51` (Modern Text, used in modal titles)
- Check: same as Label — default color/font/size, and whether `AutoHeight` behaves
  as expected when left unset

### `ModernText@1.0.0` (used for pill badges — confirmed supports Radius)
- Check: default `Fill` (transparent?), default `Color`, whether `RadiusTopLeft` etc.
  genuinely apply with no other property needed

### `HtmlViewer@2.1.0`
- Check: default `Height`/`Width` behavior with HTML content taller than the box —
  does it scroll, clip, or auto-grow?

### `Image@2.2.3`
- Check: default `ImagePosition` (Fit/Fill/Stretch?) when a real image is loaded

### `Rectangle@2.3.0`
- ✅ Does NOT support `RadiusTopLeft` etc. (confirmed, PA2108) — use `ModernText` or
  `Classic/Button` styled as a shape instead if rounding is needed
- Check: default `Fill` colour

### `Badge@0.0.24`
- Check: default `Appearance`, default size/shape if left unset

### `Progress@1.1.34`
- Check: what it looks like with no `Value`/`Max` set — does it render as empty, full,
  or indeterminate by default?

---

## Buttons / icons

### `Classic/Button@2.2.0`
- Check: default `Fill`/`BorderColor` — is there a default border even when not set?
- Check: default `DisplayMode` (should be Edit/enabled)

### `Button@0.0.45` (Modern Button, used for icon-only actions like Delete/Duplicate)
- ✅ Icon-only works by setting `Text: =""` + `Icon: ="Name"`, no explicit `Layout`
  property needed for that to render icon-only (confirmed, no error this session)
- Check: default `BasePaletteColor` if left unset — does it default to something
  reasonable or something jarring?

### `ModernButton@1.0.0`
- ✅ Does NOT support `FontWeight` (confirmed, PA2108)
- ✅ Light/pastel `BasePaletteColor` renders dark regardless (Fluent overrides it) —
  always use a dark, saturated seed colour
- Check: default `Layout` (IconOnly/IconBefore/IconAfter/TextOnly?) when unset

### `Classic/Icon@2.5.0`
- Check: default `Color` when unset

### `ModernIcon@1.1.1`
- Check: how this differs from `Classic/Icon` — default size, default color, whether
  it needs an explicit `Icon` enum vs a string name

---

## Simple inputs

### `Classic/TextInput@2.3.2`
- Check: does `Default` behave normally (single evaluation at creation, same as
  everything else), or does typed text visually disappear/misbehave under any
  particular property combination?

### `Classic/CheckBox@2.1.0`
- Check: default `CheckboxSize`, default `CheckmarkFill` — anything surprising when
  totally unset?

### `Classic/Toggle@2.1.0`
- ✅ Uses `Checked` not `Default`/`.Value` (confirmed, known-bad-patterns already)
- Check: what it actually looks like out of the box — does it need explicit colours
  to not look like the default Microsoft blue?

### `DatePicker@0.0.46` (the one that actually works with `SelectedDate`)
- ✅ Confirmed working, `SelectedDate` is settable and correct for pre-fill
- Check: default date format shown when `Format` isn't set

### `Classic/DatePicker@2.6.0`
- Check: how this differs from `DatePicker@0.0.46` above — is `SelectedDate` also
  valid here, or does it use `DefaultDate` only (as it did before we swapped it out)?

---

## Selection / search inputs — the ones that have burned us most

### `ModernDropdown@1.0.2`
- ✅ `Default` only evaluates once at creation — reactive changes need the source
  variable set BEFORE the control renders, not `Reset()` after (Reset does work
  correctly for this control, unlike Combobox)
- ✅ `Required: =true` does NOT reliably suppress the clear/X button in practice
  (confirmed — Microsoft's docs imply it should, it didn't)
- ✅ `Default` built via hand-made `{Value: ...}` record can silently fail to
  pre-select even with a real value — use `LookUp(Items, Value = target)` instead
- Check: is there ANY property/combination that suppresses the clear-X? (Low
  expectation given prior research, but worth one direct look before we fully give up)

### `ModernCombobox@1.1.1`
- ✅ `Reset()` clears the selection instead of restoring `DefaultSelectedItems` —
  never call `Reset()` on this control
- ✅ `DefaultSelectedItems` must point at a row that's ALSO present in the current
  `Items` result — if `Items` is a live search that starts blank, the pre-selected
  person must be manually included in the blank-search branch or it won't show
- ✅ `FontWeight: =""`, `Appearance.FilledLighter`, `AllowExternalSelectedItems` are
  all invalid — don't reintroduce them
- ✅ Documented Microsoft-acknowledged instability: "blank dropdown menu from time
  to time" — no further debugging needed if it recurs after all the above are correct,
  that's the acknowledged limit, not a new bug to chase

### `Classic/DropDown@2.3.1`
- Check: does this one show the same clear-X issue as ModernDropdown, or is that
  purely a Modern-control thing?

### `Classic/Radio@2.3.0`
- Check: default `Layout` (Horizontal/Vertical) and default spacing between options

### `ComboBox@0.0.51` (the "Canvas"-family one used for Owner before Modern issues)
- ✅ Supports `BorderRadius` and `Appearance: ='ComboboxCanvas.Appearance'.FilledLighter`
  — can look modern without being the buggy Modern version
- ✅ Requires `ComboBoxDataField@1.5.0` children (set via the Data/"Edit fields" panel)
  to define searchable/displayed fields — `ItemDisplayText` doesn't apply here
- ⚠️ May no longer be insertable fresh from the Insert panel in current environments
  — only recreatable by pasting its YAML directly (confirmed this session)

### `Classic/ComboBox@2.4.0`
- Check: is this a third, different combo box variant from the two above? Where does
  it appear in the Insert panel, and how does it differ visually/functionally?

### `ComboBoxDataField@1.5.0`
- Not independently insertable — appears as a child of `ComboBox@0.0.51` via the
  Data/Fields panel. No standalone check needed.

---

## Data / collection controls

### `Gallery@2.15.0`
- Check: default `TemplateSize` and default `TemplatePadding` if unset
- Check: default behavior when `Items` is empty — does it show a blank area or
  collapse to zero height?

### `Form@2.4.4`
- ✅ Deliberately avoided for main data entry (can't restyle TypedDataCards) — still
  used for the link-add/edit modal. Check: does `ResetForm()` reliably clear it
  between "add new" and "edit existing" uses, or does stale data linger?

### `TypedDataCard@1.0.7`
- Only relevant if `Form@2.4.4` is in use. Check: can its rendered fields be styled
  (rounded corners, custom colours) at all, or does it fully resist styling as
  previously assumed?

---

## Full Modern control catalogue — not yet used anywhere in this app

Microsoft lists 23 Modern controls total. The sections above cover every one we've
actually used; these are the remaining ones — not used yet, so nothing's confirmed
broken, but nothing's confirmed *working* either. Verify before the first real use
rather than finding out mid-build, which is what happened with Combobox/Dropdown/
DatePicker this session. Where there's an obvious fit for this app, it's noted.

### `Progress bar` (likely tag `Progress@1.1.x` or similar — confirm exact tag on insert)
**Why we want this one specifically:** wanted for `ViewItem` — a readiness/completion
bar for the 3-stage approval pipeline (Pillar Lead → Approved → Published), similar to
the aggregate "Release Pipeline" bars already built in `Main_1`. Never got added to
`ViewItem_1` at all — first real gap to close once this control is verified.
- Check: `Value`/`Max`/`Min` — plain numbers, or does it need a 0–1 ratio?
- Check: determinate vs indeterminate mode — is that a separate property or inferred
  from whether `Value` is set?
- Check: default fill colour, default track colour, default height/corner style
- Check: can it be made to visually match the mint/green "complete" theme already used
  elsewhere on this screen, or does it fight the theme like ModernButton's palette does

### `Avatar`
**Possible fit:** could replace the hand-built avatar-circle-plus-initials-fallback
pattern (`imgOwnerAvatar1`/`lblOwnerInitials1`) with one real control.
- Check: does it accept an image URL directly, or does it need `Office365Users.
  UserPhotoV2` piped through some intermediate property?
- Check: built-in initials fallback when no image — does it generate initials itself,
  or still need our own `Left`/`Find`-based formula?
- Check: default size, default shape (circle vs square)

### `Header`
**Possible fit:** could replace the hand-built `conTopBar1` entirely.
- Check: what slots/properties it exposes (title, actions, nav) — does it support a
  free-form action-button cluster like Save/Close/Delete/Duplicate, or is it more
  rigid/opinionated than a plain GroupContainer?
- Check: default height, default colours — does it auto-theme from the app's palette?

### `Tabs or tab list`
**Possible fit:** `Main_1`'s segmented dashboard-tile control was hand-built — this
might be the real control for that pattern, worth knowing for next time even though
`Main_1` already works without it.
- Check: default appearance (underline vs filled pill style)
- Check: how selection state is read (`Selected`/`SelectedIndex`?) and set (`Default`?)

### `Data Grid (preview)`
**Possible fit:** an alternative to `Gallery@2.15.0` for anything that becomes a real
table view (sortable/scrollable columns) rather than a card list.
- Check: how columns are configured — via a Fields/Edit-columns panel like Combobox,
  or via YAML properties directly?
- Check: sorting/filtering — built in, or still manual via `Sort`/`Filter` on `Items`?
- Note: preview feature — check for the same kind of instability warning we found for
  ModernCombobox before relying on it for anything real

### `Table (preview)`
**Possible fit:** similar territory to Data Grid — check what actually differs between
the two before picking either for future use.
- Check: same points as Data Grid above

### `Card (preview)`
**Possible fit:** could replace the hand-built link-card pattern (`btnLinkCard1` +
layered icon/label/actions) with a single purpose-built control.
- Check: does it support the "whole card clickable, icons/actions layered on top and
  independently clickable" pattern we had to hand-build, or does it fight that the way
  ModernButton fights conditional colours?
- Check: horizontal vs vertical orientation — property or separate variant?

### `Checkbox` (Modern — distinct from `Classic/CheckBox@2.1.0`)
- ✅ Already know Modern `Toggle` uses `Checked` not `Default`/`.Value` — check whether
  Modern `Checkbox` follows the same convention or its own
- Check: default size, default checkmark colour, whether label text is a separate
  property or built in

### `Combobox` — already covered above as `ModernCombobox@1.1.1`, no further action.

### `Copilot answer (preview)`
**Fit:** none currently anticipated for this app. Low priority — verify only if a
future feature actually calls for AI-generated answers in-app.

### `Info button`
**Possible fit:** tooltip/help-text affordance — e.g. explaining what "Not Yet
Approved" means, or clarifying the anomaly-amber stepper state, without cluttering
the layout with permanent explanatory text.
- Check: default icon, default popover styling, how the content is set (plain text
  property vs a child-control popover)

### `Link`
**Possible fit:** the "Cadets Branch Writing Guide" reference inside the pillar-lead
confirmation `HtmlViewer` text is currently a plain HTML `<a>` tag — this might be a
cleaner native alternative if a non-HTML-embedded link control is ever needed.
- Check: default styling (underline? colour?), whether `OnSelect` fires or it only
  supports a raw URL to open directly

### `Number input`
**Fit:** not currently needed anywhere in this app (no numeric entry fields), but
cheap to verify now while doing the rest of this pass.
- Check: `Default` vs `Value` split (same input/output pattern as Slider, already
  noted in the Bible) — confirm it still holds
- Check: default `Min`/`Max`/`Step` when unset

### `Slider`
**Fit:** not currently needed, verify for completeness.
- Check: same `Default`/`Value` split question as Number input

### `Spinner`
**Possible fit:** could replace the app's existing `varLoading`/`varLoadingLinks`-style
loading text/blank states with a real spinner control.
- Check: default size, default colour, how it's shown/hidden (its own `Visible`
  property, or does it need a wrapping container?)

### `Rating`
**Missed entirely until an external research pass caught it — added 2026-07-05.** Not
in Microsoft's own "Modern controls" summary list, but has its own docs page.
**Fit:** none obvious yet for this app (no star-rating data anywhere in Policy
Tracker), but verify anyway since it was missed once already — check the summary
list itself isn't hiding other gaps.
- Check: `Default`/`Value` split (same input/output pattern question as Slider/Number
  input), default number of stars, default icon/style

### `Stream (preview)`
**Fit:** none anticipated (no video content in this app). Lowest priority — skip
unless time allows after everything else.

---

## Once this is done

Send me the screenshots/notes (issue 8 or directly here, whichever's easier) and I'll
turn every confirmed finding into a `known-bad-patterns.md` entry, and update
`powerapps-control-defaults.md` with the real, observed default behavior for each
control — replacing "Microsoft's docs say" with "confirmed live in this environment"
everywhere it matters.
