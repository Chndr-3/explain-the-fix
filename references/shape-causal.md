# Causal chain

Trigger → Root cause → Fix. Nodes are events/states, not files (single-system) or one node per file/service (cross-system).

## Single system (3 nodes)

Example: "Private campaign gate shows password prompt to owner"

- Diff: 1 file (`PrivateCampaignGate.tsx`), one `if` condition wrong.
- Nodes:
  1. **Trigger** — Owner opens a private campaign they own
  2. **Cause** — Gate checked `isPrivate` but never checked `isOwner`
  3. **Fix** — Added `isOwner` short-circuit before the password check
- Prose: "Owners were hitting the password gate meant for outside visitors; added an ownership check. Existing gate tests still pass."

## Cross-system (4–6 nodes, one per file/service)

Example: "Auth token expiry not respected across API gateway + auth service"

- Diff: 2 services (`api-gateway`, `auth-service`), cause crosses the boundary.
- Nodes:
  1. **Trigger** — Expired token still accepted at the gateway
  2. **api-gateway** — cached the token's validity for 5 min, didn't recheck expiry
  3. **auth-service** — issues correct `exp` claim, not the problem
  4. **Root cause** — gateway cache TTL outlived token TTL
  5. **Fix** — gateway cache TTL now derived from token `exp`, not fixed 5 min
- Prose: "Gateway was trusting a stale cache past the token's real expiry; cache TTL now tracks the token. Verified with an expired-token integration test."

Keep the chain linear — no branching diamonds. If the real cause has two contributing factors, pick the dominant one and mention the other in the one-line prose instead of adding a node.
