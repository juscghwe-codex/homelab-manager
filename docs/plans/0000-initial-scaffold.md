# Initial architecture scaffold

Status: documentation only. No implementation completed.

## Goal
Establish stable product intent, module boundaries, terminology, security constraints, and V1 acceptance tests before agentic implementation.

## Follow-up plan
Start with milestone 1 from `docs/PLANS.md`: host-local Git state inspection with fixture-based tests and a host-native CLI. Do not start with GUI or privileged executor code.

## Open decisions
- Final language and packaging choices for agent/controller/UI.
- Enrollment and certificate lifecycle format.
- Exact topology representation/graph evaluator algorithm.
- Local-only no-remote workflow (supported as future extension vs V1 optional).
- The exact range of nftables hooks and owned tables allowed in V1.
