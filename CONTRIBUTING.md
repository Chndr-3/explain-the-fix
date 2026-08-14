# Contributing

Thanks for considering a contribution. This repo hosts two small skills that share one house style — most changes are edits to markdown or a reference HTML file, no build step involved.

## Project layout

```
skills/
  visualize-the-fix/       — explains a just-finished coding task
    SKILL.md
    references/
  visualize-the-plan/    — visualizes a proposed plan before it's built
    SKILL.md
    references/
docs/                    — example SVGs used in README.md
```

Each skill is self-contained: its own `SKILL.md`, its own `references/renderer.md` (rendering spec), `references/style-reference.html` (canonical working example), `references/glossary.md` (jargon dictionary), and its own shape-classification/shape-detail docs. Neither skill depends on the other at runtime — the shared house style is duplicated on purpose so either can be installed alone.

- `SKILL.md` — the skill definition Claude Code reads: when it triggers, the workflow steps, the shape-classification table.
- `references/renderer.md` — the rendering spec: style tokens, layout rules per shape, sizing/wrapping guidance.
- `references/style-reference.html` — the canonical *working* example of every shape, the glossary system, and the theme toggle. If you change the style, this file is the source of truth to update first — `renderer.md` describes it, it doesn't duplicate it.
- `references/glossary.md` — the starter term dictionary used to link jargon in diagrams.
- `references/classification.md` — the shape-classification heuristic.
- `references/shape-*.md` — worked examples and node-labeling conventions per shape.

## Making a change

1. **Style/layout changes** — edit the skill's `references/style-reference.html` directly, open it in a browser to confirm it still looks and behaves correctly (theme toggle, glossary modal + drawer, all shapes), then update `renderer.md`'s prose to match what you changed. If the change should apply to both skills (e.g. a token color fix), make the same edit in both `skills/visualize-the-fix/references/style-reference.html` and `skills/visualize-the-plan/references/style-reference.html` — they're intentionally independent copies, not a shared include.
2. **New shape or workflow change** — edit that skill's `SKILL.md` first (the workflow/shape table), then add the corresponding section to `renderer.md` and a `shape-*.md` file.
3. **New glossary terms** — add to the skill's `references/glossary.md` rather than only defining a term inline in one diagram; that way it's reused next time. `visualize-the-fix` and `visualize-the-plan` keep separate glossaries since their jargon skews differently (implementation detail vs. planning/rollout vocabulary) — add a term to both only if it's genuinely common to both.
4. **A wholly new skill** — give it its own `skills/<name>/` directory following the same internal layout, and add it to `.claude-plugin/plugin.json`'s description, `.codex-plugin/plugin.json`'s `defaultPrompt`/description, and this file's project layout section.

## Before opening a PR

- If you touched `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`, `.codex-plugin/plugin.json`, or `.agents/plugins/marketplace.json`, make sure they're still valid JSON and that `name`/`version` stay in sync across the manifests.
- If you touched a `references/style-reference.html`, open it in a browser and check both themes (the toggle is bottom-right) and at least one glossary term click-through.
- Keep diagrams to the existing budget: 8 nodes max, one diagram per explanation/plan. Don't add a new shape without updating both that skill's `SKILL.md` table and its `renderer.md`.
- `visualize-the-plan` diagrams must never use `visualize-the-fix`'s "done" treatment (`.box-done` / check tick) — a plan hasn't executed. Use `.box-goal` / the target mark instead.

## Reporting bugs / requesting shapes

Open a [GitHub issue](https://github.com/Chndr-3/visualize-the-fix/issues). For a bug, include the diff/plan and which skill produced the bad diagram if you can — most rendering bugs are about a specific label/shape combination, not the renderer in general.
