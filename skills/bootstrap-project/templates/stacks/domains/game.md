## Stack notes — Game

### Toolchain & commands
- Engine {{engine}} {{engine_version}} (pin it; editor and CI must match). Run: {{run_notes}}.
- Asset/import caches ({{cache_dirs}}) are never committed; scenes/prefabs/assets are
  {{binary_asset_policy}} (merge conflicts there are expensive — coordinate before editing).

### Agent failure modes
- Editing scenes/prefabs/serialized assets for what code can do — asset diffs are
  unreviewable; prefer code, and flag asset edits explicitly.
- Per-frame allocations in hot loops (GC hitches); string concat/LINQ/closures in update
  paths ({{hot_paths}}).
- Frame-rate-dependent logic — missing delta time, physics outside the fixed step.
- Breaking determinism where replays/netcode/seeded systems require it ({{determinism_areas}}).
- "It runs in the editor" as proof — editor and build behave differently ({{build_target}}).

### Verification norm
Proven = play the affected loop in a build (not just the editor) on {{build_target}};
perf-sensitive changes profiled before/after against {{perf_budget}}.

### Interview add-ons
- Engine + version + how it's pinned? (infer: ProjectSettings, project.godot, .uproject)
- Perf budget + primary build target? (state it, or perf lines drop)
- Determinism constraints (netcode/replays)? (ask if multiplayer signals present)
- Asset-edit coordination rules? (infer: LFS config, .gitattributes)
