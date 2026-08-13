# Renderer (no diagram-design dependency)

Own house style, inline SVG + CSS, no external assets, no `diagram-design` dependency. Always used — never invoke `diagram-design`.

**Source of truth: `references/style-reference.html`.** It's a complete, working page with all four shapes, the glossary system, and the theme toggle already wired up correctly — copy markup/CSS from it rather than retyping from this doc's prose. This file explains *why* each piece looks the way it does and *when* to use each shape; the reference file is what you actually lift code from.

## Style tokens

Dark mode is the default (`:root`). Light mode is a full override under `body:has(#theme-toggle:checked)` — see `style-reference.html` lines 7–38 for the exact block, copy it verbatim.

| Token | Dark (default) | Light (toggled) | Used for |
|---|---|---|---|
| `--bg` | `#0e1116` | `#f7f6f3` | page background (behind a subtle dot grid, see below) |
| `--panel` | `#141922` | `#ffffff` | card / glossary / modal backgrounds |
| `--box-fill` | `#1a2029` | `#ffffff` | node box fill |
| `--box-stroke` | `#333d4b` | `#cfcabf` | node box border, connector lines |
| `--rule` | `#242c37` | `#e5e1d8` | hairline dividers, card borders |
| `--text` | `#e4e7ec` | `#171a1f` | primary text |
| `--muted` | `#8b95a4` | `#6f7784` | secondary text, kicker labels, dividers |
| `--accent` / `--accent-ink` | `#f5a524` | `#f59e0b` / `#9a5b06` | resolved node, changed row, glossary highlight — reserved, only ever marks the "this is what changed / this is the answer" element |
| `--accent-soft` | `rgba(245,165,36,.13)` | `rgba(245,158,11,.14)` | resolved-node fill, glossary wash |
| `--accent-line` | `rgba(245,165,36,.45)` | `rgba(154,91,6,.5)` | accent-colored dashed borders |
| `--sans` | `'Trebuchet MS','Century Gothic',Futura,Avenir,'Segoe UI',system-ui,sans-serif` | same | all prose/titles |
| `--mono` | `ui-monospace,SFMono-Regular,'SF Mono',Menlo,Consolas,monospace` | same | anything that appears in the codebase (file paths, function calls) |

Note `--accent-ink` differs from `--accent` in light mode (`#9a5b06` vs `#f59e0b`) — pure amber text fails contrast on a light background, so ink text uses a darkened shade while fills/strokes keep the brighter accent. Don't collapse these into one variable.

Body background is a faint 28px dot grid (`background-image: linear-gradient(var(--grid) 1px,transparent 1px), linear-gradient(90deg,var(--grid) 1px,transparent 1px); background-size:28px 28px;`) — a drafting-table cue, not decoration to skip.

## Page structure

Wrap the whole output in `.wrap` (max-width 820px, centered), then one `.card` per diagram (`background:var(--panel); border:1px solid var(--rule); border-radius:14px; box-shadow:var(--shadow); padding:18px 20px 20px`). Inside the card: `<header>` with `<h2>` (shape name) + `.tag` (the node-flow summary, e.g. "Trigger → Cause → Fix"), then `p.note` (one sentence on what this specific diagram shows), then the `<svg>`.

Skip the page-level `.eyebrow`/`h1`/`.dek` — that's this reference file's own title block, not part of a single explanation's output.

## Layout by shape

All four are in `style-reference.html` as complete, copyable `<svg>` blocks (search for the `<!-- N. SHAPE -->` comments). Node text always splits into a kicker (`n-kick`, uppercase, letterspaced, sequence/role label like "01 · TRIGGER"), a title (`n-title`), and where relevant a monospace subtitle (`n-sub`, e.g. `fetchQueue.ts:114`) — this file→line reference is what makes the diagram feel grounded in the actual diff, include it whenever you know the location.

- **Causal chain** (`section 1`, viewBox `0 0 692 132`): 3–5 boxes left-to-right. Each node is `.box` (rx 9) with kicker + title + mono subtitle. Nodes connect via a small dot (`<circle r="2">`) then a thin `.flow` line with the shared arrow `<marker>` (a hollow chevron path, not a filled triangle). The terminal node only gets `.box-done` (accent-soft fill, accent stroke) plus a small accent tick-mark (`.tick`, a checkmark path) in the top-right corner — that tick is the "resolved" signal, not just the color change.
- **Before/after refactor** (`section 2`, viewBox `0 0 692 214`): two `.dash` containers (dashed stroke, rx 11) side by side, connected by the same dot+flow-line connector. Each container holds a monospace call chain (`.mono` for the entry point, `.mono-out` + tree characters `├`/`└` for the calls under it), rows separated by `.row-rule` hairlines. The "after" container's border switches to `--accent-line`; the one row that actually changed gets a 2.5px accent bar on its left edge (`<rect width="2.5" fill="var(--accent)">`) plus `.mono` in accent-ink instead of `.mono-out` — only that row gets weight, everything else stays muted.
- **Deep dive** (`section 3`, viewBox `0 0 692 288`): full-width boxes stacked vertically (cause → fix → why), connected by a `.spine` (short dashed vertical line, not an arrow — this tier isn't a flow, it's a drill-down). Fix tier is `.box-done` + tick, same as the causal chain's terminal node. The "why it works" tier drops the box fill entirely: `.dash` outline only, plus a small accent bar on the left edge, holding one sentence of `.n-prose` (muted, smaller, reads as commentary not a node).
- **Sequence** (not in the reference file — extrapolate from causal chain): same left-to-right box row with kicker/title/sub, dot+flow connectors; skip the terminal `.box-done` tick since a sequence doesn't resolve to one answer the way a causal chain does. 3–6 steps.

Node/shape budget unchanged: 8 nodes max, one diagram per explanation.

## Glossary (click to see meaning)

Two tiers, both CSS-only (`:target`), no JavaScript — see `references/glossary.md` for the term dictionary, and `style-reference.html` section 4 + the drawer + the `.modal` divs at the bottom for the full working markup.

1. **Inline term** → `<a class="term" href="#m-<slug>">term</a>` in prose, or `<a href="#m-<slug>"><text class="n-title svg-term">term</text></a>` inside an SVG label. Dotted underline, `cursor:help`.
2. **Click opens a centered modal** — `<div class="modal" id="m-<slug>">` containing a `.scrim` (click to dismiss, `href="#!"`) and a `.card-m` with the term, one-line definition, and an "All terms →" link to `#g-<slug>` in the drawer. This keeps the reader's scroll position instead of jumping the page.
3. **"All terms" falls through to the drawer** — a collapsible `.glossary` section (checkbox-hack `#gl-open`, chevron rotates via `body:has(#gl-open:checked)`) holding a `<dl>` of every term used in this diagram, each entry `id="g-<slug>"`. Landing on one via `:target` gives it a left accent border plus a `wash` keyframe animation (soft gradient fades in then out over 2.4s) instead of a flat background fill — that's the fix for the "ugly highlight" from the earlier version.

One drawer per output page (not per diagram), listing only the terms actually used. Only link terms a non-expert reader might not know — plain box labels never get the treatment.

## Theme toggle

Pill switch, fixed bottom-right, no top-of-page label. Checkbox hack (`#theme-toggle`), unchecked = dark (default). `.theme` label wraps a `.knob` (sliding dot) and a text span that swaps between "Dark"/"Light" via `.lbl-d`/`.lbl-l` display toggling — copy `style-reference.html` lines 130–147 and the closing `<input>`/`<label>` pair at the bottom of the body verbatim.

## Rules

- Consistent house style every time — same tokens, same font stack, same accent, same card/glossary/toggle chrome. This is the skill's signature look, not a per-diagram choice.
- `viewBox` sized to content, matching the reference file's proportions (e.g. `692×132` for a 3-node causal chain) rather than inventing new aspect ratios per diagram.
- Same node/shape budget as the main skill (8 nodes max, one diagram per explanation).
- Output as a single self-contained `.html` file — inline `<svg>` + `<style>`, no external assets, no Google Fonts import (system font stack only).
