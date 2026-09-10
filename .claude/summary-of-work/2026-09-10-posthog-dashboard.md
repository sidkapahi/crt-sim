## Prompt
can you make me a posthog dashboard for crtsim stuff?

1. total exports
2. total uploads
3. upload details (avg size, file extension pie chart)
4. aspect ratio comparison (pie chart)
5. total effect saves
6. total effect loads
7. effect resets
8. export file format numbers (pie chart of file formats selected)
9. effect bar graph (most adjusted)

## Summary
Built a PostHog dashboard ("CRT Sim — Usage", id 2082258) in the shared
KapKit project (id 576339) and added the one property the "avg size" tile
needed.

Dashboard: https://us.posthog.com/project/576339/dashboard/2082258

Tiles (all TrendsQuery insights, date range = all time):
- **Total exports** — count of `crtsim_export_completed` (BoldNumber)
- **Total uploads** — count of `crtsim_video_loaded` (BoldNumber)
- **Avg upload size (MB)** — `avg(toFloat(properties.file_size_bytes))/1048576`
  on `crtsim_video_loaded` (BoldNumber, ` MB` postfix)
- **Uploads by file extension** — `crtsim_video_loaded` broken down by
  `file_ext` (ActionsPie)
- **Aspect ratio selections** — `crtsim_aspect_changed` broken down by
  `aspect` (ActionsPie)
- **Total effect saves** — count of `crtsim_preset_saved` (BoldNumber)
- **Total effect loads** — count of `crtsim_preset_loaded` (BoldNumber)
- **Effect resets** — count of `crtsim_effects_reset` (BoldNumber)
- **Export formats selected** — `crtsim_export_format_selected` broken down
  by `format` (ActionsPie)
- **Most adjusted effects** — `crtsim_effect_changed` broken down by `effect`
  (ActionsBarValue, ranked)

Code changes:
- `index.html`: added `file_size_bytes:f.size||0` to the `crtsim_video_loaded`
  event payload so the avg-size tile has data.
- `README.md`: updated the `crtsim_video_loaded` row of the analytics event
  table to list `file_ext` and `file_size_bytes`.

Follow-ups:
- No `crtsim_*` events have been ingested into project 576339 yet, so the
  tiles read empty until the instrumented app is live with the `phc_` key.
  The `file_size_bytes` property only backfills uploads that happen after
  this `index.html` change ships.
