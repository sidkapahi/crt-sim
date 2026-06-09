# Bottom-align coffee button with footer

## Asked
Bottom-align the "Buy me a coffee" button into the same div/frame as the
"Designed & Vibe Coded with ♥ in Toronto" footer text.

## Changed (`index.html`)
- Wrapped the existing `.coffee` `<a>` and `.footer` `<p>` in a new
  `<div class="signoff">` so they form one group.
- Added `.signoff{ display:flex; flex-direction:column; gap:18px; }` to
  preserve the prior 18px spacing between the two elements.
- Moved the desktop bottom-align from `.sidebar .footer{ margin-top:auto; }`
  to `.sidebar .signoff{ margin-top:auto; }` so the whole group (coffee +
  footer) pins to the bottom of the sidebar instead of just the footer.

## Effect
On desktop (≥980px) the coffee button now sits directly above the footer,
both pinned to the bottom of the sidebar. On mobile they stay in normal flow.

## Follow-ups
None.
