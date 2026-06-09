# 2026-06-09 — Square overlay buttons, crop-left, bare media bar, working volume

## Asked
1. Make the two title brand buttons the same height / square (keep 1:1).
2. Make the crop button square (height matches, 1:1).
3. Move the crop button to the left of the aspect toggle; remove the background
   from the active aspect chip only.
4. Bump the top-left overlay block's padding from the preview edge to 8px.
5. Media bar: remove its background, make the volume slider look like the effects
   sliders, show the audio icon in default mode, and let it work with no video loaded.

## Changed (`index.html`)
- **Brand buttons**: added `aspect-ratio:1` + `padding:0` to `.brandbtn` and
  `align-items:center` to `.brandicons` so both stay 34×34 squares.
- **Stage overlay**: `.stage-tl` offset 4px→8px. Reordered DOM so `#b_crop` comes
  before `#aspectSeg`. `#b_crop` is now 34×34 (square); `.aspect-seg` is a fixed
  34px-tall pill (`align-items:center`, `padding:0 4px`) so it matches the crop
  button. Added `.stage-tl .chip.active{ background:transparent }` (keeps amber
  text/border, drops the fill).
- **Media bar**: `.stage .bottombar` background/border/blur removed (transparent);
  controls now float over the preview.
- **Volume slider**: `.audioctl input[type=range]` gets the same amber `--fill`
  gradient track as the effects sliders; `reflectRange(volSlider)` is called on
  input, on mute changes, and in `updateAudioUI`/boot.
- **Audio works without a clip**: `reflectMuteUI` falls back to `prevMuted/prevVol`
  when there's no `vid`. The mute button and volume slider now update the stored
  `prevMuted/prevVol` (and the icon/fill) when no video is loaded, so the state
  carries over to the next imported clip.

## Follow-ups
- Active aspect chip keeps its amber border as the selected indicator; only the
  fill was removed per the request.
