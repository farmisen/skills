## Stack notes — Agent skills / plugins (Claude Code, Codex, Agent Skills spec)

### Toolchain & layout
- Skills follow the Agent Skills spec (agentskills.io): `skills/<name>/SKILL.md`,
  frontmatter `name` + `description`; extras in `templates/`, `references/`, `scripts/`.
- Target harnesses: {{target_harnesses}}. Distribution: {{distribution}} (plugin
  marketplace / skills.sh / vendored copies — manifests: {{manifest_files}}).

### Agent failure modes
- Treating the frontmatter description as a label — it is the trigger API: it must say
  WHEN to fire (and implicitly when not to); a vague description means the skill never
  triggers or always does. Rewriting it is a behavior change, version it as one.
- Bloating SKILL.md with reference material — the body is loaded on every invocation;
  bulk goes to `references/`/`templates/`, loaded on demand (progressive disclosure).
- Harness-specific residue breaking portability: absolute paths, one CLI's tool names,
  personal conventions baked into templates ({{portability_rules}}).
- Editing a vendored downstream copy instead of the canonical upstream — provenance
  says where fixes land; downstream edits are clobbered on sync.
- Shipping a change without testing BOTH trigger directions: fires on its use cases,
  stays silent on near-misses.
- Forgetting the manifest version bump (plugin.json files) when skill behavior changes.

### Verification norm
Proven = spec-valid frontmatter + a live run of the skill in {{target_harnesses}} on a
real case + a trigger check (one should-fire, one shouldn't) + manifests version-bumped
when behavior changed. {{eval_note}}

### Dependency policy
Skills are prose-first: a script dependency needs a reason and pinned invocation; no
network fetches at skill-load time.

### Interview add-ons
- Target harnesses + distribution channels? (infer: .claude-plugin/, .codex-plugin/,
  marketplace.json, skills.sh conventions)
- Eval/test harness in use (claude plugin eval, skill-creator evals)? (infer: eval configs)
- Vendoring relationships — who copies from this repo? (infer: PROVENANCE files, README)
