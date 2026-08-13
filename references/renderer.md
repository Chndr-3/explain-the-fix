# Renderer (no diagram-design dependency)

Own house style, inline SVG, no external assets, no `diagram-design` dependency. Always used — never invoke `diagram-design`.

## Style tokens

| Token | Value | Used for |
|---|---|---|
| `--bg` | `#ffffff` | page/canvas background |
| `--box-fill` | `#f8fafc` | node box fill |
| `--box-stroke` | `#1e293b` (slate-800) | node box border |
| `--text` | `#1e293b` (slate-800) | labels, captions |
| `--accent` | `#f59e0b` (amber-500) | arrows, arrowhead, title accent bar, emphasized/changed node |
| `--muted` | `#94a3b8` (slate-400) | container/group borders in before-after layout |
| font | `'Trebuchet MS', 'Century Gothic', 'Futura', sans-serif` | all text — rounded geometric sans, system-available, no font file to embed |

Node corners are rounded (`rx="8"`), not sharp. Arrows are the accent color, not the box stroke color — this is the one deliberate departure from monochrome that makes the direction of flow pop at a glance.

## Layout by shape

- **Causal chain / trivial** (2–6 nodes): boxes left-to-right, one arrow between each consecutive pair, all centered on one horizontal midline. Box width fits its label + 20px padding, height 40px, 20px gaps between boxes. The "Fix" node (last box) gets `fill="var(--accent)"` at 15% opacity and `stroke="var(--accent)"` to mark it as the resolution.
- **Sequence** (3–6 steps): same left-to-right box row — this fallback doesn't draw lifelines/activation bars, just ordered steps with arrows, since the point is glanceability, not full UML fidelity.
- **Before/after refactor**: two side-by-side containers, `stroke="var(--muted)"` `stroke-dasharray="4 3"`, rounded `rx="12"`, with child labels stacked inside as plain text lines (`--box-fill` mini-rects per child), one accent-colored arrow between the containers.

## Minimal box + arrow primitive

```svg
<rect x="X" y="Y" width="W" height="40" rx="8" fill="var(--box-fill)" stroke="var(--box-stroke)"/>
<text x="X+W/2" y="Y+24" font-family="'Trebuchet MS','Century Gothic','Futura',sans-serif" font-size="11" fill="var(--text)" text-anchor="middle">Label</text>
<line x1="X+W" y1="Y+20" x2="NEXT_X" y2="Y+20" stroke="var(--accent)" stroke-width="2" marker-end="url(#arrow)"/>
```

Arrow marker (define once in `<defs>`):

```svg
<marker id="arrow" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto">
  <polygon points="0 0, 8 3, 0 6" fill="var(--accent)"/>
</marker>
```

CSS variables go in a `<style>` block inside the SVG (or inline on each element if the host strips `<style>`):

```svg
<style>
  :root { --bg:#ffffff; --box-fill:#f8fafc; --box-stroke:#1e293b; --text:#1e293b; --accent:#f59e0b; --muted:#94a3b8; }
</style>
```

## Rules

- Consistent house style every time — same tokens, same font stack, same accent. This is the skill's signature look, not a per-diagram choice.
- `viewBox` sized to content, no fixed canvas — `width = sum(box widths) + gaps`, `height = 220` (room for a title above and a one-line caption below).
- Same node/shape budget as the main skill (8 nodes max, one diagram per explanation).
- Output as a single self-contained `.html` file — inline `<svg>`, no external assets, no Google Fonts import (system font stack only).
