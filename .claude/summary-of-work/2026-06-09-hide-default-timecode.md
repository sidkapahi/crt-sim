# 2026-06-09 — Hide the default timecode in the media bar

## Context
The 6 preview-control changes were implemented and pushed from another machine
(commit `496b059`). Reviewing it against the original request, task 5 ("no time
indicators by default") was only partly met: the trim handles/range/playhead were
hidden when the media bar is disabled, but the `00:00 / 00:00` timecode still showed.

## Changed (`index.html`)
- Added `.bottombar.disabled .timecode` to the `display:none` rule so the timecode is
  also hidden until a clip is loaded. With no video the bottom bar now shows only the
  play button and the (always-visible) volume control.

## Verified
- Headless Edge screenshot: default media bar has no timecode and no trim handles;
  volume icon + slider still visible.
