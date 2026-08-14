# Scope map (Now / Planned)

Two containers side by side — same dashed-container layout as `visualize-the-fix`'s before/after refactor shape, but the meaning is different: **After** hasn't happened yet. Use when the plan's substance is *which files/modules will change*, more than the order you'll touch them in.

Example: "Add per-user rate limiting to the export endpoint"

- Plan touches 3 files, order is incidental (they can go in any sequence).
- Now container: **exportRouter** — no rate check, calls `runExport()` directly (1 child)
- Planned container: **exportRouter** (adds `checkRateLimit(userId)` before `runExport()`) + **rateLimiter.ts** (new — `checkRateLimit`, backed by the existing Redis client) — 2 containers, 1–2 children each
- Prose: "New rate-limit module plus one call site in the export router — will confirm the per-user limit (10/min) before wiring it in."

## Layout rule

- Left container = **Now**, right = **Planned** (labeled kickers, not "before"/"after" — this is a proposal, not a completed change).
- The Now container uses the plain `.dash` chrome. The Planned container adds `stroke="var(--accent-line)"`, same as `visualize-the-fix`'s "after" — but its rows use plain `.mono`/`.mono-out`, never the solid accent-bar row treatment from a *completed* refactor, since nothing here has landed. A new/changed row gets a thin accent-line-colored (not solid accent) left bar instead, to read as "proposed" rather than "done."
- Children are responsibilities/functions being added or touched, not every line — group tightly-coupled additions into one child.
- Draw at most one arrow between containers (labeled "adds" / "extends"), not one per child pair.
- If more than 4 children would be needed per side, collapse the smallest into an "other" child rather than exceeding the 8-node total budget.
