## Prompt
> give me a spot to upload a favicon.png maybe under public/ and also change the site title/tab title to: kapKit - CRT Simulator and use the header.png as the social share/embed

## Summary
- `index.html`:
  - Changed `<title>` from `📺 CRT Simulator` to `kapKit - CRT Simulator`.
  - Added `<link rel="icon" type="image/png" href="public/favicon.png">`.
  - Added a `<meta name="description">` plus Open Graph (`og:type`, `og:site_name`,
    `og:title`, `og:description`, `og:url`, `og:image`, `og:image:width/height`)
    and Twitter (`twitter:card=summary_large_image`, `twitter:title`,
    `twitter:description`, `twitter:image`) tags. The share image points at
    `https://crtsim.com/assets/header.png` (the existing 1920×1080 header.png).
- `public/README.md` (new): created the `public/` folder as the upload spot and
  documented dropping a square `favicon.png` there (path already wired in the
  `<link rel="icon">`).

## Follow-ups
- Upload the actual `public/favicon.png` (square PNG). Until then the tab shows
  the browser default icon.
- `og:url` is hardcoded to `https://crtsim.com/` — update if the canonical
  domain changes.
