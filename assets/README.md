# Assets — your spot to drop in icons & logos

This folder holds the brand logos and (optionally) icons the CRT Simulator UI
loads. **To use your own, just overwrite the matching file, keeping the same
file name** — the app references these paths directly, so no code change is
needed. Hard-refresh the page after replacing a file.

## `logos/` — sidebar link row + footer

| File               | Where it shows up                                         | Notes |
| ------------------ | --------------------------------------------------------- | ----- |
| `github.svg`       | Dark **crt-sim** repo chip (link row)                     | seeded from kapkit-cs2overlay |
| `kofi.svg`         | Blue **Ko-fi** chip (link row)                            | seeded from kapkit-cs2overlay |
| `twitch.svg`       | Purple **Twitch** chip (link row)                         | seeded from kapkit-cs2overlay |
| `x.svg`            | Black **X / Twitter** chip (link row)                     | **placeholder** — replace with your real X mark |
| `kapkit.png`       | The **kapKit** lockup in the sidebar footer               | seeded from kapkit-cs2overlay (mark + white wordmark) |

Tips:
- Any `.svg` works — an icon-only mark or a full wordmark. The chips size the
  mark to ~16–18px, so it won't overflow.
- The chips have their own background colour (Ko-fi blue, Twitch purple, X
  black, repo dark), so a logo with its own brand colours paints as-is. A
  monochrome mark should be **white** so it reads on the coloured chip.
- Keep each SVG's `viewBox` so it scales cleanly.
- `kapkit.png` is shown ~30px tall on a dark background; its wordmark is white.

## `icons/` — functional UI glyphs (optional override)

The functional interface glyphs (upload, download, crop, play/pause, volume,
reset) currently ship **inline** in `index.html` as [Phosphor](https://phosphoricons.com)
SVGs, so this folder starts empty. If you'd rather control those too, drop your
files here and tell me which ones to wire up — say the word and I'll switch the
markup to load them from `assets/icons/`.

## Link URLs

The Twitch and X chips and the footer **Privacy Policy** / **Terms of Service**
links ship with placeholder `#` hrefs (search `TODO: link` in `index.html`).
Point them at your real URLs when ready.
