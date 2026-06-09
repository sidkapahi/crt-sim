# CRT Sim

A browser-based CRT/OLED shader playground. Load a clip, dial in the curve, scanlines, phosphor mask, bloom and vignette, then crop, trim and export it — MP4, MOV or GIF. No install, no build step, everything runs client-side.

![Version](https://img.shields.io/badge/version-v1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)

> [!WARNING]
> This project was built with AI assistance (vibe coded). While it works, the code hasn't been professionally audited — use at your own risk. If you get a chance, feel free to review the code before deploying. PRs and fixes are always welcome!

---

## Table of contents

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [How it works](#how-it-works)
  - [Render pipeline](#render-pipeline)
  - [The shaders](#the-shaders)
  - [Export pipeline](#export-pipeline)
- [Project layout](#project-layout)
- [Code map (`index.html`)](#code-map-indexhtml)
- [Developing](#developing)
  - [Adding a new effect](#adding-a-new-effect)
  - [Adding an export format](#adding-an-export-format)
  - [Vendored libraries & CDN fallbacks](#vendored-libraries--cdn-fallbacks)
- [Browser support](#browser-support)
- [Contributing](#contributing)
- [License](#license)

---

## What it does

- **Live CRT shader** running on WebGL — five tunable effects:
  - **Curvature** — barrel distortion of the screen surface
  - **Scanlines** — horizontal beam lines
  - **Phosphor** — RGB shadow-mask / aperture-grille pattern
  - **Vignette** — darkened corners
  - **Bloom** — glow from bright regions (threshold → blur → composite)
- **Import** your own video (`.mp4`, `.webm`, `.mov`) or use the built-in **test pattern**.
- **Aspect ratios** — 4:3 and 16:9.
- **Crop** — interactive crop box locked to the chosen aspect.
- **Trim** — pick an in/out range; export covers the trimmed range only.
- **Audio** — preview with mute/volume; audio is mixed into MP4/MOV exports.
- **Presets** — save/load your effect settings as JSON.
- **Export** — MP4, MOV (H.264 with VP9/AV1 fallback) or GIF (silent).

Everything happens in the browser. No files are uploaded anywhere.

## Quick start

There is no build step. The app is a static page — `index.html` plus a
`styles.css` stylesheet.

```bash
# clone
git clone https://github.com/sidkapahi/crt-sim.git
cd crt-sim

# serve it (any static server works — a server is needed so the
# vendored worker/lib files load without file:// CORS issues)
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly via `file://` mostly works, but GIF export
(which spins up a Web Worker) and some `fetch`-based lib loading behave
better over `http://`. Use a local server while developing.

## How it works

### Render pipeline

The source — either a `<video>` element or a procedurally drawn 2D test
pattern — is uploaded into a WebGL texture every frame. A `requestAnimationFrame`
loop (`loop()`) uploads the current source and calls `render()`.

```
source (video / test pattern)
        │  uploadSource() → srcTex
        ▼
  ┌──────────────┐
  │ scene shader │  curvature, scanlines, phosphor, vignette, crop
  └──────┬───────┘
         │ (if bloom enabled)
         ▼
  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
  │  brightness  │ → │ blur (H pass)│ → │ blur (V pass)│
  │  threshold   │   └──────────────┘   └──────┬───────┘
  └──────────────┘                              │
         ▼                                       ▼
  ┌────────────────────────────────────────────────┐
  │ final composite  (scene + bloom * strength)      │ → canvas
  └────────────────────────────────────────────────┘
```

Bloom is a classic threshold → separable Gaussian blur (two passes) →
additive composite. When bloom is disabled the blur passes are skipped and
`strength` is set to `0`.

### The shaders

All GLSL lives inline as JS strings near the top of the `<script>` block:

| Shader | Source var | Purpose |
| ------ | ---------- | ------- |
| Vertex | `VS` | Full-screen quad, passes UVs |
| Scene  | `uScene` uniforms | Curve, scanlines, phosphor mask, vignette, crop window, effect on/off |
| Bright | `uBright` | Extracts pixels above a brightness threshold |
| Blur   | `uBlur` | Directional Gaussian blur (run twice: horizontal, vertical) |
| Final  | `uFinal` | Adds scene + bloom |

Slider values are **0–100 in the UI** but normalized to **0–1** before being
sent to the shader (see the matching `s_*` range inputs and `n_*` number
inputs, e.g. `s_curve` / `n_curve`).

### Export pipeline

Three independent encoders, picked by the format chips (`#fmtSeg`):

- **MP4 / MOV** — [WebCodecs](https://developer.mozilla.org/docs/Web/API/WebCodecs_API)
  `VideoEncoder` (H.264, falling back to VP9/AV1 if H.264 isn't supported)
  muxed with [`mp4-muxer`](https://github.com/Vanilagy/mp4-muxer). Audio is
  encoded as AAC and muxed in. See `// ---- MP4 / MOV via WebCodecs + mp4-muxer`.
- **GIF** — [`gif.js`](https://github.com/jnordberg/gif.js) running in a Web
  Worker (silent). See `// ---- GIF via gif.js`.
- **WebM (fallback)** — `MediaRecorder` over `canvas.captureStream()`, with
  audio mixed via Web Audio. Used internally when WebCodecs isn't available.
  See `// ---- WebM via MediaRecorder`.

Exports cover the **trimmed range only**.

## Project layout

```
crt-sim/
├── index.html              # markup, JS, GLSL
├── styles.css              # all styling
├── libs/
│   ├── gif.js              # GIF encoder
│   ├── gif.worker.js       # GIF encoder worker
│   └── mp4-muxer.min.js    # MP4/MOV muxer
├── LICENSE                 # MIT
└── README.md               # you are here
```

Markup, JS, and GLSL live in `index.html`; all styling lives in `styles.css`.
There is no bundler, transpiler, or framework — it's vanilla JS + WebGL.

## Code map (`index.html`)

The JS is organized into clearly commented sections. Search for these
comment markers to jump around:

| Marker | What lives there |
| ------ | ---------------- |
| `var gl = cv.getContext('webgl'...)` | WebGL setup, shader compilation |
| `VS` / `uScene` / `uBright` / `uBlur` / `uFinal` | GLSL sources & uniform handles |
| `function render()` | Per-frame draw + bloom passes |
| `function loop()` | RAF loop, source upload |
| `// ---- crop overlay` | Interactive crop box |
| `// ---- import (video only)` | File import handling |
| `// ---- test pattern <-> video` | Test-pattern toggle |
| `// ---- aspect` | 4:3 / 16:9 switching |
| `// ---- effect sliders + numeric fields` | Slider ↔ number-field binding |
| `// ---- reset` | Zero all five effect params |
| `// ---- presets (Load / Save)` | JSON preset import/export |
| `// ---- Preview audio` | Mute / volume (Web Audio) |
| `// ---- export format (chips)` | Format selection |
| `// ---- export range / progress` | Export orchestration |
| `// ---- MP4 / MOV via WebCodecs + mp4-muxer` | H.264/AAC export |
| `// ---- GIF via gif.js` | GIF export |
| `// ---- WebM via MediaRecorder` | WebM fallback export |
| `// ---- boot` | Startup |

The CSS at the top uses CSS custom properties (`:root { --bg, --amber, ... }`)
for theming — change the palette in one place.

## Developing

### Adding a new effect

1. **Add a uniform** to the scene fragment shader string and a handle in the
   `uScene` lookup.
2. **Wire it in `render()`** with the appropriate `gl.uniform1f(...)` call.
3. **Add UI** — a `.ctl` row in the `Effects` card with a range input
   (`s_yourEffect`, 0–100) and a number input (`n_yourEffect`, 0–1). Follow
   the existing `Curvature` / `Scanlines` rows as a template.
4. **Bind it** in the `// ---- effect sliders + numeric fields` section so the
   slider and number field stay in sync and update the shader.
5. **Include it** in the reset (`// ---- reset`) and preset save/load logic so
   it round-trips.

### Adding an export format

1. Add a chip to `#fmtSeg` with a `data-fmt="..."` attribute.
2. Handle the new value in the export dispatch (`// ---- export range / progress`).
3. Implement the encoder following the pattern of the existing MP4/GIF/WebM
   functions — drive progress via `#codecBar` / `#codecFill` / `#codecLabel`,
   and respect the trim range.

### Vendored libraries & CDN fallbacks

The encoders are vendored under `libs/` so the app works offline, but each
also has a CDN fallback (loaded via `ensureLib(...)`):

| Local | CDN fallback |
| ----- | ------------ |
| `libs/mp4-muxer.min.js` | `cdn.jsdelivr.net/npm/mp4-muxer@5.1.5` |
| `libs/gif.worker.js` | `cdn.jsdelivr.net/npm/gif.js@0.2.0` |

If you bump a vendored version, update both the file in `libs/` **and** the
matching CDN URL constant (`MP4MUXER_SRC`/`MP4MUXER_CDN`,
`GIFJS_WORKER`/`GIFJS_WORKER_CDN`).

Fonts (`VT323`, `IBM Plex Mono`) are pulled from Google Fonts.

## Browser support

- **Chrome / Edge** — best experience; full WebCodecs H.264 MP4/MOV export.
- **Firefox / Safari** — WebGL preview works; MP4/MOV export depends on
  WebCodecs availability and falls back to VP9/AV1 or WebM where needed.
- Requires WebGL. GIF export requires Web Workers.

## Contributing

PRs and issues welcome. Because there's no build step, contributing is just:

1. Edit `index.html` / `styles.css` (and `libs/` if needed).
2. Serve locally and test in Chrome + at least one other browser.
3. Keep the no-build, client-only architecture intact.
4. Match the existing code style — vanilla JS, clear section comments, the
   `--var` CSS theme.

> Reminder: this codebase is AI-assisted and hasn't been formally audited.
> Reviews and hardening PRs are especially appreciated.

## License

[MIT](LICENSE) © 2026 Sid

---

<p align="center">Designed &amp; Vibe Coded with ♥ in Toronto</p>
