## Stack notes — Go

### Toolchain & pinning
- Go version from `go.mod`'s directive; lint config in `{{lint_config}}` (golangci-lint).

### Commands
- Single test: `go test ./{{pkg}} -run '{{TestName}}'`. Lint: `{{lint_cmd}}`.
  Generate: `go generate ./...` when {{generate_areas}} change.

### Agent failure modes
- Discarding errors (`_ =`, or checking and doing nothing) — every error is handled,
  wrapped with context (`fmt.Errorf("...: %w", err)`), or explicitly justified.
- Goroutines without a termination story — pass ctx, respect cancellation; leaks are bugs.
- `go get` without `go mod tidy` in the same change (dirty go.sum).
- Wide interfaces or `any` where a two-method interface at the consumer fits.
- init() side effects and package-level mutable state.

### Verification norm
Proven = build + vet + {{lint_cmd}} + tests; `-race` on anything touching concurrency.

### Dependency policy
Ask before adding (global rule); the stdlib bar is higher in Go — prefer it strongly;
`go get` + `go mod tidy` together.

### Interview add-ons
- golangci-lint config + strictness? (infer: .golangci.yml, CI)
- go:generate steps and their generated files? (infer: grep go:generate)
- Race-sensitive areas that must always run -race? (ask if concurrency-heavy)
