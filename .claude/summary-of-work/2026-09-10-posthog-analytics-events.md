## Prompt
can you add posthog analytic events to this? make sure the events start with "crtsim" since I'm going to use the same posthog account. I want things like upload event triggered, ko-fi button click, github clicked, file format for export, exports, effect saves, refreshes, loads, aspect ratio, crop and values for each tune option

## Summary
Added client-side PostHog analytics to the single-page app. All custom events
are prefixed `crtsim_`.

Key changes in `index.html`:
- New PostHog loader snippet in `<head>`, keyed off two swappable globals
  (`CRTSIM_POSTHOG_KEY`, `CRTSIM_POSTHOG_HOST`). `posthog.init(...)` only runs
  when the key looks like a `phc_…` project key. Points at the existing PostHog
  project (`us.i.posthog.com`).
- Added a `track(name, props)` helper inside the main IIFE that no-ops if
  PostHog isn't loaded (wrapped in try/catch), plus an `aspectLabel()` helper
  that maps the internal `640:360`/`640:480` size strings to `16:9`/`4:3`.
- Wired events:
  - `crtsim_app_loaded` (boot)
  - `crtsim_kofi_clicked`, `crtsim_github_clicked` (brand links)
  - `crtsim_upload_triggered`, `crtsim_video_loaded`, `crtsim_upload_rejected`
  - `crtsim_test_pattern_toggled`, `crtsim_effect_toggled`
  - `crtsim_effect_changed` — fires on slider/field `change` (commit) per tune
    option, with `{effect, value}`. `field()` gained a `label` param.
  - `crtsim_effects_reset` (Reset/refresh button)
  - `crtsim_aspect_changed` (16:9 / 4:3 chips)
  - `crtsim_crop_applied`, `crtsim_crop_reset` (crop apply / fill frame)
  - `crtsim_preset_saved`, `crtsim_preset_loaded` (Save / Load presets)
  - `crtsim_export_format_selected` (MP4/GIF/MOV chips)
  - `crtsim_export_started`, `crtsim_export_completed`, `crtsim_export_stopped`,
    `crtsim_export_failed` — completion/failure wired into the WebM, MP4/MOV and
    GIF paths plus `encoderFallback`.

`README.md`: added an **Analytics** subsection under Developing documenting the
key/host swap and the full event table.

Follow-ups:
- The embedded key is the project's public `phc_` ingestion key (safe in
  client code). Swap `CRTSIM_POSTHOG_KEY`/`CRTSIM_POSTHOG_HOST` to route events
  to a dedicated crt-sim project if desired.
- `crtsim_effect_changed` intentionally fires on commit (release/blur), not on
  every drag tick, to avoid flooding.
