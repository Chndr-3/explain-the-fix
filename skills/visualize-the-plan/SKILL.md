---
name: visualize-the-plan
description: Use right before presenting an implementation plan for approval (e.g. before exiting plan mode), or when the user asks to "visualize," "diagram," or "show" a plan — produces a small proportional diagram of the proposed work instead of a wall of prose.
license: MIT
---

# Visualize the Plan

## Overview

A plan read as a bulleted list is slower to scan than a small diagram of the same shape. This skill picks the smallest diagram shape that fits a *proposed* plan and renders it — no essay alongside it. It's the forward-looking sibling of `visualize-the-fix`: same house style, but the diagram shows work that's about to happen, not work that already did.

## When to use

- Right before presenting an implementation plan for approval — typically just before exiting plan mode.
- User says "visualize/diagram/show the plan," or asks "what's the plan?" for anything beyond a one-line answer.

Skip it for a plan that's really one step ("just rename this variable") — answer in prose there, same as the trivial case below.

## Workflow

1. **Read the plan** — your own just-formed plan (the steps you're about to propose), not a diff. If steps aren't finalized yet, finalize them first; don't diagram a plan you're still revising.
2. **Name the scope** — not optional. Identify which feature, screen, or user-facing flow this plan touches, plus platform if it's not universal ("Try-On carousel → Product detail page, iOS + Android"). A file path says where in the *code*; this says where in the *product*, which is usually what a reader needs first to know if the plan is even touching the right thing. If there's genuinely no user-facing surface (an internal job, a build script), say that explicitly instead of skipping this.
3. **Classify the shape** — see `references/classification.md` for the heuristic (step count, whether steps branch on a decision, whether the plan is really about which files/modules will change, whether phases group naturally). Default to the smaller shape when ambiguous.
4. **Build the node list** — max 8 nodes total. If the plan has more steps than that, group a cluster of related steps into one node ("wire up the three API routes") rather than enumerating every one.
5. **Render** — always use the house style in `references/renderer.md`, copying markup/CSS from `references/style-reference.html` (the working example for all shapes, tokens, glossary, and theme toggle). Do not invoke `diagram-design`, even if installed. Node text and the diagram itself scale fluidly with whatever container it renders in (no fixed pixel size, no JS) — see renderer.md's "Sizing for the viewer's actual screen" if you know the render target is unusually narrow or wide. The scope from step 2 renders as the required `p.scope` line — see renderer.md's "Naming the scope."
6. **Link jargon** — not optional. Check every node label and subtitle against `references/glossary.md`. Any stdlib/API name, flag, class, or concept that isn't the user's own vocabulary gets linked per `## Glossary` in `references/renderer.md` — add a one-line definition to `glossary.md` if the term is missing rather than skipping it. Only variable/function names the user themself already used, and generic words like "Plan" or "Step," are exempt. If a diagram has zero linkable terms, that's a real outcome worth double-checking, not a default.
7. **Output** — the diagram, AND in the chat reply (not just the card's note) 1–2 sentences: what the plan does and what you need confirmed before starting. The card's note is the diagram's own caption; the chat reply is separate and still required. No restated written summary beyond that. Then proceed with plan approval as normal (e.g. exiting plan mode) — this skill only changes how the plan is *shown*, not the approval flow.

## Shape reference

| Shape | Condition | Nodes |
|---|---|---|
| Linear steps | Ordered plan, no branching, ≤6 steps | 3–6, sequence in order |
| Phased plan | Steps group into 2–4 natural phases/milestones | 2–4 phase containers × 2–3 children |
| Scope map | The plan is really "which files/modules get touched," order is secondary | before/after, Now vs. Planned, 2 containers × 2–4 children |
| Decision plan | Plan forks on a real choice ("if X, do A; else B") | fork, 4–6 nodes |
| Rationale | User asks *why* this approach, not just what the steps are | 3, vertical: Goal → Approach → Why this way |
| Trivial plan | Single step or a plan too small to have real structure | 2: Goal → Step, or skip the diagram entirely |

Full worked examples and node-labeling conventions per shape: `references/shape-linear.md`, `references/shape-phased.md`, `references/shape-scope.md`, `references/shape-branch.md`, `references/shape-rationale.md`.

## Rules

- One diagram per plan, unless the plan genuinely has two independent tracks of work.
- 8 node hard cap regardless of shape.
- Never use the `.box-done` fill or the check-mark tick from `visualize-the-fix`'s house style — those mean "already executed." Nothing in a plan diagram has happened yet; use `.box-goal` (dashed accent outline + target mark) for the node the plan is building toward instead.
- No scrolling/squinting — this is glanceable, not documentation.
- Prose is 1–2 sentences, doesn't restate the diagram.
