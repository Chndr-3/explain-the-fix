---
name: visualize-the-fix
description: Use when a coding task (bug fix, refactor, new feature) just finished and it's time to explain the change, or when the user asks to "explain," "visualize," or "show" what was just done — produces a small proportional diagram instead of a text summary.
license: MIT
---

# Visualize the Fix

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
4. **Render** — always use the house style in `references/renderer.md`, copying markup/CSS from `references/style-reference.html` (the working example for all shapes, tokens, glossary, and theme toggle). Do not invoke `diagram-design`, even if installed. Node text and the diagram itself scale fluidly with whatever container it renders in (no fixed pixel size, no JS) — see renderer.md's "Sizing for the viewer's actual screen" if you know the render target is unusually narrow or wide.
5. **Link jargon** — not optional. Check every node label and subtitle against `references/glossary.md`. Any stdlib/API name, flag, class, or concept that isn't the user's own vocabulary from the diff (e.g. `TextWrapper`, `stdlib`, `break_on_hyphens`, `idempotent`) gets linked per `## Glossary` in `references/renderer.md` — add a one-line definition to `glossary.md` if the term is missing rather than skipping it. Only variable/function names the user themself just wrote, and generic words like "Fix" or "Trigger", are exempt. If a diagram has zero linkable terms, that's a real outcome worth double-checking, not a default.
6. **Output** — the diagram, AND in the chat reply (not just the card's note) 1–2 sentences: what changed, what's verified. The card's note is the diagram's own caption; the chat reply is separate and still required. No restated written summary beyond that.

## Shape reference

| Shape | Condition | Nodes |
|---|---|---|
| Causal, single system | 1–2 files, one clear cause | 3: Trigger → Cause → Fix |
| Causal, cross-system | 3+ files/services, cause crosses a boundary | 4–6, one per file/service |
| Structural refactor | file count changes (1↔N), class/component boundaries move, behavior unchanged | before/after, 2 containers × 2–4 children |
| New flow/feature | new function/endpoint/route, ordering matters | sequence, 3–6 steps |
| Trivial/config | single-line change, no branching cause | 2: Trigger → Fix |
| Deep dive | user asks *why* the fix works, not just what changed | 3, vertical: Cause → Fix → Why it works |

Full worked examples and node-labeling conventions per shape: `references/shape-causal.md`, `references/shape-refactor.md`, `references/shape-sequence.md`.

## Rules

- One diagram per explanation, unless the task genuinely made two independent changes.
- 8 node hard cap regardless of shape.
- No scrolling/squinting — this is glanceable, not documentation.
- Prose is 1–2 sentences, doesn't restate the diagram.
