## Intent

What V1 requirement or approved design decision does this change implement?
What concrete problem does it solve, and what changes for the caller/reviewer?

- Plan document and approved decisions:
- Scope and known limitations:

## Affected contracts

- Docs / APIs / agent behavior / privileged behavior / topology semantics:

## Security and authority impact

What can be read, modified, or executed that could not before?

- Authorization / privilege boundaries / deployment semantics / firewall impact:
- Required human reviewer for sensitive contract changes:

## Verification

- Tests run:
- Commands and actual results (pass/fail/skipped/unrun):
- Documentation links and `git diff --check` (when applicable):
- Disposable test environment used (if privileged):
- Known gaps:

## Compatibility

Do standalone agent and headless modes still work without the UI?
State whether this was verified or remains unimplemented/unrun.

## Integration

- Target `main` unless a different integration branch is documented.
- Keep one coherent concern and stage only relevant files.
- Match branch, commit and PR types: `<type>: <subject>` for commit subjects and PR titles; `<type>/<kebab-case-subject>` for branches (for example, `milestone`, `feat`, `fix`, or `docs`). Branch and PR describe the overall concern; each commit describes its individual change.
- Confirm no secrets, generated temporary data or local environment settings are included.
- Require explicit human review for authorization, privilege, deployment or firewall changes. Do not auto-merge.
