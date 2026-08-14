# Linear steps

3–6 ordered nodes, left to right (or top to bottom if labels run long). Use when the plan has a clear step order and no branching.

Example: "Add owner-unlock action to the private campaign route"

- Plan: add a server action, wire it into the route, update the client gate to call it.
- Steps:
  1. Add `unlockCampaignAction(campaignId)` — checks `session.userId === campaign.ownerId`
  2. Register the action on the campaign route
  3. Update `PrivateCampaignGate` to call it and re-render on success
- Prose: "Three steps, all in the campaign module — will confirm the ownership check server-side before touching the client gate."

## Rules

- Steps are units of work, not internal statements — if a step is too fine-grained to explain on its own ("declare a variable"), fold it into the step it serves.
- The last node is the plan's end state — mark it `.box-goal` (dashed accent outline + target mark, see `references/renderer.md`), never `.box-done`. Nothing is done yet.
- Cap at 6 steps; if the real plan has more, group a cluster into one node labeled with what it accomplishes (e.g. "wire up the three API routes").
