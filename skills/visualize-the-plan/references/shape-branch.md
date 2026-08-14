# Decision plan (fork)

One root node, forking into 2 branches, each 1–2 nodes. Use when the plan's substance is a real conditional — a choice you're presenting for the user to pick, or a rollout with a fallback path — not just an ordered list of steps.

Example: "Roll out the new pricing calculator"

- Plan forks on whether the feature flag check is safe to do server-side:
  - Root: **Check flag support**
  - Branch A (chosen, accent): **Server-side flag check available** → gate at the API layer, old calculator stays as dead code behind the flag
  - Branch B (fallback, muted): **Flag only readable client-side** → gate in the UI, API always computes both and picks client-side
- Prose: "Prefers gating server-side if the flag SDK supports it there; falls back to a client-side gate otherwise — will check SDK support before picking a branch."

## Layout rule

- Root node on the left, same `.box` chrome as linear steps.
- Two flow lines fork from the root's right edge to two vertically-stacked branch nodes (upper and lower) — same dot + `.flow` connector and arrowhead marker as the other shapes, just two of them from one origin instead of one straight line.
- If the plan already has a clear preferred path, give that branch's node `.box-goal` styling (dashed accent outline + target mark) and leave the other branch as a plain `.box` — don't force a choice that hasn't actually been made yet; if genuinely undecided, leave both plain.
- Branches don't need to reconverge — most decision plans just pick one path and stop. If they do reconverge (both paths lead to the same next step), draw one more node downstream of both with two incoming flow lines.
- Cap at 6 nodes total (root + up to 2 per branch); if a branch needs more detail than that, it's really two separate linear plans — pick the more likely one and mention the other only in the one-line prose.
