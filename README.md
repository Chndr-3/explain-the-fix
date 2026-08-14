# visualize-skills

Two [Claude Code](https://claude.com/claude-code) skills, one house style: **visualize-the-fix** explains a just-finished coding task (bug fix, refactor, new feature) as a small diagram, and **visualize-the-plan** does the same for a plan before you build it. Both trade a wall of text for a small, proportional diagram.

Some people read changes and plans fastest as prose. Others get there faster from a picture — a cause chain, a before/after, a sequence — and lose time wading through paragraphs to reconstruct what a three-node diagram would have shown instantly. These skills are for the second group: they read the diff or the plan, pick the smallest diagram shape that actually fits, and skip the long-form recap.

## visualize-the-fix

Reads the diff, classifies it into one of six shapes — trivial fix, single-system cause chain, cross-system cause chain, before/after refactor, sequence flow, or a vertical deep dive (cause → fix → why it works, for when you ask *why* a fix works rather than just what changed) — and renders 2–8 nodes sized to match the change. No shape is picked without a diff to justify it, and no diagram exceeds 8 nodes regardless of how big the change was.

A retry bug fixed in three lines, explained as a causal chain instead of a paragraph:

<img src="docs/example.svg" alt="Example diagram: Trigger (Retry on 429) leads to Cause (Backoff never reset) leads to Fix (Reset on success), with a one-line caption underneath" width="700">

Triggers automatically once a coding task finishes, or on request: "explain what you just did," "visualize the fix," "show me what changed," or "why does this fix work" for the deep-dive shape.

## visualize-the-plan

The forward-looking sibling. Reads a just-formed plan — typically right before it's presented for approval — and classifies it into one of five shapes: linear steps, a phased plan grouped into milestones, a Now/Planned scope map for plans that are really about which files will change, a decision fork for plans that hinge on a real choice, or a rationale deep dive for when you're asked *why* an approach was chosen. Same 8-node cap, same house style — but nothing in these diagrams is marked "done," since nothing has been built yet: the plan's target gets a dashed accent outline and a small target mark instead of the solid fill and check tick that `visualize-the-fix` reserves for finished work.

A three-step plan, shown before any of it is built:

<img src="docs/example-plan.svg" alt="Example diagram: Step 1 (Add unlock action) leads to Step 2 (Register on route) leads to Goal (Gate re-renders unlocked, not yet built), with a one-line caption underneath" width="700">

Triggers right before presenting a plan for approval (e.g. before exiting plan mode), or on request: "visualize the plan," "diagram this plan," "what's the plan?"

## Install

**As a plugin (recommended):**
```
/plugin marketplace add Chndr-3/visualize-skills
/plugin install visualize-skills
```
Installing the plugin gives you both skills.

**Manually:** drop `skills/visualize-the-fix` and/or `skills/visualize-the-plan` into your Claude Code skills folder (e.g. `~/.claude/skills/`).

Also ships a `.codex-plugin/plugin.json` manifest for installing into [Codex CLI](https://github.com/openai/codex).

## How it renders

Its own house style — no dependency on `diagram-design` or any other skill. Inline SVG + CSS, self-contained HTML, no external assets, no JavaScript:

- **Drafting-table look**: dark mode by default (CSS-only toggle to light, bottom-right), dot-grid background, hairline strokes, monospace subtitles for file:line references. Amber is reserved — in `visualize-the-fix` it only ever marks the node that resolves the diff (solid fill + check tick); in `visualize-the-plan` it only ever marks the node a plan is aiming at (dashed outline + target mark), never something already done.
- **Clickable glossary**: technical terms in a diagram (checked against each skill's own `references/glossary.md`) open a small modal on click, which falls through to a collapsible drawer listing every term used, highlighted with a soft wash animation instead of a flat color block.
- **Text sizing**: box widths and line-wrapping are computed per diagram, not copied from a fixed template — see each skill's `references/renderer.md` for the sizing pass.

Each skill carries its own copy of the house style (`references/style-reference.html` is the canonical working example for that skill's shapes) so either can be installed independently. `skills/visualize-the-fix/references/style-reference.html` and `skills/visualize-the-plan/references/style-reference.html` are the canonical working examples — open either directly in a browser to see the current look. See each skill's `SKILL.md` for its shape-classification table.

## Contributing

See `CONTRIBUTING.md`. Bugs and feature requests: [GitHub issues](https://github.com/Chndr-3/visualize-skills/issues). Security issues: see `SECURITY.md` (please don't file those as public issues).

## License

MIT — see `LICENSE`.
