# Owner Picker Fallback — Text Input + Gallery (no Combobox)

Built 2026-07-05 as a fallback for `cmb_Owner1` after both `ComboBox@0.0.51` (retired
from the Insert panel, only pasteable) and `ModernCombobox@1.1.1` (Microsoft-acknowledged
"blank dropdown from time to time" instability) gave trouble. This avoids the Combobox
control family entirely — built from Text Input + Gallery, the two most mature, boring,
stable controls in the product. Nothing about this can go "unstable" the way Combobox has.

## Architecture

- The field itself (`conFieldOwner1`) shows a clickable "chip" — avatar + selected name,
  or a placeholder — instead of an inline searchable box.
- Clicking it opens `OwnerPickerModal1`, a full-screen overlay (top-level screen sibling,
  same pattern as the already-working `DuplicateModal1`/`DeleteItemModal1`) containing a
  search box and a live-filtered results gallery.
- Selecting a result sets `varSelectedPerson` directly to the real Office365Users search
  row object and closes the modal.
- `varSelectedPerson` becomes the single source of truth for the chosen owner everywhere
  downstream (Save/Save & Close Patch, avatar image, initials fallback) — replacing every
  `cmb_Owner1.Selected.X` reference with `varSelectedPerson.X`.

Because everything downstream reads `varSelectedPerson` via plain property formulas
(`Text`, `Image`, `Color`), not `Default`/`DefaultSelectedItems`, this design also sidesteps
the "Default only evaluated once at control creation" issue entirely — property formulas
recompute reactively whenever the variable changes, so no `Reset()` calls are needed for
the owner field at all.

## Part A — replace the field itself

Select `conFieldOwner1`, delete `cmb_Owner1`, paste this in its place:

```yaml
- btnOwnerPicker1:
    Control: Classic/Button@2.2.0
    Properties:
      BorderColor: =RGBA(223, 229, 224, 1)
      BorderThickness: =1
      Fill: =RGBA(255, 255, 255, 1)
      Height: =68
      HoverFill: =RGBA(245, 247, 245, 1)
      OnSelect: |-
        =Set(varShowOwnerPicker1, true);
        SetFocus(txtOwnerSearchBox1)
      PressedFill: =RGBA(235, 240, 235, 1)
      RadiusBottomLeft: =8
      RadiusBottomRight: =8
      RadiusTopLeft: =8
      RadiusTopRight: =8
      Text: =""
      Width: =Parent.Width
      Y: =22
- lblOwnerDisplay1:
    Control: Label@2.5.1
    Properties:
      Color: =If(IsBlank(varSelectedPerson), RGBA(139, 151, 143, 1), RGBA(28, 32, 48, 1))
      Font: =Font.'Open Sans'
      Height: =68
      Size: =13
      Text: =If(IsBlank(varSelectedPerson), "Search for a person...", varSelectedPerson.DisplayName & " (" & varSelectedPerson.Mail & ")")
      VerticalAlign: =VerticalAlign.Middle
      Width: =Parent.Width - 130
      X: =80
      Y: =22
- icoOwnerSearch1:
    Control: Classic/Icon@2.5.0
    Properties:
      Color: =RGBA(139, 151, 143, 1)
      Height: =24
      Icon: =Icon.Search
      Width: =24
      X: =Parent.Width - 40
      Y: =44
```

The existing `imgOwnerAvatar1`/`lblOwnerInitials1`/`recErrOwner1` controls already sitting
in `conFieldOwner1` stay exactly where they are — just update the formula references
inside them per Part D below.

## Part B — the search modal

Select the `ViewItem` screen itself (or select `DuplicateModal1`/`DeleteItemModal1` to
paste as a new sibling right after), and paste this as a new top-level screen child:

```yaml
- OwnerPickerModal1:
    Control: GroupContainer@1.5.0
    Variant: ManualLayout
    Properties:
      Fill: =RGBA(0, 0, 0, 0.55)
      Height: =Parent.Height
      Visible: =varShowOwnerPicker1
      Width: =Parent.Width
    Children:
      - OwnerPickerCard1:
          Control: GroupContainer@1.5.0
          Variant: ManualLayout
          Properties:
            DropShadow: =DropShadow.Bold
            Fill: =RGBA(255, 255, 255, 1)
            Height: =480
            RadiusBottomLeft: =12
            RadiusBottomRight: =12
            RadiusTopLeft: =12
            RadiusTopRight: =12
            Width: =520
            X: =Parent.Width / 2 - 260
            Y: =Parent.Height / 2 - 240
          Children:
            - lblOwnerPickerTitle1:
                Control: Label@2.5.1
                Properties:
                  Color: =RGBA(28, 32, 48, 1)
                  Font: =Font.'Open Sans'
                  FontWeight: =FontWeight.Bold
                  Height: =30
                  Size: =16
                  Text: ="Select Owner"
                  VerticalAlign: =VerticalAlign.Middle
                  Width: =400
                  X: =24
                  Y: =16
            - btnOwnerPickerClose1:
                Control: Classic/Button@2.2.0
                Properties:
                  BorderThickness: =0
                  Color: =RGBA(91, 105, 97, 1)
                  Fill: =RGBA(0, 0, 0, 0)
                  Font: =Font.'Open Sans'
                  FontWeight: =FontWeight.Bold
                  Height: =30
                  HoverFill: =RGBA(240, 240, 240, 1)
                  OnSelect: |-
                    =Set(varShowOwnerPicker1, false);
                    Reset(txtOwnerSearchBox1)
                  Size: =14
                  Text: ="Close"
                  Width: =70
                  X: =Parent.Width - 94
                  Y: =16
            - txtOwnerSearchBox1:
                Control: Classic/TextInput@2.3.2
                Properties:
                  BorderColor: =RGBA(223, 229, 224, 1)
                  BorderThickness: =1
                  Default: =""
                  DelayOutput: =false
                  Fill: =RGBA(255, 255, 255, 1)
                  Font: =Font.'Open Sans'
                  Height: =44
                  HintText: ="Type a name or email..."
                  PaddingLeft: =10
                  RadiusBottomLeft: =8
                  RadiusBottomRight: =8
                  RadiusTopLeft: =8
                  RadiusTopRight: =8
                  Width: =472
                  X: =24
                  Y: =56
            - lblOwnerHint1:
                Control: Label@2.5.1
                Properties:
                  Color: =RGBA(139, 151, 143, 1)
                  Font: =Font.'Open Sans'
                  Height: =20
                  Size: =10
                  Text: ="Type at least 2 characters to search."
                  Visible: =Len(Trim(txtOwnerSearchBox1.Text)) < 2
                  Width: =472
                  X: =24
                  Y: =104
            - lblOwnerNoResults1:
                Control: Label@2.5.1
                Properties:
                  Color: =RGBA(139, 151, 143, 1)
                  Font: =Font.'Open Sans'
                  Height: =20
                  Size: =10
                  Text: ="No matches found."
                  Visible: =Len(Trim(txtOwnerSearchBox1.Text)) >= 2 && CountRows(galOwnerResults1.AllItems) = 0
                  Width: =472
                  X: =24
                  Y: =104
            - galOwnerResults1:
                Control: Gallery@2.15.0
                Variant: Vertical
                Properties:
                  BorderColor: =RGBA(0, 0, 0, 0)
                  Height: =340
                  Items: |-
                    =If(
                        Len(Trim(txtOwnerSearchBox1.Text)) >= 2,
                        Filter(
                            Office365Users.SearchUserV2({
                                searchTerm: txtOwnerSearchBox1.Text,
                                isSearchTermRequired: true,
                                top: 30
                            }).value,
                            "RC-CDTS" in DisplayName
                        ),
                        []
                    )
                  TemplateSize: =56
                  Width: =472
                  X: =24
                  Y: =128
                Children:
                  - btnSelectOwnerRow1:
                      Control: Classic/Button@2.2.0
                      Properties:
                        BorderThickness: =0
                        Fill: =RGBA(255, 255, 255, 1)
                        Height: =56
                        HoverFill: =RGBA(240, 246, 242, 1)
                        OnSelect: |-
                          =Set(varSelectedPerson, ThisItem);
                          Set(varUnsavedChanges, true);
                          Set(varErrOwner, false);
                          Set(varShowOwnerPicker1, false);
                          Reset(txtOwnerSearchBox1)
                        PressedFill: =RGBA(227, 239, 232, 1)
                        Text: =""
                        Width: =Parent.TemplateWidth
                  - lblOwnerRowName1:
                      Control: Label@2.5.1
                      Properties:
                        Color: =RGBA(28, 32, 48, 1)
                        Font: =Font.'Open Sans'
                        FontWeight: =FontWeight.Semibold
                        Height: =24
                        Size: =12
                        Text: =ThisItem.DisplayName
                        VerticalAlign: =VerticalAlign.Middle
                        Width: =Parent.TemplateWidth - 20
                        X: =10
                        Y: =4
                  - lblOwnerRowMeta1:
                      Control: Label@2.5.1
                      Properties:
                        Color: =RGBA(139, 151, 143, 1)
                        Font: =Font.'Open Sans'
                        Height: =20
                        Size: =10
                        Text: =ThisItem.Mail & " - " & Coalesce(ThisItem.JobTitle, "")
                        Width: =Parent.TemplateWidth - 20
                        X: =10
                        Y: =28
```

## Part C — OnVisible changes

In the screen's `OnVisible`:

1. Delete `Reset(cmb_Owner1);` — no longer applicable, and not needed anyway (see the
   reactivity note above).
2. Right after the existing `Set(varOwnerDefault, ...)` block, add:
   ```
   Set(varSelectedPerson, First(varOwnerDefault));
   ```
   This seeds the initial display from the same real Office365Users row `varOwnerDefault`
   already resolves to (from the earlier fix), so the owner chip shows the right person
   immediately on load.

## Part D — find-and-replace everywhere else

Search the whole screen's formulas for `cmb_Owner1.Selected` and replace every occurrence
with `varSelectedPerson` (drop the `.Selected` — `varSelectedPerson` already *is* the
selected record, there's no `.Selected` step anymore). This touches:

- The two `Set(varErrOwner, IsBlank(cmb_Owner1.Selected.Mail))` validation lines (Save and
  Save & Close) → `IsBlank(varSelectedPerson.Mail)`
- The `Owner: {Claims: ..., DisplayName: cmb_Owner1.Selected.DisplayName, Email:
  cmb_Owner1.Selected.Mail, ...}` block inside both Patch calls → `varSelectedPerson.DisplayName`
  / `varSelectedPerson.Mail`
- `imgOwnerAvatar1.Image` → `Office365Users.UserPhotoV2(varSelectedPerson.Mail)`, guarded
  the same way (`!IsBlank(varSelectedPerson) && !IsBlank(varSelectedPerson.Mail)`)
- `lblOwnerInitials1.Text` → same formula, `cmb_Owner1.Selected.DisplayName` →
  `varSelectedPerson.DisplayName` (three occurrences inside that one formula)
- Also **delete** the two `Set(varOwnerDefault, Table({DisplayName: cmb_Owner1.Selected...}))`
  lines that used to run after a successful save — not needed anymore, `varSelectedPerson`
  already holds the right value from the moment it was picked and doesn't need re-deriving.

## Status

Not yet applied to the live app — built as the proven fallback if the `Color` property fix
on `ModernCombobox`/reverting to `ComboBox@0.0.51` doesn't pan out. Not yet paste-tested.
