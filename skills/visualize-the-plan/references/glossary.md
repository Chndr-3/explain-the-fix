# Glossary

Starter dictionary for clickable terms in plan diagrams (see `## Glossary` in `renderer.md` and the worked markup in `style-reference.html`). If a diagram uses a technical term not listed here, write a one-line definition inline in the same plain style — don't block on this list being incomplete.

Each term used in an output gets two anchors: `id="m-<slug>"` on its modal (opened by clicking the term) and `id="g-<slug>"` on its drawer entry (landed on via the modal's "All terms →" link). `<slug>` is the term lowercased with spaces/slashes replaced by `-`, e.g. "feature flag" → `feature-flag`.

| Term | Definition |
|---|---|
| feature flag | A runtime switch that turns a code path on or off without a new deploy, often used to roll a change out gradually. |
| rollout | Releasing a change to users gradually (a percentage, a region, a beta group) instead of all at once. |
| fallback path | What the system does when the preferred approach isn't available — a planned backup, not an error case. |
| spike | A short, time-boxed investigation to answer an open question before committing to an implementation. |
| milestone | A checkpoint in a plan where a meaningful, checkable chunk of work is complete. |
| cutover | The moment a system switches from an old path to a new one, usually the last step of a migration. |
| migration | Moving data or behavior from one structure/system to another, usually in stages to avoid downtime. |
| backward compatibility | New code still working correctly with old data, old clients, or old callers. |
| rate limiting | Capping how often an action can happen in a given window, usually per user or per client. |
| edge cache / CDN | A cache located close to the user geographically, so repeat requests don't have to reach the origin server. |
| idempotent | Safe to run more than once — repeating it doesn't change the result further. |
| dependency | A step or piece of work that another step needs to be finished before it can start. |
| technical debt | Shortcuts taken to ship faster that will cost extra work to clean up later. |
| scope | The set of files, modules, or behaviors a plan intends to touch. |
| regression | A previously working behavior that breaks as a side effect of a change. |
| API layer | The boundary code that receives requests and translates them into internal calls — where you'd add a server-side check. |

## Adding new terms

When a diagram uses a term worth linking that isn't here, add it to this table (keep definitions to one plain-English sentence, no jargon-on-jargon) rather than defining it only inline — that way it's reused next time the term comes up.
