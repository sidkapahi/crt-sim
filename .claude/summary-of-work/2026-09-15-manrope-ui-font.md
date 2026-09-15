## Prompt

> lets do manrope, explain how tailwind/shadcn fits into supabase and how its being used?
> [earlier: "whats the closest responsive google font replacement for circular? satoshi or something?" → chose Manrope for UI text, keep a mono for readouts]

## Summary

Switched crt-sim's UI text from JetBrains Mono to **Manrope** (a free Google
Fonts stand-in for Supabase's Circular), while keeping JetBrains Mono for
numeric readouts / code and VT323 for the display title.

### Changes
- `index.html` — Google Fonts link now loads `Manrope:wght@400;500;600;700;800`
  alongside VT323 and JetBrains Mono.
- `styles.css`:
  - Added font tokens to `:root`: `--font-ui` (Manrope + system fallbacks),
    `--font-mono` (JetBrains Mono / IBM Plex Mono), `--font-display` (VT323).
  - `body` font-family → `var(--font-ui)` (was JetBrains Mono).
  - `h1` font-family → `var(--font-display)`.
  - Set `var(--font-mono)` on `input.num`, `.chip-repo span`, `.timecode`,
    `.codeclabel` so readouts and the repo chip stay monospace.

### Notes / follow-ups
- Manrope loads over Google Fonts; system-ui fallback stack covers offline.
- Mono pairing kept as JetBrains Mono (already bundled). Supabase's own mono is
  Source Code Pro if an exact match is ever wanted.
- Verified in headless Chromium at 1440×900: UI text renders Manrope, value
  boxes / repo chip / timecode render JetBrains Mono, layout unchanged.
