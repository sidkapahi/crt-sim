# 2026-06-09 — Fix crop/toggle overlay, extract CSS to styles.css

## Asked
1. Fix the broken crop button and the spacing around the aspect-ratio toggle.
2. Move styles into a CSS stylesheet and use classes in the HTML.

## Changed
- **Overlay fix** (`styles.css`): replaced the flaky `align-items:stretch` +
  `aspect-ratio:1` crop sizing with a shared `--ovh:34px` var on `.stage-tl`.
  `.stage-tl .aspect-seg` and `#b_crop` both use `height:var(--ovh)`; the crop is
  `width:var(--ovh)` → a reliable square that matches the toggle height. Change
  `--ovh` in one place to resize both.
- **Toggle spacing** (`styles.css`): `.aspect-seg .chip` is now `display:inline-flex;
  align-items:center; padding:0 8px`, and the chips stretch to the pill height, so the
  label has even space on all sides; pill padding trimmed to `0 2px`.
- **CSS extraction**: moved the entire `<style>` block out of `index.html` into a new
  `styles.css`, linked via `<link rel="stylesheet" href="styles.css">`. Dedented the
  former 2-space block indent.
- **Inline styles → classes**: `style="margin-bottom:14px"` on the Export title →
  `.card > .sectlabel` rule; `#vidHolder` inline offscreen style → `.offscreen`;
  the WebGL-error `<p>` inline style → `.glerror`. `index.html` now has zero
  `style="..."` attributes.
- **README**: updated the "single static page / self-contained `index.html`" wording
  and the file tree to mention `styles.css`.

## Follow-ups
- The active aspect chip is still text-only (amber text, no box) from the prior change.
