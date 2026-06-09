# CRT-Sim — UI Rebuild Summary

Rebuild of `index.html` to match `MacBook Pro 14_ - 3.svg` / `.png`, per `UI_REBUILD_BRIEF.md`.
Single file, no build step, no framework. The WebGL + export engine is untouched.

## Source of truth & decisions
- Matched the SVG mockup (1512×982, 370px sidebar). The SVG's palette is identical to the
  existing CSS custom properties, so all tokens were reused as-is.
- Confirmed decisions before building:
  - **Left column = one unified `--panel` (#141418) card** containing lighter `--panel2`
    (#1b1b21) inner cards (matches the SVG; inverted from the old transparent block stack).
  - **Spacing = brief tokens** (`--pad:24` / `--gap:24`), not the SVG's tighter ~12px gutters.
  - **Filename default = "Test pattern"**, swaps to the real filename on import.
  - **Buy me a coffee → `https://buymeacoffee.com/sidkapahi`** (new tab).
  - **Slider load defaults = brief item 7** (Curvature 0.60 / Scanlines 0.45 / Phosphor 0.40 /
    Vignette 0.45 / Bloom 0.60), *not* the PNG's illustrative slider positions.

## Layout

**Left column** (single `--panel` card, scrolls on desktop):
- VT323 amber **CRT Sim** title + glow + tagline.
- **IMPORT** button (Phosphor upload icon) + ".mp4, .webm, .mov accepted" caption.
- **EFFECTS** card: reset icon (top-right); Curvature / Scanlines / Phosphor / Vignette / Bloom
  rows (label + range with amber fill + numeric field); **Load** / **Save** preset buttons.
- **EXPORT** card: FILE FORMAT segmented chips **MP4 / GIF / MOV** (MP4 active); caption;
  **Export** button; transient status line.
- **Codec progress bar** (`<codec> // <pct>%`) — visible only during export.
- Green **Buy me a coffee** button.
- Footer: "Designed & Vibe Coded with ❤ in Toronto".

**Right column** (3-row vertical stack):
- **Top bar**: sparkle effect-toggle (before filename) → filename → centered **4:3 / 16:9**
  aspect toggle → top-right **TV** (test pattern) + **crop** icon buttons.
- **Middle**: preview auto-fills remaining space via `min(availW, availH·ar)`; shows the SMPTE
  test pattern through the full effect pipeline by default.
- **Bottom bar**: play/pause → time readout `trimmed / total` (H:MM:SS) → trim/timeline
  scrubber (two amber handles + white playhead) → volume icon + slider.

## Behaviors (wired to the existing engine)
1. **Load / Save** = existing preset load/save logic.
2. **Aspect** = only `640:480` (4:3, default) and `640:360` (16:9).
3. **Sparkle** = effect on/off; filled icon when ON, bold outline when OFF.
4. **Codec progress bar** shows only while exporting (`<codec> // <percent>`), hidden otherwise.
5. **Bottom-bar time** = trimmed-range length / total clip length.
6. **TV icon** toggles the test pattern (and back to the loaded clip if one exists).
7. **Initial load** uses the non-zero effect defaults; **Reset** zeros all five params without
   changing those load defaults.
8. **Timeline** is empty/disabled-but-visible for the test pattern; on import the full range is
   selected and trimmable from either handle at any time (now lives in the bottom bar, live
   during preview).
9. **All icons** are inline Phosphor SVGs (`currentColor`): **bold** when inactive, **filled**
   when active. Icons used: upload, arrow-counter-clockwise, coffee, floppy-disk, folder-open,
   sparkle, television-simple, crop, play, pause, speaker-high, speaker-x.

## Test pattern
Kept the generated SMPTE-bars pattern as the default preview (through the effect pipeline) when
no video is loaded; the TV icon toggles it; loading a clip replaces it; sliders affect it live.

## Removed (video-only, no orphaned code)
- Image input (`fImg` / `accept="image/*"`) and the "Load image" UI + handler.
- Image/frame export entirely (Save-frame PNG/JPG/WebP UI + handler + format `<select>`).
- All `mode==='image'` source branches and now-dead helpers/vars/listeners/CSS
  (`openCropImage`, `cropImg`, `b_recrop`, `b_toggle`, the old `Source`/`Presets`/`Export image`
  blocks, `imgfmt`, `b_png`, etc.).
- Inputs are now `video/mp4,video/webm,video/quicktime` only, with graceful rejection.
- Outputs = MP4 / GIF / MOV; WebM remains the internal fallback only.
- WebGL frame rendering itself was **not** removed — only the user-facing still-image features.

## Untouched engine (carried over verbatim)
- WebGL pipeline: `F_SCENE` (curve, scanlines, phosphor mask, vignette), bloom passes
  (`pBright → pBlur → pFinal`), crop UV uniforms (`cropMin` / `cropMax`).
- Export engine: MP4/MOV via WebCodecs `VideoEncoder` (H.264 → VP9 → AV1) + AAC via
  `AudioEncoder`, muxed with mp4-muxer; WebM via MediaRecorder fallback; GIF via gif.js (silent);
  encoder libs from `libs/` with CDN fallback; `exporting` flag preserved; all audio paths kept.
- Presets, aspect handling, sliders + numeric fields, trim logic (handles, playhead,
  `enforceTrim` / `trimTick`), and the preview auto-fill math.

## Interaction states
Every interactive element has default / `:hover` / `:active` / selected-active states plus
`:focus-visible` outlines, with ~140ms transitions. Selected states: active format chip, active
aspect, effect-on sparkle, test-pattern-on TV, crop-active.

## Responsive
Two columns on desktop with the right-column bottom bar pinned under the preview; below 980px it
collapses to one column (sidebar stacked above the preview), preview still fills, bottom bar
stays usable.

## Verification performed
- Rendered headlessly (Chromium/Edge) at 1512×982 — composition matches the mockup; live WebGL
  test pattern renders through the shader by default.
- Inspected sidebar, top bar, and bottom bar crops — icons, chips, states, and the
  disabled-but-visible timeline all correct.
- No app-level console errors; JS passes `node --check`; no remaining image references.
- Responsive collapse at <980px verified (single column, preview still fills).

## Not yet exercised
Real video **import → trim → crop → export** was not run end-to-end (no sample clip / no
WebCodecs in the headless env). Those code paths are unchanged from the working original and the
new wiring is verified, but a quick manual pass with a real clip is recommended.
