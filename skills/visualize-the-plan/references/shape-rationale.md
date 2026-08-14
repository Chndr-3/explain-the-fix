# Rationale (deep dive)

3 nodes, vertical: Goal → Approach → Why this way. Use when the user asks *why* you're planning to do it this way, not just what the steps are — the plan equivalent of `explain-the-fix`'s deep dive.

Example: "Why cache the report at the edge instead of in the app server?"

- Goal: **Cut report load time under 200ms for repeat views**
- Approach: **Cache rendered report at the CDN edge, keyed on filter params**
- Why it works: "App-server caching still pays a network round trip per region; edge caching serves the repeat hit from the same PoP the request already landed on."
- Prose: "Will confirm the filter param set is small enough to keep the cache key space bounded before implementing."

## Layout rule

- Same vertical `.spine` connector as `explain-the-fix`'s deep dive (short dashed line, not an arrow-flow — this tier is a drill-down, not a sequence).
- **Goal** and **Approach** are both plain `.box` chrome — neither is `.box-done`. If the plan is confident enough to mark an end state, use `.box-goal` on **Approach** instead (dashed accent outline + target mark), same convention as the other shapes.
- **Why it works** drops the box fill entirely, same as the original deep dive: `.dash` outline only, accent bar on the left edge, holding one sentence of `.n-prose` — reads as commentary, not a node.
- Use this shape only on explicit request ("why," "why this way," "what's the reasoning") — don't default to it just because a plan has a design decision behind it; that's what the one-line prose caption is for.
