## Stack notes — Elixir

### Toolchain & pinning
- Elixir/OTP pinned in `{{version_pin_file}}` (.tool-versions / mise). Deps via mix;
  {{umbrella_note}}.

### Commands
- Single test: `mix test path/to/test.exs:LINE`. Full gate: `{{check_alias}}`
  (run it when done and fix everything it reports).

### Agent failure modes
- HTTP with httpoison/tesla/httpc when the project standard is `Req`.
- Converting external input to atoms (`String.to_atom` on user data) — atoms never GC;
  boundaries keep string keys, internals use atoms deliberately.
- Reaching for Repo/Ecto from web modules instead of the context boundary ({{contexts_note}}).
- GenServer for what a plain module/Task/Agent covers; state where no process is needed.
- Phoenix-version idiom drift: {{phoenix_notes}} (e.g. v1.8: templates start with
  `<Layouts.app flash={@flash} ...>`; `current_scope` in the right live_session).

### Verification norm
Proven = `{{check_alias}}` green (format, credo{{dialyzer_note}}, tests) + exercising the
changed route/LiveView in dev.

### Dependency policy
Ask before adding (global rule); prefer what Phoenix/Elixir already ships (Req, Jason, Task).

### Interview add-ons
- Phoenix? Version + LiveView vs API-only? (infer: mix.exs deps)
- The done-gate alias? (infer: mix.exs aliases — precommit/check/ci)
- Dialyzer in use? (infer: :dialyxir dep, PLT files in CI)
- Umbrella or single app? (infer: apps/ directory)
