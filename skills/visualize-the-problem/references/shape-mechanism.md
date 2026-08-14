# Mechanism deep dive

3 nodes, vertical: Symptom → Root cause → Why it happens. Use when the user asks *why* a bug happens at a mechanism level, not just what's wrong — the diagnostic equivalent of `visualize-the-fix`'s deep dive and `visualize-the-plan`'s rationale.

Example: "Why does this only fail under concurrent writes?"

- Symptom: **Row occasionally saved with a stale `updated_at`**
- Root cause: **Read-modify-write on the row isn't atomic**
- Why it happens: "Two concurrent writers both read the row before either writes; the second write silently overwrites the first's changes along with its timestamp, because there's no version check between read and write."
- Prose: "Confirmed via a repro with two concurrent updates; single-writer paths never show this."

## Layout rule

- Same vertical `.spine` connector as the siblings' deep-dive/rationale shapes (short dashed line, not an arrow-flow — this tier is a drill-down, not a sequence).
- **Symptom** is a plain `.box`. **Root cause** gets `.box-problem` (solid accent outline + warning mark) — not `.box-done` (nothing here is fixed) and not `.box-goal` (nothing here is proposed).
- **Why it happens** drops the box fill entirely, same as the siblings: `.dash` outline only, accent bar on the left edge, holding one sentence of `.n-prose`.
- Use this shape only on explicit request ("why," "why does this happen," "what's the mechanism") — don't default to it just because a bug has an interesting cause; that's what the one-line prose caption is for.
