---
name: visualize-the-problem
description: Use right after diagnosing a bug, error, or incident — before proposing a fix — or when the user asks to "explain," "visualize," or "diagram" a problem, or asks why something is failing. Produces a small proportional diagram of the confirmed root cause instead of a wall of investigation notes.
license: MIT
---

# Visualize the Problem

## Overview

A diagnosis explained as investigation notes is slower to scan than a small diagram of the same shape. This skill picks the smallest diagram shape that fits what you've found wrong and renders it — no essay alongside it. It's the diagnostic sibling of `visualize-the-plan` and `visualize-the-fix`: same house style, but the diagram shows what's confirmed broken right now, not a proposal (`visualize-the-plan`) or something already resolved (`visualize-the-fix`). The natural order across the family is diagnose → propose → resolve: `visualize-the-problem` → `visualize-the-plan` → `visualize-the-fix`.

## When to use

- Right after you've diagnosed a bug, error, or incident — before proposing how to fix it.
- User says "explain/visualize/diagram the problem/bug/issue," or asks "why is this failing?" / "what's going wrong?" for anything beyond a one-line answer.

Skip it for a problem that's really obvious ("a typo in the config key") — answer in prose there, same as the trivial case below. Also skip it if you haven't actually confirmed a cause yet — don't diagram a guess as if it were a diagnosis; investigate further or use the hypothesis-fork shape if there are genuine competing theories still being weighed.

## Workflow

1. **Read the diagnosis** — your own investigation (logs, stack traces, repro steps, the code you traced through), not a diff or a plan. If you haven't pinned down a cause yet, finish investigating first; don't diagram a hunch as if it were confirmed.
2. **Name the scope** — not optional. Identify which feature, screen, or user-facing flow this affects, plus platform if it's not universal ("Try-On carousel → Product detail page, iOS + Android"). A file:line reference says where in the *code*; this says where in the *product*, which is usually what a reader needs first. If there's genuinely no user-facing surface (an internal job, a CI script), say that explicitly instead of skipping this.
3. **Classify the shape** — see `references/classification.md` for the heuristic (whether the cause is confirmed vs. still competing theories, whether reproduction order matters, whether the cause crosses a file/service boundary). Default to the smaller shape when ambiguous.
4. **Build the node list** — max 8 nodes total. If the investigation touched more files/steps than that, group a cluster into one node ("three call sites in the retry module") rather than enumerating every one.
5. **Render** — always use the house style in `references/renderer.md`, copying markup/CSS from `references/style-reference.html` (the working example for all shapes, tokens, glossary, and theme toggle). Do not invoke `diagram-design`, even if installed. Node text and the diagram itself scale fluidly with whatever container it renders in (no fixed pixel size, no JS) — see renderer.md's "Sizing for the viewer's actual screen" if you know the render target is unusually narrow or wide. The scope from step 2 renders as the required `p.scope` line — see renderer.md's "Naming the scope."
6. **Link jargon** — not optional. Check every node label and subtitle against `references/glossary.md`. Any stdlib/API name, flag, class, or concept that isn't the user's own vocabulary gets linked per `## Glossary` in `references/renderer.md` — add a one-line definition to `glossary.md` if the term is missing rather than skipping it. Only variable/function names the user themself already used, and generic words like "Symptom" or "Cause," are exempt. If a diagram has zero linkable terms, that's a real outcome worth double-checking, not a default.
7. **Output** — the diagram, AND in the chat reply (not just the card's note) 1–2 sentences: what's actually broken and how confident you are in the cause. The card's note is the diagram's own caption; the chat reply is separate and still required. No restated written summary beyond that. Then move on to proposing a fix as normal — this skill only changes how the diagnosis is *shown*, it doesn't replace `visualize-the-plan` for the fix itself.

## Shape reference

| Shape | Condition | Nodes |
|---|---|---|
| Symptom chain, single system | Confirmed cause, 1–2 files, one clear chain | 3: Symptom → Contributing factor → Root cause |
| Symptom chain, cross-system | Confirmed cause, 3+ files/services, chain crosses a boundary | 4–6, one per file/service |
| Reproduction sequence | The order of steps needed to trigger the bug matters | sequence, 3–6 steps, ending in the failure |
| Hypothesis fork | Root cause not yet confirmed, real competing theories | fork, 4–6 nodes |
| Mechanism deep dive | User asks *why* the bug happens, not just what's wrong | 3, vertical: Symptom → Root cause → Why it happens |
| Trivial problem | Single obvious cause, no real diagnostic structure | 2: Symptom → Root cause, or skip the diagram entirely |

Full worked examples and node-labeling conventions per shape: `references/shape-symptom-chain.md`, `references/shape-reproduction.md`, `references/shape-hypothesis.md`, `references/shape-mechanism.md`.

## Rules

- One diagram per problem, unless the investigation genuinely found two independent issues.
- 8 node hard cap regardless of shape.
- Never use `visualize-the-fix`'s `.box-done`/check-tick (means "already resolved") or `visualize-the-plan`'s `.box-goal`/target-mark (means "proposed, not built"). A confirmed root cause is real and current but still unresolved — mark it with `.box-problem` (solid accent outline, no fill, warning mark) instead.
- No scrolling/squinting — this is glanceable, not documentation.
- Prose is 1–2 sentences, doesn't restate the diagram.
