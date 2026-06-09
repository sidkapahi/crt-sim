# 2026-06-09 — Overlay preview controls, in-button effect label, audio defaults

## Asked
1. Put the "Effect" text inside the effect button (not a sibling span).
2. Remove the test-pattern toggle button; keep the test image as the default.
3. Move the aspect-ratio and crop controls into the preview area, top-left,
   4px from the border and 4px between each other.
4. Move the bottom media bar into the preview area, stuck to the bottom with 4px padding.
5. Make the trim track show no indicators by default (handles/playhead were
   visible at the far left before a clip loads).
6. Make the volume slider + icon always visible, defaulting to unmuted.

## Changed (`index.html`)
- **Effect button**: topbar now holds one `.effectbtn` with `<span class="ico">` +
  `<span>Effect</span>`. `updateEffectUI` writes the sparkle icon into `.ico` only
  (so the label survives); boot no longer overwrites the button's innerHTML. Added
  `.effectbtn` CSS; removed the `.topbar .effectlabel` rule.
- **Test-pattern button**: removed `#b_tv` markup. `setTvActive` and the click
  listener are guarded so the now-null `tvBtn` is a no-op.
- **Overlaid controls**: added `.stage-tl` (absolute, top/left 4px, 4px gap) holding
  `#aspectSeg` + `#b_crop`. Moved `#bottomBar` inside `.stage`; `.stage .bottombar`
  is absolute at left/right/bottom 4px. Both overlays get a translucent blurred bg
  and `z-index:6`.
- **Trim indicators**: `.bottombar.disabled` now hides `.thandle/.trange/.playhead`;
  the play button + track stay dimmed but the volume group is no longer dimmed.
- **Audio**: `updateAudioUI` always shows `#audioCtl`; `reflectMuteUI` shows the
  unmuted icon when there's no video; `prevMuted` default `true`→`false` so imported
  clips start unmuted.

## Follow-ups
- Unmuted-by-default relies on user activation from the file picker; if metadata
  loads slowly, autoplay-with-sound may be blocked and the clip waits for a click.
- `.topbar .spacer` / `.topbar .filename` CSS rules are now unused but left in place.
