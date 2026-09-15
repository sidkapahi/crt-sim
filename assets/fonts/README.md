# Fonts — Stratum2 UI typeface

The UI is set in **Stratum2** (the "stratum" redesign). The three webfont weights
below are committed here and pulled in by `@font-face` in `styles.css`:

| Weight        | File (this folder)                | Used for |
| ------------- | --------------------------------- | -------- |
| Regular (400) | `stratum2-regular-webfont.woff`   | tagline, Privacy / Terms footer links |
| Medium (500)  | `stratum2-medium-webfont.woff`    | slider labels (SCANLINES …) and value boxes |
| Bold (700)    | `stratum2-bold-webfont.woff`      | "CRT Sim" title, buttons, section labels, chips, FX PREVIEW pill, aspect labels |

These `.woff` files were carried over from
[`kapkit-cs2overlay`](https://github.com/sidkapahi/kapkit-cs2overlay)
(`public/fonts/`) so the two projects share the same typeface. If a weight is ever
missing, the UI falls back to `JetBrains Mono` (also used for the `crt-sim` repo
chip and the media-bar timecode).

## Swapping in a smaller / fuller kit

`.woff` is supported by every modern browser, so nothing else is required. To
shrink load size, drop a matching `.woff2` next to each `.woff`
(`stratum2-<weight>-webfont.woff2`) and add it as the first `src` in each
`@font-face` rule in `styles.css`:

```css
src:url('assets/fonts/stratum2-bold-webfont.woff2') format('woff2'),
    url('assets/fonts/stratum2-bold-webfont.woff')  format('woff');
```

Hard-refresh after replacing a file (the browser caches fonts).
