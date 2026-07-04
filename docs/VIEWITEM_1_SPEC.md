# ViewItem_1 — Target Design Spec

This is the durable record of what `ViewItem_1.yaml` needs to become. It was decided across several sessions via Claude Artifact mockups (ephemeral, Claude-session-only URLs — not readable outside that session) and is transcribed here so any future builder (human or AI) has a real source of truth instead of relying on chat history.

**Status as of 2026-07-05: not yet built.** `Main_1.yaml` (see `yaml/Main_1.yaml`) IS built and merged, and already implements the shared stepper/completion/anomaly logic described below in real PowerFx — read that file as a working reference for the patterns, not just this doc.

---

## Scope

This is a full redesign of the `ViewItem` screen (currently `yaml/ViewItem.yaml`), same relationship as `Main_1` is to `Main`: build as a **new screen `ViewItem_1`**, paste-test independently, don't touch the real `ViewItem` until it's confirmed working. `Main_1`'s `+ New Entry` and card-click handlers already `Navigate(ViewItem_1, ...)`, so once `ViewItem_1` exists the two new screens work together as a pair.

## Locked design decisions

1. **Link cards (right side)** — one real full-card click target (`Rectangle` control per the Bible's rounded-panel pattern, or `Classic/Button` — see `known-bad-patterns.md` for which controls support radius), icon/text/actions layered on top in Tree view order so action buttons (edit/delete/open-in-app) stay independently clickable on top of the card-wide click handler. Icons: user's own icons8 set (Word/Excel/PPT/PDF/HTML), stretched to square. Real formula for file-type detection and the `colLinks` loading already exists in `yaml/ViewItem.yaml` (`Selector.OnSelect`/`btnResultSelect.OnSelect`, `AddColumns` with `FileExt`) — reuse it, don't rebuild it.

2. **Left-side form — do NOT use `Form`/`TypedDataCard`.** Can't restyle it (no rounded corners, no custom backgrounds). Rebuild as standalone `Label` + `Classic/TextInput`/`Classic/ComboBox`/`Classic/Toggle`/`Classic/DatePicker` controls (exact versions confirmed in `yaml/ViewItem.yaml`: `Classic/DropDown@2.3.1`, `ComboBox@0.0.51`, `DatePicker@0.0.46`, `Classic/DatePicker@2.6.0`, `Classic/ComboBox@2.4.0`) inside hand-built `GroupContainer` cards, saved via `Patch('Policy Proof Tracker', ..., {...})` instead of `SubmitForm(Form1)`. Accepted trade-off: lose free required-field validation UI and edit/view mode switching — rebuild those manually if needed.

3. **Top bar** — full width, matches the real app's existing `TopButtons_1.Width: Parent.Width` (confirmed correct, not legacy cruft).

4. **10-day warning banner** — must occupy the *exact same slot* as the top bar (identical X/Y/Width/Height), toggling `Visible` on each so one replaces the other with zero layout shift. Real field: `'Planned Publish Date'` within 10 days and `!'High Priority'` triggers the warning (see `yaml/ViewItem.yaml` line ~298 for the exact existing condition to reuse).

5. **High Priority** (`'High Priority'`, real field, Yes/No) → gold border on the Policy Details card (`RGBA(212, 175, 55, 1)`, ~2px) + a flag-icon badge in the top bar.

6. **Top Item** (`'Top Item'`, real field — NOT "Top Newsletter Item", that was an early misremembering) → same gold border treatment on the Newsletter Details card + a **different** icon badge (newspaper/star, not the flag) in the top bar — deliberately different from High Priority's icon so both stay visually distinguishable when both are true at once. `Main_1.yaml`'s `badgeTopItem1`/`flagHighPriority1` controls already show this exact pattern for the card list — reuse the same icon choices for consistency.

7. **Approval pipeline** — real field `'Approved by Pillar Lead (Ready to Release)'` (Yes/No), display label **"Approved by Pillar Lead (Ready to Release)"**. Real confirmation blurb already exists in the app (`ApprovedPillarInfo`/`HtmlText1` in `yaml/ViewItem.yaml`, shown when that box is ticked) — copy it verbatim (spelling/grammar/links/formatting checklist + Cadets Branch Writing Guide link), do not rewrite it.

8. **3-stage approval stepper** (Pillar Lead → Approved For Release By set → Published) — NOT a red/amber/green traffic light (wrong semantics for a checklist that only ever progresses one direction). Green-fill progress stepper. Appears in **two places**, kept in sync: the full labelled version inside the "Approval and Release" card, and a persistent compact dot-and-line version directly under the top bar (so status is visible without scrolling). **`Main_1.yaml` already implements this exact stepper (`stepDot1`/`stepDot2`/`stepDot3`, `stepLine1`/`stepLine2` in the card template) — copy that logic directly rather than re-deriving it.**

9. **Completion (mint) + anomaly (amber) colours** — when all three stages are done, every dot/node in both stepper versions AND all three cards (Policy Details, Newsletter Details, Approval and Release) shift to a soft mint (`RGBA(159, 224, 189, 1)`, matches `Main_1.yaml`'s `--complete` equivalent) — explicitly not a neon/lime green, a distinct softer shade from the normal in-progress green (`RGBA(11, 74, 54, 1)`). Anomaly detection: if a *later* stage is done while an *earlier* one is still unticked (items occasionally get force-published skipping a stage), that earlier dot turns amber (`RGBA(226, 150, 60, 1)`) instead of staying grey — flags the out-of-sequence gap. Amber only applies to the three stepper dots, never to card borders (cards only get the mint treatment, and only when genuinely all three stages are complete). **Exact PowerFx for this is already written and working in `Main_1.yaml`'s `stepDot1`/`stepDot2`/`stepDot3` `Fill` formulas — copy verbatim, they're already scoped correctly.**

10. **Connector-line direction bug (already fixed once, don't reintroduce it):** each stepper's connecting line must reflect the state of the node it leads *FROM*, not the node it leads *TO*. A previous mockup pass got this backwards (the Pillar Lead→Approved line only went green once Approved was done, ignoring that Pillar Lead had already finished). `Main_1.yaml`'s `stepLine1`/`stepLine2` already implement this correctly — copy them, don't re-derive from first principles.

## Explicitly deferred (do not build these, just leave as-is)

- **Owner people-picker** (`Office365Users.SearchUser` + `UserPhotoV2` + custom Combo Box template) — user says the existing real implementation already works and is "quite unique/complex." Just restyle visually later if asked; do not rebuild the underlying logic.
- **Document Type dropdown** — full real choice list plus a new "External Consultation" option is wanted eventually, but explicitly deferred — leave the field as the real existing dropdown for now.
- Need the app's real `AppTheme.palette.themePrimary` hex (Themes panel) to make the accent colour match exactly instead of the approximated `RGBA(11, 74, 54, 1)` used throughout `Main_1.yaml` — not yet obtained from the user.

## Known dead code to remove while in here

- The `Unsaved`/"You have unsaved changes" banner inside `ReleaseDate_1` in the real `yaml/ViewItem.yaml` has `Visible: false` hardcoded — abandoned, never wired up, safe to delete when rebuilding this area.

## Real field names confirmed (from `yaml/ViewItem.yaml`, do not guess these)

`Title`, `'Document Type'` (choice, `.Value`), `Owner` (record: `Claims`/`Department`/`DisplayName`/`Email`/`JobTitle`/`Picture`), `'Planned Publish Date'`, `'High Priority'`, `'Reason for High Priority'`, `'Top Item'`, `'External consultation required'`, `'External consultation details'`, `'Item summary'`, `'Title for Newsletter'`, `Audience`, `'Expiry Date'`, `Remarks`, `Published`, `'Published Date'`, `'Approved by Pillar Lead (Ready to Release)'`, `'Approved For Release By'` (choice, `.Value`, `"Not Yet Approved"` is the unset sentinel value — not blank).
