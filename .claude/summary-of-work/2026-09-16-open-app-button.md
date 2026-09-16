## Prompt
I made changes to the readme and added the header image, can you pull the latest updates to the branch and add the button from the other github I sent you with this link: https://crtsim.com?utm_source=github&utm_medium=readme

## Summary
Pulled the latest branch updates (user's header image at `assets/header.png` and
their README title/copy tweaks), then added a call-to-action button matching the
`kapkit-cs2overlay` "Use Overlay" button.

- `assets/open-app-button.svg` (new) — green pill button (`#6fce72` bg,
  `#0d100c` text/arrow) reading **OPEN CRT SIM** with a right arrow, styled to
  match the reference repo's button.
- `README.md` — inserted the button as `<a href="https://crtsim.com?utm_source=github&utm_medium=readme">`
  wrapping the SVG (`height="54"`), placed between the badges and the
  How-To Guide / Report-a-Bug nav links, mirroring the reference layout.

Note: the reference button draws its label as vector paths; this one uses an SVG
`<text>` element with a sans-serif stack, which renders reliably on GitHub.
