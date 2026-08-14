# Classification heuristic

Cheap, not perfect. Run in this order, take the first match:

1. **One obvious cause, nothing to diagnose** (a typo, an unset env var, a config value clearly wrong on inspection) → trivial, 2 nodes or skip the diagram.
2. **User explicitly asks *why* the bug happens** at a mechanism level, not just what's wrong ("why does this race condition happen," "why does this only fail on retry") → mechanism deep dive, regardless of how the cause was found.
3. **The root cause isn't confirmed yet** — you have real competing theories and haven't ruled one out → hypothesis fork. Don't force this shape once you've actually confirmed a cause; move to a chain shape instead.
4. **The order of steps needed to trigger the bug matters** and isn't obvious from the code alone (a specific sequence of requests, a timing window, a particular input sequence) → reproduction sequence.
5. **The confirmed cause crosses a service/file boundary** (3+ files/services involved) → symptom chain, cross-system, 4–6 nodes (one per file/service, not per line).
6. **Everything else with a confirmed symptom → cause chain** (1–2 files) → symptom chain, single system, 3 nodes.

If two rules match, prefer the earlier (more specific) one — e.g. a bug that's technically cross-system but really just the same typo copy-pasted into three files is still trivial, not a cross-system chain.

## Reading the investigation for nodes, not log lines

- A node is a *finding* (a symptom, a contributing factor, a file/service, a step), never a raw log line or stack frame.
- Skip investigation dead ends that didn't pan out — a hypothesis you ruled out gets, at most, one line in the prose caption, not its own node (except in the hypothesis-fork shape, where a ruled-out branch is the point).
- If a file's role in the chain is "just forwards the error" (e.g. a wrapper that rethrows), fold it into the node it forwards to rather than giving it its own.
- Only mark a node `.box-problem` (the confirmed-root-cause treatment) once you're actually confident — a diagram that marks an unconfirmed guess as confirmed is worse than no diagram.
