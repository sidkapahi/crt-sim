# 2026-06-09 — Move Load button, relocate Export button, change defaults, swap topbar label

## Asked
1. Move the Load button to the left of the Reset button, across from the Effects title.
   Take the Export button out of the Export card and place it below with the same
   spacing as between the Effects and Export cards; give it an icon and all-caps
   label to mimic the Upload button.
2. Default Curvature to 0; default aspect ratio to 16:9 and place 16:9 left of 4:3.
3. Remove the filename title at the top; add an "Effect" label next to the sparkle icon.

## Changed (`index.html`)
- Effects card header: wrapped Reset in a `.headbtns` group and added `#b_loadpreset`
  (the Load button) to its left. Removed Load from the `.pair`, leaving Save. Added
  `.headbtns` / `.headbtn` CSS.
- Export card: removed `#b_export` from the card. Re-added it as a direct `.sidebar`
  child (so it inherits the 18px flex gap) with class `import-btn`, a download icon,
  and the `.export-label` span (uppercase via `.import-btn`).
- Curvature default: slider `value` 60→0, number `value` 0.60→0.00, `setCurve(0.60)`→`setCurve(0)`.
- Aspect default 16:9: reordered chips so `640:360` (16:9) is first and active,
  `640:480` (4:3) second. Canvas `height` 480→360, root `--ar` 1.3333→1.7778,
  `currentAspect` and `applyAspect` fallback set to `640:360`.
- Topbar: replaced `<span class="filename" id="fileName">` with
  `<span class="effectlabel">Effect</span>`. Guarded `fileNameEl` with `|| {}` so the
  three `fileNameEl.textContent=` writes are harmless no-ops. Added `.effectlabel` CSS.

## Follow-ups
- None. `.export-btn` and `.filename` CSS rules are now unused but left in place.
