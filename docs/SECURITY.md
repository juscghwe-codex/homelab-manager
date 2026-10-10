# Security and privilege model

## Actors and authority
- Git repo reader may observe tracked configuration; Git writer may propose/commit changes.
- Controller may coordinate authorized jobs but must not gain implicit unrestricted root access.
- Host-native agent performs Git checks as dedicated unprivileged service identity.
- Docker executor is an explicitly authorized privileged boundary. A rootful Docker Compose deployment of arbitrary approved YAML can be **root-equivalent**: Compose may request host binds, privileged containers, namespaces, capabilities or devices.
- Firewall executor has independent authorization; Docker deployment credentials do not authorize firewall changes.
- A modeled device (including one from Git) is not automatically a managed device.

## Credentials
- mTLS for agent/controller connection is the preferred design. Agent private keys generated and retained on host, with one-time, authenticated enrollment and revocation.
- Git credentials are scoped per repository/operation; avoid controller-shared keys that can write across all hosts.
- CI may use scoped short-lived credentials where available. Prevent replay, enforce authorized host/stack/revision and transaction IDs.
- Do not transmit/collect raw `.env`, secrets, keys, resolved secret config, `docker inspect` environment fields, or persistent volumes.

## Privileged execution
- Never expose a Docker TCP API or mount unrestricted `docker.sock` into the management UI/controller.
- A locally privileged executor may accept only explicit operations and configured stack IDs; never arbitrary shell fragments, file paths or Docker flags from remote callers.
- For every operation, use fixed allowlisted local paths, defined unit/capability boundaries, and a documented approval policy. Separate read-only inspection from mutation where possible.
- Authorization is about WHO approves WHAT revision and operation, not simply whether a certificate exists. A Git hash alone does not prove authorization.
- For nftables, own isolated policy tables/chains; do not flush Docker-owned or unrelated firewall state. Verify supported hook semantics prior to application.

## Fail safe behavior
- Reject unattended updates when local Git is dirty/ahead/diverged or a required approval is absent.
- Preserve existing running services if controller/Git become unreachable.
- Before remote firewall application, stage valid rules, snapshot managed state, arm a durable host-local rollback deadline, then apply atomically when possible. Verify a **fresh** authenticated management connection and explicitly confirm before disarming rollback.
- Report partial failures and limits. Rolling back a Compose revision does not restore databases or persistent volumes after migrations.

## Security review gates
Required negative tests: forged job/revision; replayed confirmation; path traversal and shell injection; agent outage; missing Git commit; dirty local tree; Docker status secrets leakage; unauthorized stop; unauthorized firewall apply; firewall breaks connectivity; rollback timer survives a daemon restart; no unintended edits outside owned firewall tables.
