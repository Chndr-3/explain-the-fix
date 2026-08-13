# Fallback renderer (no diagram-design dependency)

Plain, unstyled inline SVG. System font, black stroke on white, no design tokens, no Google Fonts import. Use whenever `diagram-design` isn't installed, or when the user explicitly wants a diagram that doesn't route through it.

## Layout by shape

- **Causal chain / trivial** (2–6 nodes): boxes left-to-right, one arrow between each consecutive pair, all centered on one horizontal midline. Box width fits its label + 20px padding, height 40px, 20px gaps between boxes.
- **Sequence** (3–6 steps): same left-to-right box row — this fallback doesn't draw lifelines/activation bars, just ordered steps with arrows, since the point is glanceability, not full UML fidelity.
- **Before/after refactor**: two side-by-side rects (containers) with their child labels stacked inside as plain text lines, one arrow between the containers.

## Minimal box + arrow primitive

```svg
<rect x="X" y="Y" width="W" height="40" fill="none" stroke="#333"/>
<text x="X+W/2" y="Y+24" font-size="11" text-anchor="middle">Label</text>
<line x1="X+W" y1="Y+20" x2="NEXT_X" y2="Y+20" stroke="#333" marker-end="url(#arrow)"/>
```

Arrow marker (define once in `<defs>`):

```svg
<marker id="arrow" markerWidth="8" markerHeight="6" refX="7" refY="3" orient="auto">
  <polygon points="0 0, 8 3, 0 6" fill="#333"/>
</marker>
```

## Rules

- No color beyond black/white — this fallback is intentionally undecorated so it never fights a host page's theme.
- `viewBox` sized to content, no fixed canvas — `width = sum(box widths) + gaps`, `height = 220` (room for a title above and a one-line caption below).
- Same node/shape budget as the main skill (8 nodes max, one diagram per explanation).
- Output as a single self-contained `.html` file — inline `<svg>`, no external assets.
