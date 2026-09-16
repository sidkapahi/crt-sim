# Analytics

Usage is tracked with [PostHog](https://posthog.com). The snippet lives in the
`<head>` of `index.html`; the project key and host are set on two variables you
can swap to point at your own project:

```js
window.CRTSIM_POSTHOG_KEY  = 'phc_…';                 // project API key
window.CRTSIM_POSTHOG_HOST = 'https://us.i.posthog.com';
```

If `CRTSIM_POSTHOG_KEY` isn't a `phc_…` key, `posthog.init(...)` is skipped and
no events are sent. Analytics is opted out by default and stays that way until
the visitor accepts the cookie banner.

All custom events are prefixed `crtsim_` and go through the `track(name, props)`
helper, which no-ops when PostHog isn't loaded.

| Event | Fires when | Key props |
| ----- | ---------- | --------- |
| `crtsim_app_loaded` | page boots | `aspect`, `width`, `height` |
| `crtsim_kofi_clicked` / `crtsim_github_clicked` | brand link clicked | — |
| `crtsim_upload_triggered` | Upload button clicked | — |
| `crtsim_video_loaded` | a clip's metadata loads | `file_type`, `file_ext`, `file_size_bytes`, `source_width/height`, `duration_seconds` |
| `crtsim_upload_rejected` | unsupported file picked | `file_type`, `file_ext` |
| `crtsim_test_pattern_toggled` | TV icon toggled | `active` |
| `crtsim_effect_toggled` | CRT effect on/off | `enabled` |
| `crtsim_effect_changed` | a tune slider/field is committed | `effect`, `value` |
| `crtsim_effects_reset` | Reset (refresh) clicked | — |
| `crtsim_aspect_changed` | 16:9 / 4:3 switched | `aspect`, `width`, `height` |
| `crtsim_crop_applied` / `crtsim_crop_reset` | crop applied / filled | `crop_x/y/width/height`, `aspect` |
| `crtsim_preset_saved` / `crtsim_preset_loaded` | preset Save / Load | five effect values, `aspect` |
| `crtsim_export_format_selected` | format chip clicked | `format` |
| `crtsim_export_started` | export begins | `format`, `mode`, `duration_seconds`, `width`, `height`, `aspect` |
| `crtsim_export_completed` | file saved | `format`, `codec` |
| `crtsim_export_stopped` | export stopped early | `format`, `progress` |
| `crtsim_export_failed` | encoder/finalize failed | `format`, `codec`, `reason` |
