## Prompt

> can you pull the fonts from this github?  https://github.com/sidkapahi/kapkit-cs2overlay

## Summary

Copied the Stratum2 webfonts out of `sidkapahi/kapkit-cs2overlay` into this repo
and pointed the `@font-face` rules at them, so the "stratum" redesign now ships
with the real typeface (no upload step needed).

### What was pulled
- From `kapkit-cs2overlay/public/fonts/`: `stratum2-regular-webfont.woff`,
  `stratum2-medium-webfont.woff`, `stratum2-bold-webfont.woff` (weights 400/500/700,
  ~20 KB each, real `wOFF` files — verified via md5 against the source). Only `.woff`
  exists in that repo (no `.woff2`).
- Copied to `assets/fonts/` keeping the same filenames for cross-project consistency.

### Wiring
- `styles.css`: `@font-face` `src` now points at the three committed
  `stratum2-*-webfont.woff` files (dropped the earlier missing-`.woff2` refs so
  there are no 404s). Family/weights unchanged (`Stratum2` 400/500/700); JetBrains
  Mono stays the fallback + repo chip + timecode face.
- Docs updated to say the fonts are now committed and give the `.woff2` upgrade
  path: `assets/fonts/README.md`, `assets/README.md`, `README.md`.

### Verified
- Headless Chromium (SwiftShader WebGL): Stratum2 loads and renders across the
  title/tagline/labels/buttons/pill; layout matches the Figma frame; no JS errors.

Pushed to `claude/jolly-fermat-ftjbx4` (updates draft PR #7).
