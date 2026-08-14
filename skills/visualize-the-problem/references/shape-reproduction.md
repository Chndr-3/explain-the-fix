# Reproduction sequence

3–6 ordered steps, ending in the failure. Use when the order of steps needed to trigger the bug matters and isn't obvious just from reading the code — a specific request sequence, a timing window, a particular input order.

Example: "Cart total is wrong only after applying a coupon then removing an item"

- Investigation: reproducing it needs a specific order; doing the same actions in a different order doesn't trigger it.
- Steps:
  1. Add two items to cart
  2. Apply a percentage coupon — discount computed against current subtotal
  3. Remove one item — subtotal recalculates, discount amount doesn't
  4. **Failure** — Total shown is subtotal minus a discount computed against a subtotal that no longer exists
- Prose: "Reproduces every time in this order; removing before applying the coupon doesn't trigger it. Discount amount needs to recompute on cart changes, not just apply once."

## Rules

- Steps are actions or state transitions, not internal statements — if a step doesn't change what the user or system does next, fold it into the adjacent step.
- Order matters here more than in the symptom-chain shapes — don't reorder steps for visual balance, and don't omit a step that's actually required to trigger the bug.
- The terminal node is the failure itself, marked `.box-problem` (solid accent outline + warning mark) — not `.box-done` (that means resolved) and not `.box-goal` (that means proposed).
- Cap at 6 steps; if reproduction genuinely needs more, group a cluster of setup steps into one node labeled with what state it establishes (e.g. "cart has a coupon and 2 items").
