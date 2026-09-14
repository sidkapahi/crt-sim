## Prompt

> ok I made a design to follow the other kapkit products similarly! keep functionality the same but here are a few changes:
>
> 1. the values of the effects are now out of 100 percentage no more decimal and when people type in a value it should round to a whole number
> 2. all tracking is the same so keep posthog active
> 3. there is a default seen here: https://www.figma.com/design/Og5JkdPINVpIuggvjIu06X/CRT-Sim?node-id=59-26
> 4. then after someones uploaded something, here is everything active: https://www.figma.com/design/Og5JkdPINVpIuggvjIu06X/CRT-Sim?node-id=63-360
> 5. don't make any assumptions or make svgs yourself. I will provide you with all icons, logos just make a spot in the repo for me. you can probably pull some of the sidebar stuff from here: https://github.com/sidkapahi/kapkit-cs2overlay

(File was re-shared as `MV29M8L068n0JnbKdU7Mzp` for read access; default = node `59-26`, active = node `63-360`.)

## Summary

Restyled the CRT Simulator to the kapkit design system (matching the two Figma
frames) while keeping the amber CRT identity and all existing functionality.

### Effect values → whole-number percentages (change #1)
- `index.html`: effect number fields are now `min=0 max=100 step=1` integers
  (were `0–1`, 2 decimals). `field()` reworked so typed input is parsed,
  **rounded to the nearest whole number**, clamped 0–100, and applied live
  (shader fns still receive 0–1). `setF()` now takes a 0–100 percentage.
- Presets bumped to `version:2` (store 0–100 ints); v1 presets (0–1 floats)
  are auto-scaled ×100 on load.

### Tracking (change #2)
- PostHog init untouched; all existing `crtsim_*` events kept. `crtsim_effect_changed`
  now logs the 0–100 integer. Added `crtsim_twitch_clicked` / `crtsim_x_clicked`
  for the two new link chips.

### Layout / visual redesign (changes #3 & #4)
- `styles.css` rewritten to the kapkit palette (`#191919` card, `#2b2b2b`
  border, `#0f0f0f` fields, `#676767` line, `#101411` pill, amber `#ffb454`,
  green `#83d24a`, red `#e96262`), page gradient, `JetBrains Mono` UI font
  (VT323 kept for the title).
- Sidebar rebuilt: title + tagline, **link row** (crt-sim / Ko-fi / Twitch / X
  chips), UPLOAD, EFFECTS (reordered SCANLINES · PHOSPHOR · VIGNETTE · BLOOM ·
  CURVE, integer value boxes, `RESET TO DEFAULT`), LOAD / SAVE pair, export
  (MP4 · GIF · MOV chips + green DOWNLOAD), footer (kapKit lockup + Privacy /
  Terms links).
- Stage rebuilt as a bordered card: top bar (crop + 16:9/4:3), preview, media
  bar (play · timecode · trim track · volume).
- **Default vs active state**: `#app.no-video` dims/disables export, stage
  top-bar, media bar, RESET and SAVE until a clip is loaded (driven by the
  existing `setBottomEnabled`), matching the two frames. Effects/upload/load/
  links stay live on the test pattern.

### Assets spot (change #5)
- New `assets/logos/` seeded from `kapkit-cs2overlay` (`github.svg`, `kofi.svg`,
  `twitch.svg`, `kapkit.png`) + an `x.svg` **placeholder**, referenced via
  `<img>`. `assets/icons/` left for optional UI-icon overrides. `assets/README.md`
  documents each slot. No brand SVGs were invented beyond the flagged X placeholder.

### Deviations to confirm
- **CRT on/off compare toggle removed** — the Figma has no slot for it; `on`
  stays 1, sliders + RESET drive the look. Easy to re-add if wanted.
- **Export/crop/aspect/media gated on upload** — matches the design's dimmed
  "default" frame (drops test-pattern export). Say the word to keep any of
  these live without a clip.
- **Timecode readout kept** in the media bar (design omits it) so trim duration
  stays visible.
- **Link URLs**: Twitch, X, Privacy Policy, Terms of Service ship as `#`
  placeholders (`TODO: link` in `index.html`).

### Files
- `index.html` — head font, full sidebar/stage markup, JS (field/setF, reset,
  presets, link tracking, video gating, effect-toggle removal).
- `styles.css` — full rewrite to the kapkit system.
- `assets/` — new logos + README.
- `README.md` — layout tree, font, value-range and reset notes updated.

Verified in headless Chromium: both states render to match the frames, typed
`73.6` rounds to `74` (slider synced), slider→field sync works, no JS errors.
