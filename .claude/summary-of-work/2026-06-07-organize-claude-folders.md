# Organize .claude folders + summary-of-work convention

**Date:** 2026-06-07

## Asked
- Move `SUMMARY OF WORK/` into `.claude/` and rename to `summary-of-work`.
- Move `prompts/` into `.claude/`.
- Don't mention these folders in the README — they're personal tracking notes.
- Have Claude always write a new summary-of-work `.md` whenever it commits a
  PR or does work.

## Changed
- `git mv` of `SUMMARY OF WORK/` → `.claude/summary-of-work/` and
  `prompts/` → `.claude/prompts/`.
- Reverted an earlier README edit that listed these folders in the project
  layout (per request to not mention them).
- `git rm --cached` on `.claude/` so the moved files are local-only — the whole
  `.claude/` directory is already gitignored.
- Added root `CLAUDE.md` with the standing instruction to write a new dated
  summary file here for each task.

## Notes / follow-ups
- `.claude/` (incl. these summaries and prompts) is gitignored → not pushed.
- An earlier commit already pushed the moved files to branch
  `claude/organize-summary-work-folder-TqQyH` (PR #2); this commit removes them
  from tracking again.
