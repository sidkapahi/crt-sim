## Prompt
can you re-do the readme to be more customer facing? like this one, and leave space above for a header image I'll provide: https://github.com/sidkapahi/kapkit-cs2overlay

## Summary
Rewrote `README.md` in the customer-facing style of `kapkit-cs2overlay`, and
moved the deep technical content into `docs/` (mirroring that repo's
`docs/DEVELOPMENT.md` + `docs/ANALYTICS.md` split).

Key changes:
- `README.md` — new centered `<div align="center">` header with a header-image
  placeholder (`assets/header.png`, with an HTML comment marking the space to
  drop it), title, tagline, badges (MIT / WebGL / no-build / PRs welcome), and
  `How-To Guide · Report a Bug` nav links. New sections: **Overview** (friendly
  "what you get" list + a `> [!NOTE]` about being AI-built/unaudited),
  **How-To Guide** (numbered: get it running → load a clip → tune → frame →
  export), **Browser support**, and a short **For Developers** section that
  links out to the docs.
- `docs/DEVELOPMENT.md` (new) — running locally, render pipeline, shaders,
  export pipeline, project layout, code map, adding an effect, adding an export
  format, vendored libs/CDN fallbacks, fonts, browser support, contributing.
- `docs/ANALYTICS.md` (new) — the PostHog event reference table.

Notes / follow-ups:
- No hosted demo exists (the app runs locally), so I kept the CTA honest with
  nav links instead of inventing a hosted "Use App" button like the reference's
  customizer URL.
- The header image is not committed — user will provide `assets/header.png`.
  The README references it now so it renders once dropped in.
