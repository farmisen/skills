## Stack notes — shape: library / package

- The public API is the product: exports, types/signatures, error shapes, and default
  behavior are contracts — any change to them is semver-relevant and gets called out as
  such ({{semver_tooling}}: changesets/release-please/manual changelog).
- Keep the dependency tree light: every dependency here becomes every consumer's
  dependency; heavy or duplicative additions need a strong case.
- Internal types leak: what a public signature references becomes public — wrap or
  re-export deliberately.
- Silent behavior changes are the worst break: changing a default is a major, not a patch.

### Verification norm
Proven = build + tests + an API-surface check ({{api_diff_tool}} or reviewing the public
declaration diff) + the changelog entry written.

### Interview add-ons
- Versioning/changelog tooling? (infer: .changeset/, release-please config)
- Supported version matrix (runtimes/frameworks)? (infer: CI matrix, peerDeps/engines)
- API-diff tooling? (infer: api-extractor, cargo-semver-checks, apidiff)
