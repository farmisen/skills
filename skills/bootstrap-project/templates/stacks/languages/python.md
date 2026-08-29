## Stack notes — Python

### Toolchain & pinning
- Managed by `{{py_manager}}` (uv unless the repo says otherwise); interpreter pinned in
  `{{py_pin_file}}` (.python-version / requires-python). Run everything through it
  (`uv run pytest`) — never the bare interpreter or `pip install` into whatever env is active.

### Commands
- Single test: `{{single_test_cmd}}` (e.g. `uv run pytest path/test_x.py::test_name`).
- Types: `{{typecheck_cmd}}` ({{type_checker}}). Lint/format: `{{lint_cmd}}` (ruff).

### Agent failure modes
- Adding a dependency by editing pyproject.toml without syncing the lockfile — use
  `{{add_dep_cmd}}` so both move together.
- `# type: ignore` without an error code and a reason; same for `# noqa`.
- Tests that pass only in file order or via shared state — keep tests independent.
- Editing generated files ({{generated_paths}}: migrations, protobufs) instead of regenerating.
- Import-time side effects (config reads, client construction at module top level).

### Verification norm
Proven = ruff + {{type_checker}} + affected tests, all via `{{py_manager}} run`.

### Dependency policy
Ask before adding (global rule); prefer stdlib; when approved, `{{add_dep_cmd}}` with the
lockfile in the same change.

### Interview add-ons
- Package manager? (infer: uv.lock / poetry.lock / requirements*.txt)
- Type checker + strictness? (infer: pyproject [tool.*] sections, CI)
- Single-test invocation? (infer: pytest/unittest in deps, Makefile)
- Generated paths to never hand-edit? (infer: alembic/, *_pb2.py, migrations/)
