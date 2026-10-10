# Data model and terminology

## Core entities
- `Host`: stable identity, managed/unmanaged status, discovered interfaces/routes and optional agent ID.
- `Network`: network segment or user-declared logical group; avoid interpreting logical membership as physical bridging/VLAN connectivity.
- `Device`: router, switch, Linux host or other modelled component. May be discovered, manually entered, agent-managed or Git sourced.
- `Stack`: host + repository + tracked branch/ref + path + Compose project name. Multiple stacks can share one repository and a host can reference many repositories.
- `PolicySource`: `agent`, `git_independent`, `discovered_unmanaged`, or `manual`. Source is distinct from `can_edit_git` and `can_apply_runtime`.
- `AccessRule`: source, destination, direction, protocol, port ranges, action, provenance, authority status; explicit rule order/semantics when known.
- `Annotation`: manually authored edge/group/label; no implied connectivity or service dependency.
- `Deployment`: immutable target Git commit, host/stack, command, approval, transaction, deadline and outcome.

## Agent status (minimum meaningful fields)
- `git.local_head`: checkout commit.
- `git.remote_head`: advertised remote commit, or unavailable.
- `git.dirty`: any changed/untracked files in managed scope; report categories without leaking contents.
- `git.applied_revision`: last successfully applied tracked revision (journal/ref).
- `compose.desired_hash`: sanitized effective service configuration fingerprint when inspect capability enabled.
- `compose.running_hash`: observed Compose hash/label when available, with caveat that not all runtimes expose it.
- `container.image_id`: running image content identity when Docker read capability enabled; registry digest may be additional metadata.
- `runtime.state`, `runtime.health` and `runtime.metrics`: optional, only when enabled.

## Git state labels
- `synced`: clean and desired/applied revisions match, subject to runtime verification.
- `update_available`: remote ref differs and is known to be ahead; only after ancestry established.
- `dirty`: uncommitted tracked or untracked changes in managed scope.
- `ahead` / `diverged`: unpublished local commits / incompatible history; don't auto-reset.
- `remote_unavailable`: cannot query remote; do not conclude nothing changed.
- `drifted_runtime`: observed runtime differs from last applied/desired configuration (when measurable).

## Connectivity result labels
- `permitted`: all evaluated known policies permit a modeled path; not necessarily tested.
- `assumed_permitted`: no known blocker, but one or more policies/devices unknown. DEFAULT for unknown restrictions.
- `blocked`: a known applicable policy rejects a path.
- `no_path`: discovery positively establishes no usable path; never infer this from unknown router policy or different VLAN IDs alone.
- `preview_change`: delta result only, from proposed rules, never the persisted baseline.

Every result records whether evidence was observed, Git-declared, manually modelled, or inferred by open-world default. Do not silently collapse `permitted` and `assumed_permitted` into "confirmed reachable".
