# Headless controller instructions

Read root `AGENTS.md`, `docs/INTENT.md`, `docs/DESIGN.md`, `docs/DATA_MODEL.md`, `docs/PROTOCOL.md` and `docs/SECURITY.md`.

## Responsibility
Optional, provider-neutral coordination of agents and Git sources, job approval/dispatch, maintenance schedules, topology inventory, policy evaluation and what-if preview. Headless API is a first-class product; the GUI must never be required.

## Constraints
- Never become an alternate authoritative Compose configuration store. Git remote is desired configuration; agent local worktree/journal is local observed/applied state.
- Support Git-based policies even when no agent exists for the modeled device.
- Treat policy provenance, editing authorization and runtime apply authority separately.
- Persist manually defined devices and annotations as permanent model state; preview changes are temporary overlays and cannot mutate baseline.
- Open-world connectivity: no known restriction => `assumed_permitted`; unknown infrastructure does not automatically block, but do not claim verified reachability.
- No application-specific dependency crawling or Caddy parsing.
- Authenticate and authorize each job against host, stack, revision, operation and expiry; prevent replay and cross-host execution.
- Avoid possessing root/Docker-socket credentials for managed hosts.

## Tests
Synthetic topology graph fixtures, unknown-router/default allow, known deny, persistent manual model, preview isolation, missing agents, Git-only source, request replay and unavailable hosts.
