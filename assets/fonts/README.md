# Fonts — drop the Stratum2 files here

The UI is set in **Stratum2** (the "stratum" redesign). Stratum2 is a **licensed
font**, so its files are **not** committed to this repo — you supply your own
copy from your license. Until the files are present, the UI falls back to
`JetBrains Mono` (loaded from Google Fonts), so nothing looks broken in the
meantime.

## What to drop in

`styles.css` loads three weights via `@font-face`, each expecting a `.woff2`
(preferred) with a `.woff` fallback. **Keep these exact file names** — the app
references these paths directly, so no code change is needed:

| Weight        | Files (this folder)                              |
| ------------- | ------------------------------------------------ |
| Regular (400) | `Stratum2-Regular.woff2`, `Stratum2-Regular.woff` |
| Medium (500)  | `Stratum2-Medium.woff2`, `Stratum2-Medium.woff`   |
| Bold (700)    | `Stratum2-Bold.woff2`, `Stratum2-Bold.woff`       |

Where each weight shows up:

- **Bold (700):** the "CRT Sim" title, UPLOAD / LOAD / SAVE / DOWNLOAD buttons,
  section labels (EFFECTS, RESET TO DEFAULT), MP4/GIF/MOV chips, FX PREVIEW pill,
  aspect labels.
- **Medium (500):** effect slider labels (SCANLINES …) and the value boxes.
- **Regular (400):** the tagline and the Privacy / Terms footer links.

## Notes

- **`.woff2` is enough on its own** — the `.woff` line is just an older-browser
  fallback. If you only have `.woff2`, drop those three and ignore the rest;
  the missing `.woff` sources are harmless.
- Got `.ttf` / `.otf` instead? Either convert them to `.woff2` (e.g. with
  `woff2` / `fonttools`), or tell me and I'll point the `@font-face` `src` at
  the extensions you have.
- After adding files, hard-refresh the page (the browser caches fonts).
- `JetBrains Mono` stays wired as the fallback and is also used for the small
  `crt-sim` repo chip and the media-bar timecode (kept monospace on purpose).
