# Classification heuristic

Cheap, not perfect. Run in this order, take the first match:

1. **Single-line diff, no branching cause** (typo, string, constant, config value) → trivial, 2 nodes.
2. **New function/route/endpoint added, and reading the diff top-to-bottom the steps have an order** (validate → call → respond) → new flow, sequence shape.
3. **File count changed (1 file → N files, or N files → 1), or a class/component got split/merged, and behavior is unchanged** → structural refactor, before/after shape.
4. **Diff touches 3+ files or crosses a service/module boundary (e.g. gateway + backend, frontend + API), and there's one identifiable cause** → causal, cross-system, 4–6 nodes (one per file/service, not per line).
5. **Everything else with a clear trigger → cause → fix** (1–2 files) → causal, single system, 3 nodes.

If two rules match, prefer the earlier (smaller/simpler) one — e.g. a 3-file change that's really just a typo in three call sites is still trivial, not cross-system.

## Reading the diff for nodes, not lines

- A node is a *concept* (a file, a function, a step), never a line of the diff.
- Skip files that only changed because of a rename/reformat with no logic change.
- If a file's role is "just wiring" (e.g. re-export, route registration), fold it into the node it wires rather than giving it its own.
