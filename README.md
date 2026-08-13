# explain-the-fix

A [Claude Code](https://claude.com/claude-code) skill that explains a just-finished coding task (bug fix, refactor, new feature) as a small, proportional diagram instead of a wall of text.

It classifies the diff into one of five shapes — trivial fix, single-system cause chain, cross-system cause chain, before/after refactor, or sequence flow — and renders 2–8 nodes sized to match the change. No shape is picked without a diff to justify it, and no diagram exceeds 8 nodes regardless of how big the change was.

## Install

Drop this directory into your Claude Code skills folder (e.g. `~/.claude/skills/explain-the-fix`).

## Use

Triggers automatically once a coding task finishes, or on request: "explain what you just did," "visualize the fix," "show me what changed."

## How it renders

Uses the `diagram-design` skill for styled output if it's installed, otherwise falls back to a minimal unstyled inline SVG. See `SKILL.md` for the full classification table and `references/` for worked examples per shape.

## License

MIT — see `LICENSE`.
