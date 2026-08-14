# Hypothesis fork

One root node, forking into 2 branches, each 1–2 nodes. Use when the root cause isn't confirmed yet and there are genuinely two competing explanations worth showing — not for a cause you've already confirmed (use the symptom chain for that).

Example: "Intermittent 500s on the checkout endpoint"

- Investigation forks on two live theories:
  - Root: **Intermittent 500s, ~1 in 200 requests**
  - Branch A (confirmed, accent): **Connection pool exhaustion** → pool size (10) matches the failure rate under peak concurrency; confirmed by pool-wait metrics spiking at the same timestamps
  - Branch B (ruled out, muted): **Downstream payment API timeout** → checked, payment API's own latency graph shows no correlated spikes
- Prose: "Confirmed via pool-wait metrics: connection pool exhaustion, not the payment API. Ruled the latter out from its own latency graph."

## Layout rule

- Root node on the left, same `.box` chrome as the other shapes.
- Two flow lines fork from the root's right edge to two vertically-stacked branch nodes (upper and lower) — same dot + `.flow` connector and arrowhead marker as the other shapes, just two of them from one origin instead of one straight line.
- The confirmed branch (if you've actually confirmed one) gets `.box-problem` styling (solid accent outline + warning mark) and an accent-ink kicker; the ruled-out branch stays a plain `.box` with a muted kicker noting it was ruled out (not "fallback" — that's `visualize-the-plan`'s vocabulary, this is "eliminated"). If genuinely still undetermined between the two, leave both plain and say so in the prose — don't force a confirmation that hasn't happened.
- Branches don't reconverge — a hypothesis fork's whole point is that exactly one theory survives investigation.
- Cap at 6 nodes total (root + up to 2 per branch); if a branch needs more explanation than that, it's really a symptom chain of its own — confirm it first, then switch shapes.
