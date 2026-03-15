# Sweep Tool Reference

## Purpose

The sweep capability now lives in the `sweep` skill:

- `../sweep/scripts/baseline_sweep.sh`
- `../sweep/scripts/diff_sweeps.py`

It captures preflight/postflight artifacts and liveliness state.

## Invocation

```bash
../sweep/scripts/baseline_sweep.sh [OUT_BASE]
```

- `OUT_BASE` (optional): root output directory.
- Default output root: `$wt/SWEEP/<phase>/...` when `WT` is set.

## Output contract

The script prints the final output directory on stdout, then writes:

- `manifest.env`: run metadata and effective settings
- `summary.json`: structured liveliness summary
- `status.tsv`: per-step execution status (`id`, `critical`, `rc`, `duration_s`)
- `raw/*.out`: command stdout per step
- `raw/*.err`: command stderr per step

## Key environment controls

- `SWEEP_PHASE` (`preflight` or `postflight`)
- `SWEEP_LOG_SINCE` (default: `-2h`)
- `SWEEP_TIMEOUT_S` (default: `15`)
- `SWEEP_LONG_TIMEOUT_S` (default: `45`)
- `SWEEP_FAIL_ON_CRITICAL` (default: `1`)
- `WT`, `REPO_ROOT`, `DOTFILES_REPO`, `WORKTREE_BASE`

## Exit behavior

- Exit `0`: baseline completed and critical checks satisfied.
- Exit `1`: critical command failure or missing critical artifact (when `BASELINE_FAIL_ON_CRITICAL=1`).

## How to use in the plan loop

1. Run preflight sweep before PLAN.
2. Attach generated `manifest.env` + `status.tsv` as baseline evidence.
3. Use `raw/*.out` for `assets_required`, `assets_impacted`, and `matrix` sections.
4. Run postflight sweep after IMPLEMENT and diff against preflight using:
   `../sweep/scripts/diff_sweeps.py --pre <pre_dir> --post <post_dir>`.

## Proof reference format

Use file-anchored references in matrix rows, for example:

- `raw/user_units_all.out`
- `raw/unit_show_otel_backend_listener_service.out`
- `raw/journal_focus.out`
- `status.tsv`
- `manifest.env`

Mark each proof as:

- `observed`: directly present in sweep artifacts.
- `inferred`: derived from observed artifacts and stated assumptions.
