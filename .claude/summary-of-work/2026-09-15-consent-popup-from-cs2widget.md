## Prompt
can you copy the consent popup from this? https://github.com/sidkapahi/kapkit-cs2overlay and adjust the terms and privacy policy accordingly based on what you're collecting?

(follow-ups: "use the same modal from cs2widget please" / "for priacy policy and terms of service")

## Summary
Ported the cookie consent popup from kapkit-cs2overlay (cs2widget.kapkit.ca) into
CRT Sim: a cookie banner plus the "Privacy & Cookies" and "Terms of Service" modals,
using the same modal design, themed to CRT Sim's amber accent and CSS tokens. The
Privacy/Terms copy was rewritten for what CRT Sim actually collects.

Key changes:
- `index.html`
  - PostHog init now starts opted out: added `opt_out_capturing_by_default: true`
    and `persistence: 'localStorage+cookie'`, so nothing is captured until the
    visitor clicks Allow/Accept.
  - Added a `.consent-root` block before `#vidHolder` with the cookie banner and the
    two modals (Privacy & Cookies, Terms of Service).
  - Added `mountConsent()` in the app IIFE: stores the choice in `localStorage`
    (`crtsim_analytics_consent`), mirrors it to `posthog.opt_in_capturing()` /
    `opt_out_capturing()`, shows the banner until a choice is made, and wires the
    footer `#b_privacy` / `#b_terms` links, the in-modal Accept/Reject, backdrop
    click, and Escape.
- `styles.css`
  - Appended the consent styles (`.cookie-banner`, `.cookie-*`, `.modal-*`,
    `.consent-state`) adapted from the source, mapped to CRT Sim's variables
    (`--card`, `--field`, `--radius`, `--amber`, etc.) with an amber accent.

Privacy copy reflects the actual data: anonymous PostHog usage events (app loaded,
effect settings + values, aspect/crop actions, video metadata only — format,
extension, size, dimensions, duration, never the file or its name — preset save/load,
export actions, brand-link clicks, errors). Emphasizes the video is processed and
exported entirely client-side and never uploaded. Terms cover MIT license, content
responsibility, no warranty, limitation of liability, and a cross-link to Privacy.

Verified in Chromium (Playwright): banner shows on first load, both modals open from
the banner link and the footer links, in-modal Accept persists consent, banner stays
hidden on return visits, Escape/backdrop close, no page errors.

Follow-ups: the banner is pinned bottom-left and overlaps the footer legal links
while it is up (by design — it dismisses on choice, same tradeoff as the source, and
carries its own Privacy Policy link). The PostHog project key/host are unchanged.
