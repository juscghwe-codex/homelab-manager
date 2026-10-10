# Codex work plan and milestone contracts

Before implementing nontrivial functionality, add `docs/plans/<NNNN>-<topic>.md` with purpose, affected contracts, design, security assumptions, unit/integration tests, acceptance checks and recovery approach. Complete one vertical slice per PR. Keep open decisions explicit; avoid turning guesses into permanent architecture.

## Milestones

### 0 — Docs and contracts
Freeze terminology, API contracts and ownership boundaries. Add fixture specifications, no privileged code. Deliver docs-first PR.

### 1 — Agent-only Git core
CLI, local revision markers, dirty/tree checks, `ls-remote`, staged immutable revision, multi-repo stack mapping and fixture tests. Works as unprivileged user; no controller.

### 2 — Docker executor
Opt-in Docker status/deploy operations, fixed stack allowlists, audited local privileged boundary, image ID/config hashing, pull/apply/recreate behavior and rollback journal. No implicit stop permission or blanket sudo.

### 3 — Firewall inventory, model and executor
Read Linux interfaces/routes/nftables. Persistent manual and Git-only devices. Optimistic path evaluator with provenance. Read-only preview. Separate nftables executor limited to owned tables; durable rollback timer and fresh-channel verification. Test only in disposable network namespaces/VMs.

### 4 — Headless controller/API
mTLS enrollment, controller-agent job protocol, maintenance scheduler, Git provider adapters, topology access engine, dependency-free CI CLI integration. Keep server-side Git caching non-authoritative.

### 5 — Optional UI
Graphical inventory, groups and annotation edges; current model versus preview changes; host status, Git diff/editor, deployed rev view; operations through controller APIs only. Include clear source and certainty labels.

## Definition of done for each milestone
- Tests implement relevant `docs/ACCEPTANCE.md` criteria and pass in the available test environment.
- Security assumptions, API changes and known limitations are documented.
- Headless/agent-only modes remain usable where applicable.
- No misleading assertion of success for untested runtime behavior.
