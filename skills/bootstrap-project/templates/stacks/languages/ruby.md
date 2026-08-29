## Stack notes — Ruby

### Toolchain & pinning
- Ruby pinned in `.ruby-version`; gems via Bundler — every gem command runs through
  `bundle exec`. RuboCop config is the contract; {{rubocop_todo_note}}.

### Commands
- Single test: `{{single_test_cmd}}` ({{test_framework}}: `bundle exec rspec path:LINE` /
  `bin/rails test path:LINE`).

### Agent failure modes
- Running rake/rspec/rails without `bundle exec` (or the binstub) — wrong gem versions.
- `rescue Exception`, `rescue nil`, or rescuing broadly to make a spec pass — handle the
  specific error or let it raise.
- Monkey-patching core/third-party classes instead of a wrapper ({{monkeypatch_policy}}).
- Silencing RuboCop inline without a reason; editing .rubocop_todo.yml to hide new offenses.
- Adding a gem for what stdlib/ActiveSupport already provides; skipping the Gemfile.lock
  in the same change.

### Verification norm
Proven = RuboCop clean + affected specs green via `bundle exec`.

### Dependency policy
Ask before adding (global rule); check the gem's maintenance pulse first — the ecosystem
has long tails; Gemfile + lock move together.

### Interview add-ons
- RSpec or Minitest + single-test invocation? (infer: spec/ vs test/, Gemfile)
- Typing in use (Sorbet/RBS)? (infer: sorbet/, sig/, Gemfile)
- RuboCop strictness + todo-file policy? (infer: .rubocop.yml, .rubocop_todo.yml)
