## Stack notes — Infra / DevOps

### Toolchain & commands
- IaC: {{iac_tool}}, state in {{state_backend}}; environments: {{environments}}.
  Plan: `{{plan_cmd}}` (free, always). Apply: `{{apply_cmd}}` — **never apply, deploy, or
  destroy without explicit approval for that run**; plan output is what gets approved.
- Containers: `{{container_build_cmd}}`; lint: {{lint_tools}} (tflint/hadolint/actionlint).

### Agent failure modes
- Reading a plan as "green" without reading the verbs — a replace hiding among updates
  is an outage; call out every destroy/replace explicitly.
- Fixing drift by re-creating what should be imported ({{import_policy}}).
- Secrets into tfvars/compose/values files — they come from {{secret_source}}, never
  literals in the repo.
- Unpinned anything: :latest images, floating action versions, unpinned providers —
  pins are the repo's whole value proposition.
- Editing generated/lock files ({{iac_lock}}) by hand; state surgery without a backup.

### Verification norm
Proven = lint + plan (with the diff summary quoted in the PR/handoff) + container builds
locally; applied changes verified against the live environment's health checks.

### Interview add-ons
- IaC tool + state backend + who may apply? (infer: backend blocks, CI)
- Environments layout + promotion order? (infer: dirs, workspaces, kustomize overlays)
- Secret source? (infer: sops/vault/sealed-secrets references)
- Image registry + pinning policy? (infer: manifests)
