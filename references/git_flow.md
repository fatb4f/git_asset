# Git Flow Reference

## Baseline model

- Trunk branch is `main`.
- Work is composed in an integration worktree/branch from an immutable base SHA.
- One logical commit per deliverable.
- Run gate checks per deliverable before stacking the next one.
- Promote via squash into `main`.
- Preserve integration lineage with a branch/tag for audit.

## Command skeleton

```bash
# create integration canvas
git worktree add ../wt-int-<run_id> -b int/<run_id> <base_sha>

# inside integration worktree, per deliverable
git add -A
git commit -m "D-001: <deliverable summary>"
# run deliverable gates

# promote to trunk as squash
git checkout main
git merge --squash int/<run_id>
git commit -m "promote(<run_id>): D-001..D-00N"

# keep lineage
git tag int/<run_id>-final int/<run_id>
```

## Deliverable commit conventions

- Commit trailer fields:
  - `Deliverable: D-<id>`
  - `Gate: pass|needs-human`
  - `Base-Ref: <sha>`

## Conflict handling

- Resolve conflicts on integration branch.
- Re-run gates after conflict resolution.
- If scope grows, split deliverable into smaller commits instead of mixed edits.

## Planning usage

In `matrix` and `plan` sections:
- Mark git assumptions explicitly (`base_ref`, trunk, gating rules).
- Reference concrete proof when available (`git log`, branch state, gate outputs).
- For `APPROVED`, derive one sub-issue per deliverable from the plan.
