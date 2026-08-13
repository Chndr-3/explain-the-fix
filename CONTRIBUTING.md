# Contributing

Thanks for considering a contribution. This is a small, single-skill repo — most changes are edits to markdown or the reference HTML, no build step involved.

## Project layout

- `SKILL.md` — the skill definition Claude Code reads: when it triggers, the workflow steps, the shape-classification table.
- `references/renderer.md` — the rendering spec: style tokens, layout rules per shape, sizing/wrapping guidance.
- `references/style-reference.html` — the canonical *working* example of every shape, the glossary system, and the theme toggle. If you change the style, this file is the source of truth to update first — `renderer.md` describes it, it doesn't duplicate it.
- `references/glossary.md` — the starter term dictionary used to link jargon in diagrams.
- `references/classification.md` — the shape-classification heuristic.

## Making a change

1. **Style/layout changes** — edit `references/style-reference.html` directly, open it in a browser to confirm it still looks and behaves correctly (theme toggle, glossary modal + drawer, all four shapes), then update `renderer.md`'s prose to match what you changed.
2. **New shape or workflow change** — edit `SKILL.md` first (the workflow/shape table), then add the corresponding section to `renderer.md`.
3. **New glossary terms** — add to `references/glossary.md` rather than only defining a term inline in one diagram; that way it's reused next time.

## Before opening a PR

- If you touched `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`, `.codex-plugin/plugin.json`, or `.agents/plugins/marketplace.json`, make sure they're still valid JSON and that `name`/`version` stay in sync across the three plugin manifests.
- If you touched `references/style-reference.html`, open it in a browser and check both themes (the toggle is bottom-right) and at least one glossary term click-through.
- Keep diagrams to the existing budget: 8 nodes max, one diagram per explanation. Don't add a fifth shape without updating both `SKILL.md`'s table and `renderer.md`.

## Reporting bugs / requesting shapes

Open a [GitHub issue](https://github.com/Chndr-3/explain-the-fix/issues). For a bug, include the diff or task that produced the bad diagram if you can — most rendering bugs are about a specific label/shape combination, not the renderer in general.
