# Control Verification Checklist

Purpose: build a ground-truth reference for every control type this app actually uses,
verified by looking at and interacting with a real freshly-inserted control — not by
reading Microsoft's docs (repeatedly wrong this session) and not by exporting YAML
(untouched properties aren't written to the export at all, so there's nothing to read).

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

## Once this is done

Send me the screenshots/notes (issue 8 or directly here, whichever's easier) and I'll
turn every confirmed finding into a `known-bad-patterns.md` entry, and update
`powerapps-control-defaults.md` with the real, observed default behavior for each
control — replacing "Microsoft's docs say" with "confirmed live in this environment"
everywhere it matters.
