# Agent module instructions

Read root `AGENTS.md`, `docs/DESIGN.md`, `docs/SECURITY.md`, `docs/DATA_MODEL.md` and `docs/PROTOCOL.md` before changing this module.

## Responsibility
Host-local Git worktree inspection, registered stack resolution, local execution coordination, deployment journal, machine-readable status and optional systemd service. Agent-only CLI must work without controller, UI or hosted Git platform. Prefer small host-native executable (Go proposed).

## Constraints
- Git operations are unprivileged and never overwrite dirty/unpublished local changes.
- `ls-remote` only advertises refs; it does not transfer commit objects. Fetch and validate exact approved commits before staging/applying.
- Never confuse `HEAD` with last successfully deployed revision.
- Default status is minimal. Runtime hashes/health/CPU/RAM depend on enabled local capabilities.
- Do not read `.env` contents or volume contents to display/configure services.
- No direct arbitrary shell or dynamic Docker flags from remote API; local executor validates configured stack IDs and fixed paths.
- Keep Docker and nftables execution privileges independent. `compose up` may stop/replace containers, despite an optional explicit `stop` command.
- Maintain durable, idempotent transaction journal and operation audit; failed deployment may only perform best-effort container/config rollback.

## Tests
Fixture-based Git divergence, dirty/untracked files, staged commit, idempotent job retries, missing/malicious paths, controller outage and sanitized status output. Privileged tests run in disposable environments only.
