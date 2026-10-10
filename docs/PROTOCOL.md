# Protocol contracts (V1 target)

The specifics of HTTP routes and Go/Python DTOs are implementation decisions; preserve these invariants.

## Agent -> controller status
Report stable host ID, scope, timestamp, git refs/state, last applied deployment transaction, optional sanitized Docker metrics, and discovered topology/policy facts. mTLS identifies the agent; an untrusted JSON field cannot override identity.

## Controller -> agent jobs
A job MUST bind: host, registered stack or owned firewall policy identifier, operation, exact approved revision, authorization identity, unique transaction ID, expiry/deadline, and explicit approval outcome. Job fields may never supply arbitrary sudo commands or filesystem paths.

## Deployment sequence
1. `PREPARE` — authenticate job, compare remote advertised ref, inspect local dirty/divergence, fetch exact approved objects, stage candidate without overwriting active tree; validate.
2. `READY` — return transaction ID, exact staged commit, preflight result, affected stack/policies, last applied revision.
3. `APPLY` — explicitly authorized transition; arm local rollback mechanism when required, run only enabled operations.
4. `VERIFY` — local health and status check; report runtime identities and a new outbound status message.
5. `CONFIRM` — controller verifies new authenticated contact/reachability as appropriate and confirms matching transaction/revision; agent finalizes deployment marker.
6. `ROLLBACK_ATTEMPT` — on failure or timeout, host performs best-effort local revert; journal every transition. A Compose rollback does not undo persistent-data migrations.

Standalone CLI supplies equivalent local confirmation; it never requires a controller to use Git status or apply a locally authorized operation.

## Retry rules
- Repeated messages with the same transaction ID must be idempotent.
- Unrecognized/expired/replayed transactions are rejected.
- Multiple concurrent mutations to the same stack/policy are serialized.
- A network interruption never implies success; agents retain local journal and recovery timer.

## Topology preview
`GET current model` may combine agent facts, independent Git sources, discovered unmanaged nodes and persistent manual modeling. `POST preview delta` MUST be read-only and return provenance, affected access paths and uncertainty/assumption labels. Persist/apply is a separate, explicit operation. For a Git-only device, a commit is never mistaken for verified runtime application.

## Versioning
Version API schemas from V1 onward. Persist `schema_version` with serialized documents. No breaking protocol change without an explicit migration and interoperability tests.
