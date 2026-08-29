## Stack notes — shape: CLI tool

- Interface stability: flags, exit codes, and stdout format are the public API — changing
  any of them is a breaking change and needs the same care as a library API break.
- stdout is for output, stderr for diagnostics; keep machine-readable output (--json)
  clean of logs so pipes don't break.
- Agent failure modes: interactive prompts in what must stay scriptable; reading config
  from cwd instead of the documented precedence ({{config_precedence}}); help text drifting
  from actual flags.
- Proven = run the built binary against real invocations ({{smoke_invocations}}), not just
  unit tests.

### Interview add-ons
- Flag/output stability promise: is anything consuming --json/exit codes downstream?
- Config precedence? (infer: README, existing config loading code)
