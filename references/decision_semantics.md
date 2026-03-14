# Decision Semantics

Use this at the end of every plan packet.

## Status values

- `APPROVED`
: Plan is sufficient to execute within declared scope and approval gate.

- `REJECTED`
: Plan is not executable/safe due to unresolved blockers or weak evidence.

- `NEEDS_INFO`
: Plan may be viable but evidence is incomplete; further collection required.

## Minimum decision content

- Decision rationale (concise, evidence-backed)
- Blocking items (for `REJECTED`)
- Required follow-up evidence (for `REJECTED` or `NEEDS_INFO`)
- Re-entry condition for re-review

## Quality bar

- No decision without matrix proofs.
- If any critical dependency is `blocked`, do not mark `APPROVED`.
- If assumptions drive key plan phases, mark `NEEDS_INFO` unless validated.

## Decision actions

- On `APPROVED`:
  - Generate one sub-issue per deliverable from the plan.
  - Link each sub-issue to the parent plan issue.
  - Preserve deliverable ID in issue title/body.
  - Move to `implement` only when `evidence_required` is mapped and gate status is explicit.
  - Use `dry-run` by default; use `apply` only when evidence gate is `PASS`.

- On `REJECTED`:
  - Restate `problem_set` before next planning pass.
  - Include what changed (new evidence, invalid assumptions, corrected scope).
