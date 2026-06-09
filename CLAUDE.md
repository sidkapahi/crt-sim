# CLAUDE.md

## Summary of work

Whenever you finish a chunk of work for me — in particular when you commit
changes or open/update a pull request — write a **new** markdown file
summarizing what you did to `.claude/summary-of-work/`.

- One new file per task; do not overwrite previous summaries.
- Name it `YYYY-MM-DD-<short-slug>.md` (e.g. `2026-06-07-move-claude-folders.md`).
  If multiple in one day, add a `-2`, `-3`, … suffix.
- Start each file with a `## Prompt` section containing the exact user request
  that triggered the work, then a `## Summary` section with what changed
  (key files) and any follow-ups.
- These are notes for me. `.claude/summary-of-work/` is tracked in git
  (the rest of `.claude/`, e.g. settings, stays ignored), so do commit
  the summary files — but do **not** mention these folders in the README.

### Summary file format
```md
## Prompt
<exact user request, quoted verbatim>

## Summary
<what changed, key files, any follow-ups>
\```

## Commit & PR naming — be literal

Describe what the diff mechanically changes, not why it matters or what it
enables. Name the actual files/functions/behavior touched. If unsure, read
the diff and restate it plainly — do not summarize upward into a goal.

### Banned words (inflation)
comprehensive, robust, properly, fully, seamless, significantly, enhance,
improve, optimize (unless a perf number is in the diff), powerful, complete,
various, support for (prefer the literal verb)

### Rules
- One imperative verb + concrete object: "Add", "Remove", "Rename", "Move"
- If the diff does N distinct things, list N lines — don't abstract into one
- Never claim behavior, testing, or coverage not present in the diff
- Scope tag comes from the path touched, not a theme

### Commit titles
- feat(crop): clamp aspect ratio to source dimensions
- fix(export): release WebCodecs encoder on abort
- refactor: extract shader pass setup into createPass()

### PR titles — same rule
GOOD: fix(crt): align phosphor mask sampling to integer pixels
BAD:  fix: properly fix mask rendering issues   ← vague, inflated, "properly"

GOOD: Add trim-handle keyboard nudge (arrow keys, 1-frame step)
BAD:  Enhance trim UX with improved keyboard support   ← "enhance/improved"
```

**What changed:** removed the `prompts/` reference throughout, added the `## Prompt / ## Summary` format block so the original request is always captured at the top of each summary file. You can also delete the `prompts/` folder from your repo now if it's empty.