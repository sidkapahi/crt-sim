# CRT-Sim — UI Rebuild Brief

## Source of truth
Match `MacBook_Pro_14__-_3.svg` (and the PNG) in the project — 1512×982, 369px sidebar.
Open the SVG to read exact coordinates, sizes, and hex fills; use the PNG for overall
composition. There is no Figma MCP — work only from these files and the spec below.

## Build quality
Use the `frontend-design` skill and follow it throughout — production-grade, accessible,
token-driven frontend, not a quick mock. Read `index.html` fully first, then give me a short
plan and wait for my OK before implementing. Reuse the existing CSS custom properties
(--bg #0a0a0c, --panel #141418, --panel2 #1b1b21, --line #2c2c34, --amber #ffb454,
--text #e9e6df, --muted #8b8b94, --green #5ef08a, --leftw 370, --pad 24, --gap 24).

## Layout (two columns, 24px gutter all around, 24px gap)
LEFT column (~370 wide, scrolls):
- Title "CRT Sim" (VT323, amber) + tagline.
- IMPORT button (Phosphor upload icon) with caption ".mp4, .webm, .mov accepted".
- EFFECTS card: a RESET icon button top-right; sliders + numeric fields for Curvature,
  Scanlines, Phosphor, Vignette, Bloom; then two buttons "Load" and "Save" (= load/save preset).
- EXPORT card: FILE FORMAT segmented chips MP4 / GIF / MOV (MP4 active); the export caption;
  an "Export" button.
- Codec progress bar (e.g. "AV1 // 56.7%") — only visible during an export.
- Green "Buy me a coffee" button (text/icon #5EF08A on #1F422A).
- Footer: "Designed & Vibe Coded with ❤ in Toronto".

RIGHT column (fills remaining width + height), a vertical stack:
- TOP bar: sparkle icon (effect toggle) BEFORE the filename, then the filename
  (e.g. "filename.mp4"); centered segmented aspect toggle 4:3 / 16:9 (4:3 active); top-right
  two icon buttons: TV (test pattern) and crop.
- MIDDLE: the preview, auto-filling all remaining space while preserving aspect ratio
  (keep min(availW, availH*ar)). Shows the Test pattern by default; a loaded video replaces it.
- BOTTOM bar (this column only, not full-window): play/pause, time readout "trimmed / total"
  (e.g. 00:45:34/35:56:09), the trim/timeline scrubber with two amber handles, then volume
  icon + slider.

## Behaviors (apply exactly)
1. "Load" / "Save" = load/save preset (existing preset logic).
2. Aspect toggle has ONLY 4:3 and 16:9; 4:3 is the default active state.
3. Sparkle icon = effect on/off (replaces the old "Toggle effect"); sits before the filename;
   filled when effect is ON, bold outline when OFF.
4. Codec progress bar appears ONLY while exporting (shows "<codec> // <percent>"); hidden
   otherwise.
5. Bottom-bar time = trimmed-range length / total clip length.
6. TV icon (top-right) toggles the Test pattern (replaces the old "Test pattern" button).
7. The app LOADS with the existing non-zero effect defaults (Curvature 0.60, Scanlines 0.45,
   Phosphor 0.40, Vignette 0.45, Bloom 0.60). The RESET icon (top-right of EFFECTS) sets all
   five params to 0. Reset zeroes them; it does NOT change the initial load defaults.
8. Timeline: empty before import; on import the full range is highlighted as selected, and the
   user shortens the trim from either side via the two handles. Trim works anytime.
9. ALL icons use Phosphor: BOLD weight when default/inactive, FILLED weight when active.

## Keep Test pattern (not photo functionality)
Keep the generated SMPTE-bars test pattern. It's the DEFAULT preview shown in the right column
(through the full effect pipeline) when no video is loaded; the TV icon toggles it. Loading a
video replaces it; clearing the video falls back to it. Sliders affect it live. The timeline is
empty/disabled (but visible) while it shows.

## Remove ALL photo functionality — video only
- Remove the image input (fImg / accept="image/*") and any "load image" UI + handler.
- Remove image/frame EXPORT entirely (Save-frame PNG/JPG/WebP UI + handler + format selector).
- Remove image-only source branches (mode==='image', Image() into texture) and any now-dead
  helpers/vars/listeners/CSS. No orphaned code.
- Inputs = .mp4/.webm/.mov only (accept "video/mp4,video/webm,video/quicktime"); reject other
  types gracefully. Outputs = MP4 / GIF / MOV (WebM stays only as the internal fallback).
- Do NOT remove WebGL frame rendering itself — only the user-facing still-image features.

## Do NOT change (engine) — flag instead of touching
- WebGL pipeline: F_SCENE (curve, scanlines, phosphor mask, vignette), bloom passes
  (pBright→pBlur→pFinal), crop UV uniforms (cropMin/cropMax).
- Export engine as-is: MP4/MOV via WebCodecs VideoEncoder (isConfigSupported across H.264
  profiles → VP9 → AV1) + AAC audio via AudioEncoder, muxed with mp4-muxer; WebM via
  MediaRecorder (vp9/vp8 + opus) with original audio mixed via Web Audio; GIF via gif.js
  (silent); encoder libs from libs/ first with CDN fallback; export auto-captures the trimmed
  range (preserve the `exporting` flag). Keep all audio paths.
- Presets, aspect handling, sliders + numeric fields, trim logic (handles, playhead,
  enforceTrim/trimTick), preview auto-fill math.

## Interaction states (every interactive element)
default, :hover, :active (pressed), and a selected/active state for current choices (active
format chip, active aspect, effect-on sparkle, test-pattern-on TV). Add :focus-visible outlines.
~120–150ms transitions. Match the mockup's per-state treatments.

## Responsive
Desktop: two columns as above, right-column bottom bar pinned under the preview. Below the
breakpoint (currently 980px): collapse to one column gracefully; preview still fills; the
preview's bottom bar stays usable.

## Constraints
Single index.html unless you have a strong reason to split (ask first). No build step, no
framework. Inline all Phosphor SVGs (no CDN — opens as a local file), using currentColor.

## Done =
- Matches the mockup. Every control has hover/pressed/active/focus states; icons bold→filled.
- Right column: sparkle+filename / aspect / TV+crop on top, preview filling the middle,
  play+time(trimmed/total)+timeline+volume on the bottom.
- Reset zeroes all effects; codec progress only shows during export; aspect is 4:3 + 16:9 only.
- Video-only: no image input or image export anywhere; Test pattern kept via TV icon.
- All engine features still work: load clip, sliders, crop, trim, export MP4 (audio),
  GIF (silent), MOV (audio); WebM fallback intact.