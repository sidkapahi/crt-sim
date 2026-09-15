## Prompt

> ok, I redesigned it using stratum but the better proportions, make the changes and make a folder for me to upload stratum!
>
> https://www.figma.com/design/MV29M8L068n0JnbKdU7Mzp/CRT-Sim?node-id=78-162&t=78sFn2DeSnThXR2l-4

## Summary

Reworked the CRT Simulator to the refined "stratum" Figma frame (node `78:162`):
the primary UI face is now **Stratum2**, with tightened proportions. Functionality
is unchanged (WebGL shader, presets, crop/trim/export, PostHog, no-clip gating).

### Font: Stratum2 (the "stratum" change)
- `styles.css`: added three `@font-face` blocks for Stratum2 Regular/Medium/Bold
  pointing at `assets/fonts/Stratum2-*.woff2` (+`.woff`), `font-display:swap`.
- New tokens `--font-ui` (`'Stratum2','JetBrains Mono',…`) and `--font-mono`
  (`'JetBrains Mono',…`). Body now uses `--font-ui`; JetBrains Mono is kept as
  the fallback (until the licensed files are dropped in) and for the `crt-sim`
  repo chip and the media-bar timecode (stable digits).
- Title `h1` changed from VT323 amber-glow 42px to **Stratum2 Bold 32px, white
  (`#f0f0f0`), tracking -1.28px**. Dropped VT323 from the Google Fonts `<link>`
  (nothing else used it).

### Folder to upload Stratum2
- New `assets/fonts/` with `README.md` listing the exact filenames to drop in
  (`Stratum2-Regular/Medium/Bold` `.woff2`/`.woff`), where each weight is used,
  and the note that Stratum2 is licensed so the files aren't committed.

### Better proportions (Figma 78:162)
- `index.html`: **LOAD/SAVE moved above the sliders** (was below); tagline copy
  updated to the new frame's wording.
- Sidebar spacing: `.side-top` / `.side-bottom` / `.sidebar` gaps → 40px;
  `.sliders` gap → 20px.
- Buttons: UPLOAD / DOWNLOAD height 44 → **48px**, icons 16 → 18/16. LOAD/SAVE
  now padding-sized (12px) with gap 12.
- Slider value box: 44×30 15px → **47×34 16px** Medium; range thumb 14 → 12px.
- Export format chips: taller (`padding:16px 12px`); active MP4 ground token
  `--mp4-bg` `#5b4120` → **`#422f17`** (also used by the FX PREVIEW pill).
- Stage: padding 16 → **20px**. FX PREVIEW pill radius 9 → 6, 12.5px, sparkle
  20px, active ground `#422f17`. Aspect labels 15 → 16px, gap 16; crop button
  44 → **36px** with `#676767` border, radius 6. Media bar play/track/mute
  44 → **36px**, volume 96 → 98px. Footer links 13 → 14px.
- Updated the desktop `--availH`/`--availW` canvas-fit estimate for the new
  paddings/heights.

### Docs
- `assets/README.md`: added a `fonts/` section.
- `README.md`: font note now describes Stratum2 + the JetBrains Mono fallback.

### Notes / follow-ups
- Stratum2 files are **not** in the repo — the UI renders in JetBrains Mono until
  you drop them into `assets/fonts/` (see that folder's README). Verified in
  headless Chromium (SwiftShader WebGL): layout matches the frame, test pattern
  renders, no JS errors.
- Kept the existing **no-clip gating** (export/aspect/crop/media/RESET/SAVE dim
  until a clip loads) and the **media-bar timecode** — both are deviations from
  what the single mock frame shows; say the word to change either.
