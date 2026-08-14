# Renderer (no diagram-design dependency)

Own house style, inline SVG + CSS, no external assets, no `diagram-design` dependency. Always used — never invoke `diagram-design`. This is the same visual system as the `explain-the-fix` skill (same tokens, same card/glossary/toggle chrome) so the two skills read as one family, but the node semantics differ: everything here is *proposed*, not *done*.

**Source of truth: `references/style-reference.html`.** It's a complete, working page with all five shapes, the glossary system, and the theme toggle already wired up correctly — copy markup/CSS from it rather than retyping from this doc's prose. This file explains *why* each piece looks the way it does and *when* to use each shape; the reference file is what you actually lift code from.

## Style tokens

Identical token set to `explain-the-fix` — if you have that skill installed too, these are the same CSS variables. Dark mode is the default (`:root`). Light mode is a full override under `body:has(#theme-toggle:checked)` — see `style-reference.html` lines 7–38 for the exact block, copy it verbatim.

| Token | Dark (default) | Light (toggled) | Used for |
|---|---|---|---|
| `--bg` | `#0e1116` | `#f7f6f3` | page background (behind a subtle dot grid, see below) |
| `--panel` | `#141922` | `#ffffff` | card / glossary / modal backgrounds |
| `--box-fill` | `#1a2029` | `#ffffff` | node box fill |
| `--box-stroke` | `#333d4b` | `#cfcabf` | node box border, connector lines |
| `--rule` | `#242c37` | `#e5e1d8` | hairline dividers, card borders |
| `--text` | `#e4e7ec` | `#171a1f` | primary text |
| `--muted` | `#8b95a4` | `#6f7784` | secondary text, kicker labels, dividers |
| `--accent` / `--accent-ink` | `#f5a524` | `#f59e0b` / `#9a5b06` | the node/branch a plan is aiming at, glossary highlight — reserved, only ever marks "this is the plan's target / chosen path," never "this already happened" |
| `--accent-soft` | `rgba(245,165,36,.13)` | `rgba(245,158,11,.14)` | glossary wash only — plan diagrams don't use a solid accent fill on any node (see Goal marker below) |
| `--accent-line` | `rgba(245,165,36,.45)` | `rgba(154,91,6,.5)` | accent-colored dashed borders, "proposed" row bars |
| `--sans` | `'Trebuchet MS','Century Gothic',Futura,Avenir,'Segoe UI',system-ui,sans-serif` | same | all prose/titles |
| `--mono` | `ui-monospace,SFMono-Regular,'SF Mono',Menlo,Consolas,monospace` | same | anything that appears in the codebase (file paths, function calls) |

Note `--accent-ink` differs from `--accent` in light mode (`#9a5b06` vs `#f59e0b`) — pure amber text fails contrast on a light background, so ink text uses a darkened shade while fills/strokes keep the brighter accent. Don't collapse these into one variable.

Body background is a faint 28px dot grid (`background-image: linear-gradient(var(--grid) 1px,transparent 1px), linear-gradient(90deg,var(--grid) 1px,transparent 1px); background-size:28px 28px;`) — a drafting-table cue, not decoration to skip.

## The "not done yet" rule

`explain-the-fix` uses a solid accent fill (`.box-done`) plus a check-mark tick to mark the node that resolves a diff. **Never use that pair here.** A plan hasn't executed — marking a node as "done" before it's built is actively misleading. Instead:

- **`.box-goal`** — dashed accent outline, no solid fill (`fill:none; stroke:var(--accent); stroke-dasharray:4 4`), for the node the plan is building toward. Title text uses `.goal-title` (`fill:var(--accent-ink)`), same as `explain-the-fix`'s `.done-title`.
- **`.goal-mark`** — a small target mark (two concentric circles + center dot, accent-colored, stroke-only) in the same top-right corner position `explain-the-fix` uses for its check tick. Reads as "aim," not "complete."
- Only apply `.box-goal`/`.goal-mark` when the plan is actually confident about its end state. If a shape has no clear single end state (e.g. a decision plan where neither branch is chosen yet), leave every node as a plain `.box` — don't force a goal marker onto something undecided.

## Page structure

Wrap the whole output in `.wrap` (max-width 820px, centered), then one `.card` per diagram (`background:var(--panel); border:1px solid var(--rule); border-radius:14px; box-shadow:var(--shadow); padding:18px 20px 20px`). Inside the card: `<header>` with `<h2>` (shape name) + `.tag` (the node-flow summary, e.g. "Step → Step → Goal"), then `p.note` (one sentence on what this specific diagram shows), then the `<svg>`.

Skip the page-level `.eyebrow`/`h1`/`.dek` — that's this reference file's own title block, not part of a single plan's output.

## Layout by shape

All five are in `style-reference.html` as complete, copyable `<svg>` blocks (search for the `<!-- N. SHAPE -->` comments). Node text always splits into a kicker (`n-kick`, uppercase, letterspaced, sequence/role label like "01 · STEP" or "A · SERVER-SIDE"), a title (`n-title`), and where relevant a monospace subtitle (`n-sub`, e.g. `campaign/actions.ts`) — this file/module reference is what makes the diagram feel grounded in the real codebase, include it whenever you already know where the work will land.

- **Linear steps** (`section 1`, viewBox `0 0 692 132`): 3–6 boxes left-to-right, identical geometry to `explain-the-fix`'s causal chain. Nodes connect via a small dot (`<circle r="2">`) then a thin `.flow` line with the shared arrow `<marker>` (a hollow chevron path, not a filled triangle). The terminal node gets `.box-goal` + `.goal-mark` instead of `.box-done` + tick.
- **Phased plan** (`section 2`, viewBox `0 0 692 214`): 2–4 `.dash` containers (dashed stroke, rx 11) left to right, each holding a short list of children (`.mono`/`.mono-out` rows, `.row-rule` hairlines between them) — the same container chrome as `explain-the-fix`'s before/after, generalized past two. Containers connect with the same dot+flow-line connector. The final phase's container border switches to `--accent-line` to mark it as the plan's end state, mirroring `.box-goal` at the container level — no per-row accent bar unless one specific child is the phase's headline item.
- **Scope map** (`section 3`, viewBox `0 0 692 214`): two `.dash` containers side by side, labeled **Now** / **Planned** (never "before"/"after" — nothing has changed yet). Same call-chain layout as `explain-the-fix`'s refactor shape, but the Planned container's new/changed rows get a left-edge bar filled with `var(--accent-line)` (semi-transparent) instead of solid `var(--accent)`, and the row's mono text stays regular weight in `--accent-ink` — a proposed row reads as *lighter* than a done one, on purpose.
- **Decision plan** (`section 4`, viewBox `0 0 692 236`, not present in `explain-the-fix`): one root `.box` on the left, forking via two cubic-bezier `.flow` paths (not straight lines — `M{rootRightX} {rootCenterY} C {midX} {rootCenterY} {midX} {branchCenterY} {branchLeftX} {branchCenterY}`, one curve per branch) to two boxes stacked on the right, each with its own arrowhead marker instance. The preferred branch (if any) gets `.box-goal` + accent-ink kicker; the other stays a plain `.box` with a muted kicker noting it's the fallback. If genuinely undecided, both branches stay plain.
- **Rationale** (`section 5`, viewBox `0 0 692 288`): full-width boxes stacked vertically (goal → approach → why), connected by a `.spine` (short dashed vertical line, not an arrow — this tier isn't a flow, it's a drill-down). Goal is a plain `.box`; Approach gets `.box-goal` + `.goal-mark` (not `.box-done`/tick) if the plan is confident in that approach. The "why it works" tier drops the box fill entirely: `.dash` outline only, plus a small accent bar on the left edge, holding one sentence of `.n-prose` (muted, smaller, reads as commentary not a node).

Node/shape budget unchanged: 8 nodes max, one diagram per plan.

## Sizing and text wrapping (don't copy coordinates verbatim)

The pixel positions in `style-reference.html` fit *its own* example text ("Add unlock action," "Register on route"). Real labels are often longer — copying its `x`/`width` values as-is causes text to overflow the box. Every render needs its own sizing pass:

- **Title (`.n-title`, 13.5px sans)**: roughly 7px/character. If a title's estimated width exceeds the box's inner width (box width minus ~36px padding), split it across two `<tspan>` lines instead of widening the box past ~220px:
  ```svg
  <text class="n-title" x="X" y="Y">
    <tspan x="X" dy="0">First line of title</tspan>
    <tspan x="X" dy="16">continues here</tspan>
  </text>
  ```
  A two-line title needs a taller box (+16–18px) and pushes the subtitle down to match — recompute `n-sub`'s `y` and, for linear-steps/rationale, the connector/goal-mark coordinates that follow it. Cap titles at 2 lines; if it still doesn't fit, shorten the label instead of adding a third line.
- **Subtitle (`.n-sub`/`.mono`, 11–11.5px mono)**: roughly 6.5px/character, single line — mono file/module refs are usually short, but if one runs long (a deep namespace path), truncate with `…` rather than wrapping a second line.
- **Box width**: size to the longest line it contains (title or subtitle) + 36px padding, not a fixed 180px. Recenter connectors (`circle cx`, `path d` for `.flow`/`.spine`) and downstream node `x` positions to match — the whole row's total width and the `viewBox` change together when one box grows.
- When in doubt, measure a plain-text draft of every label first, size boxes to the longest one in the row (so nodes stay visually even), *then* place coordinates — don't place coordinates first and hope text fits.

## Glossary (click to see meaning)

Two tiers, both CSS-only (`:target`), no JavaScript — see `references/glossary.md` for the term dictionary, and `style-reference.html` section 6 + the drawer + the `.modal` divs at the bottom for the full working markup.

1. **Inline term** → `<a class="term" href="#m-<slug>">term</a>` in prose, or `<a href="#m-<slug>"><text class="n-title svg-term">term</text></a>` inside an SVG label. Dotted underline, `cursor:help`.
2. **Click opens a centered modal** — `<div class="modal" id="m-<slug>">` containing a `.scrim` (click to dismiss, `href="#!"`) and a `.card-m` with the term, one-line definition, and an "All terms →" link to `#g-<slug>` in the drawer. This keeps the reader's scroll position instead of jumping the page.
3. **"All terms" falls through to the drawer** — a collapsible `.glossary` section (checkbox-hack `#gl-open`, chevron rotates via `body:has(#gl-open:checked)`) holding a `<dl>` of every term used in this diagram, each entry `id="g-<slug>"`. Landing on one via `:target` gives it a left accent border plus a `wash` keyframe animation (soft gradient fades in then out over 2.4s) instead of a flat background fill.

One drawer per output page (not per diagram), listing only the terms actually used. Only link terms a non-expert reader might not know — plain box labels never get the treatment.

## Theme toggle

Pill switch, fixed bottom-right, no top-of-page label. Checkbox hack (`#theme-toggle`), unchecked = dark (default). `.theme` label wraps a `.knob` (sliding dot) and a text span that swaps between "Dark"/"Light" via `.lbl-d`/`.lbl-l` display toggling — copy `style-reference.html`'s theme-toggle block verbatim.

## Rules

- Consistent house style every time — same tokens, same font stack, same accent, same card/glossary/toggle chrome. This is the skill's signature look, not a per-diagram choice.
- `viewBox` sized to content, matching the reference file's proportions (e.g. `692×132` for a 3-node linear plan) rather than inventing new aspect ratios per diagram.
- Same node/shape budget as the main skill (8 nodes max, one diagram per plan).
- Output as a single self-contained `.html` file — inline `<svg>` + `<style>`, no external assets, no Google Fonts import (system font stack only).
- Never use `.box-done` or the check-mark tick — see "The 'not done yet' rule" above.
