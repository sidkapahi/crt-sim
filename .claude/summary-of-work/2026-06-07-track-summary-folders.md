# Track summary-of-work and prompts in git

**Date:** 2026-06-07

## Asked
- Make the `.claude/summary-of-work/` (and `prompts/`) notes persist instead of
  living only in the ephemeral cloud container.

## Changed
- `.gitignore`: switched `.claude/` → `.claude/*` and added negations
  `!.claude/prompts/` and `!.claude/summary-of-work/`, so those two folders are
  tracked while the rest of `.claude/` (settings, etc.) stays ignored.
- Re-added both folders to git.
- Updated `CLAUDE.md` note to reflect that summary files are now committed.

## Notes / follow-ups
- Verified `git check-ignore` still ignores `.claude/settings.local.json`.
- User plans to make the repo private until launch, then re-add these folders to
  `.gitignore` if they want them excluded again.
