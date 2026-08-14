# Glossary

Starter dictionary for clickable terms in problem diagrams (see `## Glossary` in `renderer.md` and the worked markup in `style-reference.html`). If a diagram uses a technical term not listed here, write a one-line definition inline in the same plain style — don't block on this list being incomplete.

Each term used in an output gets two anchors: `id="m-<slug>"` on its modal (opened by clicking the term) and `id="g-<slug>"` on its drawer entry (landed on via the modal's "All terms →" link). `<slug>` is the term lowercased with spaces/slashes replaced by `-`, e.g. "race condition" → `race-condition`.

| Term | Definition |
|---|---|
| root cause | The actual underlying reason something is broken, as opposed to the symptom a user sees. |
| race condition | A bug where the outcome depends on the timing of two things happening at once. |
| stale read | Reading a value that was correct at some point but has since changed, without noticing it changed. |
| stack trace | The list of function calls active at the moment an error was thrown, used to trace where it happened. |
| regression | A previously working behavior that broke, usually from an unrelated change. |
| flaky test | A test that sometimes passes and sometimes fails without any code change, usually from a timing or ordering issue. |
| edge case | An input or situation at the extreme boundary of what's expected, easy to miss when writing the main logic. |
| null reference | An attempt to use a value that turned out to be missing/empty, causing a crash. |
| memory leak | Memory that's no longer needed but never released, causing usage to grow over time. |
| deadlock | A situation where two or more processes are each waiting on the other, so neither can proceed. |
| off-by-one error | A bug where a loop or index is wrong by exactly one, often from a `<` vs `<=` mixup. |
| silent failure | An error that occurs but isn't logged or surfaced, making it invisible until its effects show up elsewhere. |
| backpressure | A signal from a slower downstream system telling an upstream one to slow down, so it isn't overwhelmed. |
| idempotent | Safe to run more than once — repeating it doesn't change the result further. |
| memoization | Caching a function's result so repeated calls with the same input skip recomputation, which can go stale if the cache key is wrong. |
| blast radius | How much of the system is actually affected by a given bug or incident. |

## Adding new terms

When a diagram uses a term worth linking that isn't here, add it to this table (keep definitions to one plain-English sentence, no jargon-on-jargon) rather than defining it only inline — that way it's reused next time the term comes up.
