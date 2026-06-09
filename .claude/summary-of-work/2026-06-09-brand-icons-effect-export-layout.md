# 2026-06-09 — Brand icons in title, Load=Reset style, Effect beside Save, bottom-anchored export

## Asked
1. Move "Buy me a coffee" inline with the title, far right (space-between), drop the
   text → square icon, brand-colored but fitting the system palette.
2. Add a GitHub square icon beside it: black background, white logo.
3. Make the Load button match the Reset button's colors and icon size/style.
4. Remove the top Effect button; move it beside Save at the same size.
5. Tighten the gap between the Export card and its button (group them) and anchor
   the group to the bottom of the sidebar (where the coffee button used to sit).

## Changed (`index.html`)
- **Title row**: wrapped the heading in `.topline` (`justify-content:space-between`)
  with a `.brandicons` cluster on the right. Replaced the full-width green `.coffee`
  link with two 34px `.brandbtn` square icons: `.coffeebtn` (BMC yellow `#ffdd00`,
  tinted bg to fit the dark theme) and `.githubbtn` (black `#0a0a0c` bg, white logo,
  links to the repo). Removed the old `.coffee*` CSS.
- **Load button**: dropped the `var(--panel)` bg override so `.headbtn` inherits the
  base `var(--panel2)` like `.iconbtn`; bumped its icon 14px→17px to match Reset.
- **Effect toggle**: removed the now-empty `.topbar` from the right column (and its
  CSS), moved `#b_effect` into the effects `.pair` next to Save. Added
  `.pair button.active` for the amber "on" state; `updateEffectUI` still drives the
  `.ico` span. Removed the unused `.effectbtn` CSS; kept a generic `.ico` rule.
- **Export grouping**: wrapped the Export card, Export button, and codec bar in
  `.export-group` (`gap:8px`) and gave it `margin-top:auto` (desktop) so it anchors
  to the bottom; the footer now sits below it as a standalone `.footer`.

## Follow-ups
- `--availH` still subtracts a topbar term that no longer exists; left as-is (only
  makes the preview slightly more letterboxed, no overflow risk).
