## Prompt

> ok uploaded a better x.svg and here is an updated design. I moved the crop/16:9 button for the fx preview. Let me know if you have questions:
>
> 1. default state: https://www.figma.com/design/MV29M8L068n0JnbKdU7Mzp/CRT-Sim?node-id=78-12
> 2. after upload: https://www.figma.com/design/MV29M8L068n0JnbKdU7Mzp/CRT-Sim?node-id=78-162
> 3. button states for effects preview:
>    1. while on: https://www.figma.com/design/MV29M8L068n0JnbKdU7Mzp/CRT-Sim?node-id=78-312
>    2. while off: https://www.figma.com/design/MV29M8L068n0JnbKdU7Mzp/CRT-Sim?node-id=78-316

## Summary

Follow-up to the kapkit redesign (PR #4). Reworks the stage top bar per the
updated Figma frames and brings back the CRT on/off toggle as **FX PREVIEW**.

### Stage top bar
- Split into left/right: **FX PREVIEW** toggle on the left, **aspect (16:9 / 4:3)
  + crop** moved to the right (`justify-content: space-between`).
- `index.html`: new `.stage-top` markup with `#b_effect` (`.fxbtn`) on the left
  and `.stage-right` wrapping `#aspectSeg` + `#b_crop`.

### FX PREVIEW toggle (re-added CRT on/off)
- Restored the effect on/off control that the previous pass had dropped — it now
  has a home. `updateEffectUI()` + the `#b_effect` click handler are back
  (`on` flips 0/1, `crtsim_effect_toggled` still fires).
- `styles.css` `.fxbtn`: OFF = `#191919` bg / `#676767` border / `#e9e6df`
  text; ON (`.active`) = `#563d1e` bg / `#ffb454` border+text; sparkle icon
  (Phosphor `sparkleFill` when on, `sparkleBold` when off), 24px.
- Not gated on upload — it drives the test pattern too, so it stays lit in the
  default (no-clip) state, matching frame 78-12.

### Gating tweak
- `styles.css`: the `#app.no-video` dim now targets `.stage-right` (aspect +
  crop) instead of the whole `.stage-top`, so FX PREVIEW stays interactive
  before a clip is loaded.

### x.svg
- Kept the official X mark uploaded to the branch (commit `d04418e`); local
  fast-forwarded onto it before making these changes.

### Files
- `index.html` — stage-top restructure; restore effect-toggle JS.
- `styles.css` — `.stage-top`/`.stage-right`/`.fxbtn` styles; no-video target.

Verified in headless Chromium: default + active states match frames 78-12 /
78-162; FX PREVIEW toggles active↔inactive; no JS errors.
