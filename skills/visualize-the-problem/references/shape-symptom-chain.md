# Symptom chain

Symptom → Contributing factor → Root cause. Nodes are observations/states, not files (single-system) or one node per file/service (cross-system). Same geometry as `visualize-the-fix`'s causal chain, but the terminal node is marked as a confirmed-and-unresolved cause, not a fix.

## Single system (3 nodes)

Example: "Report totals drift after the third refresh"

- Investigation: 1 file (`ReportTotals.tsx`), a memoized selector keyed on the wrong prop.
- Nodes:
  1. **Symptom** — Totals change slightly on refresh with no data change
  2. **Contributing factor** — Selector memoizes on `filters`, not `filters + page`
  3. **Root cause** — Stale page's cached total gets reused after paging
- Prose: "Confirmed: the memo key is missing `page`, so paging back reuses a stale cached total. Haven't touched the fix yet."

## Cross-system (4–6 nodes, one per file/service)

Example: "Webhook deliveries silently drop under load"

- Investigation: 2 services (`webhook-dispatcher`, `event-queue`), cause crosses the boundary.
- Nodes:
  1. **Symptom** — Some webhook deliveries never arrive, no error logged
  2. **event-queue** — enqueues at full rate, no backpressure signal
  3. **webhook-dispatcher** — drops enqueue failures silently on a full local buffer
  4. **Root cause** — Dispatcher's buffer overflows silently under burst load, queue has no way to know
- Prose: "Confirmed: the dispatcher swallows a buffer-full error instead of surfacing it. Queue side is healthy."

Keep the chain linear — no branching diamonds. If two contributing factors are both real, pick the dominant one and mention the other in the one-line prose instead of adding a node. If the cause genuinely isn't confirmed yet, this is the wrong shape — use the hypothesis fork instead.
