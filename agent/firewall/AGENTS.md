# Firewall executor instructions

Read root and `agent/AGENTS.md`, `docs/SECURITY.md` and `docs/DATA_MODEL.md`.

- V1 writable backend is Linux nftables. Other Linux firewall backends are adapters, not blanket auto-detection-and-rewrite.
- Read-only inventory, topology evaluation, preview, and privileged runtime mutation are different operations.
- Never flush global rulesets or Docker-owned/unrelated tables/chains. Only mutate owned, declared resources.
- Validate syntax and applicable semantics before apply; preserve ordering, stateful conntrack assumptions and NAT/hook awareness.
- Arm a durable host-local rollback *before* remote policy apply. Only cancel after matching transaction confirmation using a **new** authenticated connection; surviving existing TCP connection does not prove reachability.
- Unmanaged modeled devices are permanent baseline, even with no agent. Source != authority: Git edit is not runtime apply.
- Unknown intermediate filtering defaults to `assumed_permitted`, not verified/observed.
- Never infer service dependencies from Caddy/application config.
- Test policies and rollback with disposable network namespaces/VMs; never test lockout against developer's host.
