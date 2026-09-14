## Prompt

> everything looks so big compared to my mockups
>
> font and spacing don't seem like they line up to the figma frames I showed you
>
> https://cs2widget.kapkit.ca/ should be a reference since i used the same figma for this one
>
> don't max width it to 1250px

## Summary

The Figma artboards are 2055px wide, so implementing their raw px 1:1 rendered
everything oversized. Re-scaled the control + type system to match the live
kapkit customizer at cs2widget.kapkit.ca (source: `kapkit-cs2overlay`), which
renders the same Figma at real browser scale. Per follow-up, the layout is
**not** width-capped — it still fills the viewport.

### Scale aligned to the cs2widget reference
- Sidebar `--leftw` 336px, `--pad` 24px, `--gap` 16px, radii 16/8, sidebar
  padding 20/24, section gap 32 — matching cs2overlay's tokens.
- Control heights 48→44px (upload/load/save/download, play/track/mute/crop),
  fx toggle + stage icons 24→22.
- Type: title 48→42 (VT323), value boxes 16→15, aspect 16→15, chips 14,
  labels 12, tagline 12 — matching cs2overlay's 12–15px range.

### Not capped
- The layout keeps `padding` + `grid`, no `max-width` / centering — fills the
  viewport as before. (An earlier revision capped it at 1250px; removed at the
  user's request.)

### Files
- `styles.css` — token + control/type re-scale.

Verified in headless Chromium at 1920×1080 (fills width, canvas ~1484px) and
1440×900. Only `styles.css` changed; markup and behavior untouched.
