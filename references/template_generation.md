# Template Generation Reference

## Purpose

`scripts/generate_plan_packet.py` creates a plan packet scaffold from the canonical template.

## Invocation

```bash
scripts/generate_plan_packet.py \
  --objective "Add runtime check to spawn" \
  --failure "No deterministic preflight check exists" \
  --wt "$WT"
```

For `gh issue create --body-file`, generate body-only output:

```bash
scripts/generate_plan_packet.py \
  --objective "Add runtime check to spawn" \
  --failure "No deterministic preflight check exists" \
  --mode body \
  --out "$WT/PLAN/plan_body.md"
```

## Behavior

- Uses `assets/README.plan.template.md` by default.
- Template is issue-template shaped (GitHub compatible frontmatter + markdown body).
- When a plan introduces generated artifact families, the packet should explicitly capture:
  - canonical schema refs
  - template refs
  - optional CUE overlay refs
  - `json-tool` as canonical JSON actuator
  - `MiniJinja` as textual projection boundary only
- Relative `--template` paths are resolved from the skill root (`.../skills/plan`), not caller CWD.
- Seeds `problem_set` with:
  - `Expected behavior` from `--objective`
  - `Failure signatures` from repeatable `--failure`
- Replaces `title: \"[plan] <objective>\"` with the provided objective.
- `--mode body` strips issue-template frontmatter and emits markdown body only.
- When `--wt` (or env `WT`) is provided and `--out` is omitted, output defaults to `$wt/PLAN/README.md`.
- Also writes `$wt/PLAN/packet.meta.json` for machine-readable metadata.
- Does not execute runtime commands.
- Refuses to overwrite unless `--force` is passed.
