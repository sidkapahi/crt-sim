# Development

Everything technical lives here so the [README](../README.md) can stay short.
CRT Sim is a static page — `index.html` (markup, JS, GLSL) plus `styles.css`.
There is no bundler, transpiler, or framework: it's vanilla JS + WebGL.

- [Running locally](#running-locally)
- [How it works](#how-it-works)
  - [Render pipeline](#render-pipeline)
  - [The shaders](#the-shaders)
  - [Export pipeline](#export-pipeline)
- [Project layout](#project-layout)
- [Code map (`index.html`)](#code-map-indexhtml)
- [Adding a new effect](#adding-a-new-effect)
- [Adding an export format](#adding-an-export-format)
- [Vendored libraries & CDN fallbacks](#vendored-libraries--cdn-fallbacks)
- [Fonts](#fonts)
- [Browser support](#browser-support)
- [Contributing](#contributing)

## Running locally

There is no build step.

```bash
git clone https://github.com/sidkapahi/crt-sim.git
cd crt-sim

# serve it (any static server works — a server is needed so the
# vendored worker/lib files load without file:// CORS issues)
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly via `file://` mostly works, but GIF export
(which spins up a Web Worker) and some `fetch`-based lib loading behave better
over `http://`. Use a local server while developing.

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
├── assets/
│   ├── logos/              # brand marks (github, ko-fi, twitch, x, kapkit) — swap your own
│   ├── icons/              # optional UI-icon overrides (empty by default)
│   └── README.md           # which file shows up where
├── libs/
│   ├── gif.js              # GIF encoder
│   ├── gif.worker.js       # GIF encoder worker
│   └── mp4-muxer.min.js    # MP4/MOV muxer
├── docs/
│   ├── DEVELOPMENT.md      # you are here
│   └── ANALYTICS.md        # PostHog event reference
├── LICENSE                 # MIT
└── README.md               # user-facing overview
```

## Code map (`index.html`)

The JS is organized into clearly commented sections. Search for these comment
markers to jump around:

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
| `// ---- reset` | Restore the five effect params to their defaults |
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

## Adding a new effect

1. **Add a uniform** to the scene fragment shader string and a handle in the
   `uScene` lookup.
2. **Wire it in `render()`** with the appropriate `gl.uniform1f(...)` call.
3. **Add UI** — a `.ctl` row in the effects section with a range input
   (`s_yourEffect`, 0–100) and a whole-number field (`n_yourEffect`, 0–100).
   Follow the existing `Scanlines` / `Phosphor` rows as a template. The bind
   maps the 0–100 field to the shader's 0–1 float.
4. **Bind it** in the `// ---- effect sliders + numeric fields` section so the
   slider and number field stay in sync and update the shader.
5. **Include it** in the reset (`// ---- reset`) and preset save/load logic so
   it round-trips.

## Adding an export format

1. Add a chip to `#fmtSeg` with a `data-fmt="..."` attribute.
2. Handle the new value in the export dispatch (`// ---- export range / progress`).
3. Implement the encoder following the pattern of the existing MP4/GIF/WebM
   functions — drive progress via `#codecBar` / `#codecFill` / `#codecLabel`,
   and respect the trim range.

## Vendored libraries & CDN fallbacks

The encoders are vendored under `libs/` so the app works offline, but each also
has a CDN fallback (loaded via `ensureLib(...)`):

| Local | CDN fallback |
| ----- | ------------ |
| `libs/mp4-muxer.min.js` | `cdn.jsdelivr.net/npm/mp4-muxer@5.1.5` |
| `libs/gif.worker.js` | `cdn.jsdelivr.net/npm/gif.js@0.2.0` |

If you bump a vendored version, update both the file in `libs/` **and** the
matching CDN URL constant (`MP4MUXER_SRC`/`MP4MUXER_CDN`,
`GIFJS_WORKER`/`GIFJS_WORKER_CDN`).

## Fonts

The UI is set in **Stratum2** — the `.woff` weights live in `assets/fonts/`
(shared with `kapkit-cs2overlay`) and load via `@font-face`. `JetBrains Mono` is
pulled from Google Fonts as the fallback, and is also used for the `crt-sim` repo
chip and the media-bar timecode. See `assets/fonts/README.md`.

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
