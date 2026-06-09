# 2026-06-09 — Even aspect-chip padding, text-only toggle, auto-square crop

## Asked
- Make the padding around the aspect-toggle chip text even on all sides.
- Make the toggle hover state just highlight the text (no box).
- Keep the crop button the same height as the toggle and square as the toggle resizes.

## Changed (`index.html`)
- `.aspect-seg .chip` padding `6px 13px` → `7px` (equal on all sides).
- `.stage-tl` switched to `align-items:stretch` so the crop button and the aspect
  pill share the same (content-driven) height.
- `.stage-tl #b_crop` is now `width:auto; height:auto; aspect-ratio:1`, so it stays
  square and tracks the toggle's height automatically (was fixed 34×34).
- `.stage-tl .aspect-seg` lost its fixed `height:34px`; now `padding:3px`, content-sized.
- `.stage-tl .chip.active` now also clears `border-color` (background was already
  transparent), so the toggle is text-highlight only: selected = amber text,
  hover = amber text (`.chip:hover` was already color-only), inactive = muted.

## Follow-ups
- Crop sizing relies on flex-stretch + `aspect-ratio:1` (modern browsers). The
  active option no longer has a border box — selection is shown purely by amber
  text; say so if you'd rather keep a border on the selected chip.
