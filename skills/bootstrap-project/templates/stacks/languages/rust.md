## Stack notes — Rust

### Toolchain & pinning
- Toolchain pinned in `rust-toolchain.toml` ({{toolchain}}); edition {{edition}};
  workspace members: {{workspace_layout}}. Build with the pinned toolchain, never PATH's.

### Commands
- Single test: `cargo test -p {{crate}} {{test_name}}`. Lint: `cargo clippy --all-targets`
  (warnings are errors here: {{clippy_policy}}). Format: `cargo fmt`.

### Agent failure modes
- `.unwrap()` / `.expect()` outside tests — propagate with `?` or handle; panics are bugs
  in library and long-running code.
- Silencing clippy with `#[allow]` instead of fixing; an allow needs a comment saying why.
- `unsafe` without a `// SAFETY:` comment stating the invariant ({{unsafe_policy}}).
- `cargo add` with default features when the crate is used in a no_std / feature-disciplined
  context — check the feature set the workspace expects.
- Resolving Cargo.lock conflicts by regenerating blindly, silently bumping transitive deps.

### Verification norm
Proven = `cargo fmt --check` + clippy clean + tests, built for {{targets}}.

### Dependency policy
Ask before adding (global rule); when approved, minimal features
(`--no-default-features` where the workspace does), and glance at the transitive tree —
a small crate pulling tokio needs a reason.

### Interview add-ons
- Workspace layout + which crate is the entry point? (infer: Cargo.toml [workspace])
- Toolchain pin / MSRV? (infer: rust-toolchain.toml, package.rust-version)
- Unsafe policy? (infer: #![forbid(unsafe_code)], workspace lints table)
- Async runtime, if any? (infer: tokio/async-std/embassy in the dep tree)
