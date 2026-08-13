# Glossary

Starter dictionary for clickable terms in diagrams (see `## Glossary` in `renderer.md` and the worked markup in `style-reference.html`). If a diagram uses a technical term not listed here, write a one-line definition inline in the same plain style — don't block on this list being incomplete.

Each term used in an output gets two anchors: `id="m-<slug>"` on its modal (opened by clicking the term) and `id="g-<slug>"` on its drawer entry (landed on via the modal's "All terms →" link). `<slug>` is the term lowercased with spaces/slashes replaced by `-`, e.g. "guard clause" → `guard-clause`.

| Term | Definition |
|---|---|
| guard clause | An early check at the top of a function that exits before running the rest, so invalid input never reaches the main logic. |
| null check | A test for a missing/empty value before using it, to avoid crashing on absence. |
| race condition | A bug where the outcome depends on the timing of two things happening at once. |
| boundary | The edge between two systems or modules, e.g. client/server, service A/service B. |
| side effect | A change a function makes beyond returning a value, e.g. writing to a file or database. |
| idempotent | Safe to run more than once — repeating it doesn't change the result further. |
| mutation | Changing a value or object in place, instead of creating a new one. |
| regression | A previously working behavior that broke, usually from an unrelated change. |
| memoization | Caching a function's result so repeated calls with the same input skip recomputation. |
| dependency injection | Passing a component what it needs from outside, instead of it creating those dependencies itself. |
| middleware | Code that runs between a request arriving and the final handler processing it. |
| closure | A function that keeps access to variables from where it was defined, even after that scope has ended. |
| callback | A function passed into another function to be run later, often after an async operation finishes. |
| async/await | Syntax for writing asynchronous (non-blocking) code so it reads top-to-bottom like synchronous code. |
| type coercion | Automatic conversion of a value from one type to another, e.g. a string to a number. |
| endpoint | A specific URL/route a client can call to reach a server operation. |
| deprecation | Marking code as discouraged/scheduled for removal, while it still works for now. |

## Adding new terms

When a diagram uses a term worth linking that isn't here, add it to this table (keep definitions to one plain-English sentence, no jargon-on-jargon) rather than defining it only inline — that way it's reused next time the term comes up.
