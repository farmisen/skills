---
name: bootstrap-project
description: Bootstrap a project's coding-agent configuration — AGENTS.md (canonical), a CLAUDE.md shim, and optional .mcp.json / .codex config — by interviewing the user about the project's perimeter (purpose, stack, tracker, commands, git etiquette), then rendering from the bundled templates. Use for a new project, "set up agents config", "bootstrap this repo", or when a repo has no AGENTS.md/CLAUDE.md.
---

# Bootstrap a project's agent configuration

Produce a small, correct agent configuration for the current repository. AGENTS.md is the
canonical instruction file (read natively by Codex and 25+ tools); CLAUDE.md is a one-line
import shim plus Claude-only deltas. Never generate a bloated config: every line must pass
the test "would removing this cause the agent to make a mistake?"

## Step 1 — Observe before asking

Inspect the repo first so the interview only covers what can't be inferred:

- Existing config: `AGENTS.md`, `CLAUDE.md`, `CLAUDE.local.md`, `.claude/`, `.codex/`,
  `.mcp.json`. If AGENTS.md or CLAUDE.md already exists, this run is a **migration**
  (see Migration mode below). Never silently overwrite.
- Stack signals: `package.json` (+ package manager from lockfile), `Cargo.toml`,
  `mix.exs`, `pyproject.toml`, `go.mod`, `CMakeLists.txt`, `Makefile`, `*.ino`,
  `platformio.ini`, `*.uproject`, `ProjectSettings/` (Unity), `*.kicad_pro`, `*.scad`.
- Command candidates: scripts in `package.json`, Make targets, `mix` aliases, cargo
  defaults — collect the likely build / test / lint / format commands to confirm rather
  than ask cold.
- Layout: monorepo markers (workspaces, `turbo.json`, `apps/`, `packages/`), CI config,
  existing docs (`README`, `docs/`, `SPEC`).
- Dev journal: `docs/dev-journal/` (current convention) or a single `docs/dev-journal.md`
  (legacy — flag it for migration in Step 3).

## Step 2 — Interview (one batch, only the gaps)

Ask in ONE batch (use the question tool if available, otherwise a single message).
Skip anything Step 1 already answered; present inferred values for confirmation instead
of asking open questions. Cover:

1. **Purpose** — one sentence: what is this project, who is it for?
2. **Stack(s)** — confirm the inferred stack along three composable axes (see
   `templates/stacks/README.md`; multiple allowed per axis):
   - language(s): `ts`, `python`, `rust`, `go`, `c-cpp`, `elixir`, `ruby`
   - domain(s): `web-frontend`, `web-node`, `fastapi`, `rails`, `mobile`, `desktop`,
     `browser-extension`, `embedded`, `game`, `shader`, `hardware`, `infra-devops`,
     `data-ml`, `agent-skills`, `mcp-server`
   - shape: `application`, `library`, or `cli` (monorepo is auto-detected, not asked)
   Example: an encrypted pager = rust + embedded + hardware; a Next.js app =
   ts + web-frontend + web-node. `other` is allowed on any axis.
3. **Commands** — confirm build / test (all + single test) / lint / format / typecheck.
   Only commands that actually exist; no aspirational ones.
4. **Tracker** — Linear / GitHub Issues / none / other. If Linear: team or project key.
   Where does "status" live (issues vs a doc)?
5. **Git etiquette** — which gates apply (this maps to the Approval gates template
   section): ask-before-commit? ask-before-push? never-merge-PRs? branch naming?
   Anything protected (no force-push targets)?
6. **MCP servers** — needed for this project? (e.g. linear). Secrets are ALWAYS
   `${ENV_VAR}` references — never accept a literal token into a config file, even if
   the user pastes one; tell them which env var to set instead.
7. **Dev journal** — skip if Step 1 found one. Otherwise ask: keep a dev journal?
   (Records per-work-item entries: what changed, decisions, failures, follow-ups —
   valuable for post-mortems and handoffs; costs a small write per completed item.)
8. **Anything the agent can't guess** — deploy quirks, directories to leave alone,
   spec file location, code-style rules not enforced by tooling. Do NOT add a code-style
   section: formatter/linter-enforced rules stay out; only unlintable rules earn a
   bullet in the stack notes.

## Step 2b — Stack add-ons (second and final question batch)

Read the chosen templates' **Interview add-ons** sections (`templates/stacks/<axis>/<name>.md`).
Infer every default the add-on names (each question says where to look), then ask ONE
batch containing only what could not be inferred plus confirmations of low-confidence
inferences. Two batches total is the interview's hard ceiling — never ask add-ons one
by one, and skip the batch entirely when everything was inferable.

## Step 3 — Render

From `templates/` in this skill directory:

1. `AGENTS.md` from `templates/AGENTS.base.md` + the chosen stack templates, composed in
   language → domain → shape order, placeholders filled from the interview. Target under
   ~120 lines regardless of how many templates were composed: merge overlapping sections
   (one Verification norm, one Dependency policy — never one per template), keep only
   interview-confirmed or repo-verified lines, drop empty sections and never render an
   Interview add-ons section. No placeholder residue, no "N/A" sections.
2. `CLAUDE.md` from `templates/CLAUDE.md` — the `@AGENTS.md` import plus only
   Claude-specific deltas (usually none at bootstrap).
3. If MCP servers were requested: `.mcp.json` from `templates/mcp.json` (env-var
   references only) and, if the user also uses Codex here, `.codex/config.toml` from
   `templates/codex-config.toml`.
4. **Dev journal** (if wanted or already present):
   - Convention: `docs/dev-journal/` with one file per month (`YYYY-MM.md`), newest
     entry first inside each file, plus `docs/dev-journal/README.md` describing the
     entry format (copy `templates/dev-journal-README.md`). A single ever-growing
     journal file is explicitly not the convention.
   - Add the journal rule to AGENTS.md (the template's Dev journal section): update
     the current month's file for each completed work item.
   - **Migration** — when Step 1 found a legacy `docs/dev-journal.md`: split it by its
     `## YYYY-MM-DD` entry headings into `docs/dev-journal/YYYY-MM.md` files (entries
     stay newest-first; an entry's full text moves untouched); move any preamble/entry-
     format prose into the README. Verify before deleting: every entry heading from the
     old file appears in exactly one new file and total entry count matches; show the
     user the file list, then `git rm docs/dev-journal.md` in the same change.
5. Recommend (do not install) the matching LSP/code-intelligence plugins for the chosen
   stacks — e.g. typescript-lsp, rust-analyzer-lsp, clangd-lsp from the official
   marketplace — and note them at the end of your reply, not inside AGENTS.md.

## Migration mode (AGENTS.md or CLAUDE.md already exists)

Re-found the file — never append blindly, and never dump existing content into a
.local/override file. Why not: `AGENTS.override.md` REPLACES AGENTS.md for Codex at
that level (parking content there shadows the new file), and `CLAUDE.local.md` is
personal and gitignored (project truth parked there is invisible to collaborators and
other machines). "Everything extra in a hidden file" relocates bloat instead of
removing it.

1. Read the existing file(s) in full. Run the normal interview, but confirm inferred
   answers against what the file claims — the file may be stale; the repo wins.
2. Triage every block of the existing content:
   - **KEEP** — still-true, project-specific, not tool-enforced (commands, critical
     rules, layout, tracker, doc precedence, gotchas, journal rule) → merge into the
     matching template section, preserving the original wording where it is good.
     **The repo wins over kept content too**: for every KEEP block that makes a
     factual claim about repo state — what exists or is built, what a command runs,
     what a check covers, what is "not started" — verify the claim against the repo
     (run the command, look for the files) before carrying it. Repair what drifted;
     flag what you cannot verify for user confirmation. Stale claims are the most
     common defect that survives migration.
   - **RETIRE** — generic boilerplate, formatter/linter-enforced style rules,
     framework dogma, process prose now covered by shared skills → drop, and list
     every dropped block with a one-line reason.
   - **PERSONAL** — genuinely per-developer preference (rare) → offer
     `CLAUDE.local.md` (Claude) or a gitignored `AGENTS.override.md` (Codex,
     remembering it replaces rather than adds) only after the user confirms.
   - **UNSURE** → ask; never guess a deletion.
3. CLAUDE.md-only repos: keep-content moves into the new AGENTS.md; CLAUDE.md becomes
   the `@AGENTS.md` shim plus any Claude-only deltas found in triage.
4. Present the new file(s) AND the retired-blocks list side by side. Delete nothing
   until approved. Git history preserves tracked files; copy an untracked file to
   `<name>.pre-bootstrap` before replacing it.

## Step 4 — Verify

- Re-read every generated file. No unresolved `{{placeholders}}`, no secrets, no
  commands that don't exist in the repo, and (migration) no carried claim you did not
  verify or flag.
- Confirm CLAUDE.md's first line is exactly `@AGENTS.md`.
- Show the user the generated AGENTS.md in full and ask for one round of corrections
  before finishing. Remind them: commit these files; personal-only additions go in
  `CLAUDE.local.md` (Claude) or a gitignored `AGENTS.override.md` (Codex).
