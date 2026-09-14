## Prompt

> use this for the text sizes for buttons, privacy policy/terms of service, website subtitle
>
> * website: [cs2widget.kapkit.ca](https://cs2widget.kapkit.ca/)
> * github: https://github.com/sidkapahi/kapkit-cs2overlay

## Summary

Matched three text sizes in `styles.css` to the cs2widget customizer
(source: `kapkit-cs2overlay`, `src/customizer/customizer.css`), which renders
the same Figma at real browser scale.

### Values pulled from the reference
- Buttons — reference `.zip-btn` is `font-size: 14px`.
- Privacy Policy / Terms of Service — reference `.foot-link` is `font-size: 12px`.
- Website subtitle — reference `.setup-sub` is `font-size: 12px`.

### Changes in `styles.css`
- `button` base `font-size` 12.5px → 14px (covers `.btn-primary`, `.loadsave
  button`, `.btn-download`, `.croprow button`; icon-only buttons unaffected).
- `.foot-link` (Privacy Policy / Terms of Service) `font-size` 13px → 12px.
- `.tagline` (subtitle) already 12px — matches `.setup-sub`, left unchanged.

### Files
- `styles.css` — two `font-size` edits.
