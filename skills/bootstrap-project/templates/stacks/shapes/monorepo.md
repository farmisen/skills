## Stack notes — modifier: monorepo (auto-detected)

- Per-package commands: `{{per_package_cmd}}`; affected-only: `{{affected_cmd}}` — use
  the affected scope; running the world when the tool can scope is wasted signal and time.
- Package boundaries are real: import through public entry points, never deep-reach into
  another package's internals ({{boundary_enforcement}}).
- Shared config packages ({{shared_config_pkgs}}) have repo-wide blast radius — check
  affected output before changing one, and say so in the change.
- Dependency version drift across packages is managed by {{dep_sync_tool}}; don't
  hand-bump one package out of sync.

### Interview add-ons
- Tool + affected command? (infer: turbo.json, nx.json, moon.yml, workspaces)
- Shared config packages? (infer: packages/*-config)
- Boundary enforcement? (infer: eslint boundaries rules, nx tags)
