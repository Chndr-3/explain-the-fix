# Classification heuristic

Cheap, not perfect. Run in this order, take the first match:

1. **One step, or steps too small to have real structure** (a rename, a config tweak, "just run the migration") → trivial, 2 nodes or skip the diagram.
2. **The plan's substance is a conditional** — "if the flag is set do A, otherwise do B," a rollout with a fallback path, a choice between two approaches you're presenting for the user to pick → decision plan, fork shape.
3. **The plan's substance is which files/modules will be touched**, more than the order you'll touch them in (e.g. "add a field here, a migration there, a UI control over there") → scope map, Now/Planned before-after.
4. **Steps naturally group into 2–4 phases or milestones** (e.g. "backend," "frontend," "cleanup," or "spike," "implement," "harden") → phased plan, one container per phase.
5. **Everything else with a clear step order, ≤6 steps** → linear steps, sequence shape.

If two rules match, prefer the earlier (smaller/simpler) one — e.g. a 3-file plan that's really just "add the same field in three places" is still a linear sequence, not a scope map.

## Reading the plan for nodes, not sentences

- A node is a *unit of work* (a step, a phase, a file/module), never a sentence of your own explanation.
- Skip steps that are pure bookkeeping with nothing to show ("commit the changes," "write the summary").
- If a step is "just wiring" (e.g. registering something you built in the previous step), fold it into the step it wires rather than giving it its own node.
- Order matters for linear/phased/sequence shapes — don't reorder steps for visual balance, and don't invent an order the plan doesn't actually have.
