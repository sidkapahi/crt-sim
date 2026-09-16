# public — drop your favicon here

This folder holds static files served at the site root. Right now it's the
home for the browser-tab icon.

## `favicon.png` — the browser-tab icon

Drop a square PNG named **`favicon.png`** in this folder and it shows up as the
tab/bookmark icon. `index.html` already references it:

```html
<link rel="icon" type="image/png" href="public/favicon.png">
```

So there's no code change needed — just add the file, keep the name, and
hard-refresh the page.

Tips:
- Make it **square** (e.g. 256×256 or 512×512). Browsers downscale it to
  ~16–32px, so keep the mark bold and simple.
- Transparent background is fine.
- Until you add the file, the tab shows the browser's default icon.
