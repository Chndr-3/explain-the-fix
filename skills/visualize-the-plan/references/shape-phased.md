# Phased plan

2–4 phase containers, left to right, each holding 2–3 child steps. Use when the plan's steps naturally group into milestones rather than reading as one flat sequence.

Example: "Migrate the report exporter to the new queue"

- Plan groups into three phases:
  - **Spike** — confirm the new queue's API covers the current retry semantics
  - **Implement** — swap the producer, swap the consumer, keep the old path behind a flag
  - **Cut over** — flip the flag, remove the old path once a week of exports look clean
- Prose: "Three phases, flag-gated cutover — will confirm the new queue supports at-least-once delivery before touching the consumer."

## Rules

- A phase container is `.dash` chrome (same as `visualize-the-fix`'s before/after containers, generalized to N containers) — dashed stroke, no fill, holding a short mono or plain-text list of its children.
- Connect containers left to right with the same dot + flow-line connector as linear steps — one arrow per phase boundary, not one per child.
- The last phase's container gets the accent-line border (not the earlier phases) to mark it as the plan's end state — same idea as `.box-goal`, applied to a container instead of a single box.
- If a phase would need more than 3 children, collapse the smallest ones into "other" rather than exceeding the 8-node total budget across all containers.
- 2 containers minimum (below that it's not "phased," it's linear); 4 containers maximum before the row gets too wide to stay glanceable — collapse further first.
