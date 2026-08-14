# New flow / feature (sequence)

3–6 ordered steps. Use when a new function/endpoint/route was added and reading the diff top-to-bottom shows a clear order of operations.

Example: "Add owner-unlock server action to the private campaign route"

- Diff: 1 new server action + 1 call site wiring it into the route.
- Steps:
  1. Owner clicks "Unlock" on their private campaign
  2. Client calls `unlockCampaignAction(campaignId)`
  3. Server action checks `session.userId === campaign.ownerId`
  4. On match: sets `unlocked` flag, returns success
  5. Client re-renders gate as unlocked
- Prose: "New server action gates the unlock behind an ownership check server-side, not just client-side. Covered by a new action test."

## Rules

- Steps are participant → participant messages, not internal statements — if a step doesn't cross a function/component/service boundary, it's not sequence-diagram material; fold it into the adjacent step.
- Order matters here more than in the other shapes — don't reorder steps for visual balance.
- Cap at 6 steps; if the real flow has more, group a cluster of internal steps into one node labeled with the boundary it crosses (e.g. "validates + persists").
