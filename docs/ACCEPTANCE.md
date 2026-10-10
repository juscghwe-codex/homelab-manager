# V1 acceptance criteria

## Local Git and CLI
- Detect dirty tracked AND untracked files without sudo; detect remote commit via `ls-remote` without fetching.
- Different commit hashes alone do not establish fast-forward/divergence; verify ancestry before classifying.
- Support single repo/multiple stacks and multiple repo/one host; only changed stack(s) need deployment.
- Dirty/ahead/diverged state blocks unattended apply and presents a diff/reconciliation path without deleting edits.
- Controller offline does not prevent local CLI use or interrupt running containers.

## Docker operations
- With capabilities disabled, unauthorized Docker commands are denied.
- Opt-in status returns images, IDs, state, health and metrics without env/secrets.
- `pull + up`, `up without pull`, and `recreate without pull` are distinct commands and produce auditable records; optional explicit `stop` is separately authorized.
- Maintenance-window scheduling and authenticated `deploy now` both honor preflight and authorization.
- Simulated failed health check triggers best-effort restoration and reports limitations, not false complete data rollback.

## Firewall/network
- Read nftables data and basic host topology; no attempt to infer Caddy/Jellyfin dependencies.
- Four policy-source types are supported; independent Git source can be represented without an agent.
- Router with unknown policy defaults to *assumed permitted*, not denied; never label this as verified connectivity.
- Permanent manually modeled router rules exist in normal/current model and survive exiting preview mode.
- Preview does not write to Git, system firewall or persistent baseline model.
- Known deny changes a modeled path's result and highlights impacted connections.
- Changes affect only expressly owned nftables tables/chains and never flush Docker-owned tables.
- Broken connectivity triggers host-local rollback without depending on controller reachability.

## UI and CI portability
- API/CLI operations exist without UI; UI calls identical authorized APIs.
- No Git hosting product hard dependency: plain remote Git over SSH works; Gitea/GitHub/GitLab can be adapters.
- Git-based change remains auditable, independently of CI provider. Git-only devices are never marked runtime-updated solely because Git changed.

## Testing environment
Use throwaway Git repositories, mocked Docker calls, disposable network namespaces/VMs for nftables integration and synthetic topology graphs. Never test privileged mutations against actual home infrastructure.
