# Structural refactor (before/after)

Two containers side by side, 2–4 children each. Behavior is unchanged — the diagram shows what moved, not what broke/fixed.

Example: "God-object controller split into two components" (big-quantum-mobile)

- Diff: 1 file → 2 files, same behavior, no new tests needed beyond existing coverage passing.
- Before container: **CampaignController** — data fetch, form state, validation, submit (4 children, all in one file)
- After container: **CampaignController** (form state, submit) + **CampaignValidator** (validation, data fetch) — 2 containers, 2 children each
- Prose: "Split the 400-line controller along its two responsibilities; existing test suite passes unchanged."

## Layout rule

- Left container = before, right = after (or top/bottom if diagram-design's layer-stack type reads better for the case).
- Children are responsibilities/methods, not every line moved — group tightly-coupled lines into one child.
- Draw at most one arrow between containers (labeled "split into" / "merged into") — don't wire every child pair.
- If more than 4 children would be needed per side, collapse the smallest ones into an "other" child rather than exceeding the 8-node total budget.
