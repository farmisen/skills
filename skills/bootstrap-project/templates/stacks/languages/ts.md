## Stack notes — TypeScript

### Toolchain & pinning
- Package manager: `{{pkg_manager}}` (from the lockfile — never mix lockfiles; a second
  lockfile appearing in a diff is a bug). Node version: `{{node_pin}}` (.nvmrc / engines).
- tsconfig strictness is the contract: never weaken a flag (`strict`, `noUncheckedIndexedAccess`,
  …) to make an error go away; fix the type or discuss.

### Commands
- Single test: `{{single_test_cmd}}` (e.g. `vitest run path -t "name"`). Watch: `{{watch_cmd}}`.
- Codegen (if any): `{{codegen_cmd}}` — regenerate, never hand-edit generated files: {{generated_paths}}.

### Agent failure modes
- Treating a passing lint as a passing typecheck — run `{{typecheck_cmd}}`; they catch different things.
- `as any` / `@ts-expect-error` to silence an error instead of fixing the type; a suppression
  needs a comment saying why, or it does not land.
- `import { X }` where `import type { X }` is meant — breaks tree-shaking/isolatedModules builds.
- Adding a dependency without its types, or duplicating one that exists under another name
  (date libs, fetch wrappers, utility belts). Check first.
- Editing files under {{generated_paths}} or dist/ output instead of the source.

### Verification norm
Proven = typecheck + affected tests + a build of the touched package. "It compiles in the
editor" is none of these.

### Dependency policy
Ask before adding (global rule). When approved: exact-pin or lockfile-only per repo habit;
prefer the platform (fetch, URL, crypto) over a package when Node/browser now covers it.

### Interview add-ons
- Package manager + Node pin? (infer: lockfile name, .nvmrc, engines field)
- Test runner and its single-test invocation? (infer: devDependencies, scripts)
- Any codegen and its generated paths? (infer: openapi/graphql/prisma configs, *.gen.* files)
- Monorepo per-package command shape? (infer: workspaces/turbo.json — pairs with shapes/monorepo)
