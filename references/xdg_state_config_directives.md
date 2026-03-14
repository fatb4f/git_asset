# XDG, State, and Config Directives

## Authority and placement

- Configuration belongs in `XDG_CONFIG_HOME` (default: `~/.config`).
- Persistent mutable runtime data belongs in `XDG_STATE_HOME` (default: `~/.local/state`).
- Cache or rebuildable artifacts belong in `XDG_CACHE_HOME` (default: `~/.cache`).
- User executables belong in `~/.local/bin` (or your declared user bin path).

## Planning directives

1. Never write plan artifacts into source trees unless explicitly requested.
2. Baseline evidence defaults to state space: `${XDG_STATE_HOME}/baseline/<timestamp>/`.
3. Keep config intent and runtime evidence separate:
   - intent: repo-tracked files under `dotfiles/config/...`
   - evidence: state files under `XDG_STATE_HOME`
4. If a path is unclear, resolve in this order:
   - explicit user path
   - tool argument
   - XDG default

## Systemd alignment

- User units should read config from `XDG_CONFIG_HOME` and write runtime outputs to `XDG_STATE_HOME` or `XDG_CACHE_HOME`.
- Avoid mixing journald logs with ad-hoc log files unless the service contract requires it.
- Treat generated files as derived artifacts; keep canonical config in tracked sources.

## Baseline-specific directives

- Capture both planes for environment resolution:
  - current process env
  - `systemctl --user show-environment`
  - declarative env sources (`~/.config/environment.d/*.conf`)
- Record command provenance from `$PATH` to prevent package-manager blind spots.
- Prefer deterministic commands and explicit timestamps for replayability.
