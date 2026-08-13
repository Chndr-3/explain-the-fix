---
name: explain-the-fix
description: Use when a coding task (bug fix, refactor, new feature) just finished and it's time to explain the change, or when the user asks to "explain," "visualize," or "show" what was just done — produces a small proportional diagram instead of a text summary.
license: MIT
---

# Explain the Fix

## Overview

Explaining a finished change as prose is slower to read than a small diagram. This skill picks the smallest diagram shape that fits the diff and renders it — no essay alongside it.

## When to use

- Right after finishing a bug fix, refactor, or new feature.
- User says "explain/visualize/show what you did."

Skip it for changes with no diff to point at (pure investigation, answering a question) — just answer in prose there.

## Workflow

1. **Read the diff** — `git diff` against pre-task state (or the files you just touched) — plus your own one-line memory of the task.
2. **Classify the shape** — see `references/classification.md` for the heuristic (file count, whether a cause crosses a boundary, whether file/class count changed, whether it's a brand-new function/route). Default to the smaller shape when ambiguous.
3. **Build the node list** — max 8 nodes total. If the diff is bigger than that, collapse a whole file/module into one node ("3 files in the auth module") rather than enumerating.
4. **Render**:
   - If `diagram-design` is installed, invoke it with the shape + node list already scoped (don't let it re-derive the shape). Cause chain / trivial → `flowchart`, refactor → `layer stack` or `nested` before/after, new flow → `sequence`.
   - Else fall back to a minimal inline SVG: boxes left-to-right (or two side-by-side containers for before/after), one arrow per edge, no styling beyond stroke + text. Skip diagram-design's brand system entirely in the fallback.
5. **Output** one diagram + 1–2 sentences (what changed, what's verified). No restated written summary.

## Shape reference

| Shape | Condition | Nodes |
|---|---|---|
| Causal, single system | 1–2 files, one clear cause | 3: Trigger → Cause → Fix |
| Causal, cross-system | 3+ files/services, cause crosses a boundary | 4–6, one per file/service |
| Structural refactor | file count changes (1↔N), class/component boundaries move, behavior unchanged | before/after, 2 containers × 2–4 children |
| New flow/feature | new function/endpoint/route, ordering matters | sequence, 3–6 steps |
| Trivial/config | single-line change, no branching cause | 2: Trigger → Fix |

Full worked examples and node-labeling conventions per shape: `references/shape-causal.md`, `references/shape-refactor.md`, `references/shape-sequence.md`.

## Rules

- One diagram per explanation, unless the task genuinely made two independent changes.
- 8 node hard cap regardless of shape.
- No scrolling/squinting — this is glanceable, not documentation.
- Prose is 1–2 sentences, doesn't restate the diagram.
