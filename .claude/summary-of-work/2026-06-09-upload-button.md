# Rename Import button to Upload

## Asked
Change the icon and text of the Import button to "Upload", and space it out
more from the title and the Effects panel.

## Changed
- `index.html`
  - Swapped the button SVG to the Phosphor "UploadSimple" glyph (up arrow over a tray).
  - Changed the button label from `Import` to `Upload`.
  - Wrapped the button block in `.import-block` with `margin:12px 0`, which adds
    to the sidebar's existing `gap:18px` for ~30px spacing above (title) and
    below (Effects panel).

## Follow-ups
- None. The underlying file input still accepts .mp4/.webm/.mov; only the
  button's presentation changed (the JS handler `b_import` is unchanged).
