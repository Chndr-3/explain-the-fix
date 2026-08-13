# explain-the-fix

A [Claude Code](https://claude.com/claude-code) skill that explains a just-finished coding task (bug fix, refactor, new feature) as a small, proportional diagram instead of a wall of text.

Some people read code changes fastest as prose. Others get there faster from a picture — a cause chain, a before/after, a sequence — and lose time wading through paragraphs to reconstruct what a three-node diagram would have shown instantly. This skill is for the second group: it reads the diff, picks the smallest diagram shape that actually fits the change, and skips the long-form recap.

It classifies the diff into one of six shapes — trivial fix, single-system cause chain, cross-system cause chain, before/after refactor, sequence flow, or a vertical deep dive (cause → fix → why it works, for when you ask *why* a fix works rather than just what changed) — and renders 2–8 nodes sized to match the change. No shape is picked without a diff to justify it, and no diagram exceeds 8 nodes regardless of how big the change was.

## Install

Drop this directory into your Claude Code skills folder (e.g. `~/.claude/skills/explain-the-fix`).

## Use

Triggers automatically once a coding task finishes, or on request: "explain what you just did," "visualize the fix," "show me what changed," or "why does this fix work" for the deep-dive shape.

## How it renders

Its own house style — no dependency on `diagram-design` or any other skill. Inline SVG + CSS, self-contained HTML, no external assets, no JavaScript:

- **Drafting-table look**: dark mode by default (CSS-only toggle to light, bottom-right), dot-grid background, hairline strokes, monospace subtitles for file:line references, a tick mark on the resolved/fix node. Amber is reserved — it only ever marks the node or row that changed.
- **Clickable glossary**: technical terms in a diagram (checked against `references/glossary.md`) open a small modal on click, which falls through to a collapsible drawer listing every term used, highlighted with a soft wash animation instead of a flat color block.
- **Text sizing**: box widths and line-wrapping are computed per diagram, not copied from a fixed template — see `references/renderer.md` for the sizing pass.

`references/style-reference.html` is the canonical working example (all shapes, the glossary system, the theme toggle) — open it directly in a browser to see the current look. See `SKILL.md` for the full classification table and `references/renderer.md` for the shape-by-shape rendering rules.

## License

MIT — see `LICENSE`.
