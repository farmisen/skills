# elixir-lsp

ElixirLS language server wiring for **Claude Code**: code intelligence
(go-to-definition, references, diagnostics, dialyzer where enabled) for Elixir
projects. Once installed it is ambient — Claude Code starts the server
automatically when it works with `.ex`/`.exs`/`.heex` files; there is nothing
to invoke.

> **Claude Code only.** Codex CLI has no LSP-plugin mechanism today; installing
> this does nothing for Codex sessions.

## Supported extensions
`.ex`, `.exs`, `.heex`

## Prerequisites

Install ElixirLS so the `elixir-ls` command is on PATH:

    brew install elixir-ls

(or a release from https://github.com/elixir-lsp/elixir-ls with its
`language_server.sh` symlinked as `elixir-ls`). ElixirLS runs on the project's
Elixir/OTP — keep those pinned per the repo (.tool-versions / mise).

## Install

    /plugin marketplace add farmisen/skills
    /plugin install elixir-lsp@farmisen

## First run & verification

- **The first session in a project is slow**: ElixirLS compiles the project and
  builds dialyzer PLTs — minutes on a large repo. That is warm-up, not a hang.
- ElixirLS writes a `.elixir_ls/` build directory into the project — add it to
  `.gitignore` if it is not already.
- Verify it's alive: in a Claude Code session inside an Elixir repo, ask for the
  definition or references of a project function — answers should come with
  precise locations without grep; introduce a syntax error and diagnostics
  should name it. If nothing happens, check `which elixir-ls` succeeds in the
  same shell you launch `claude` from.

## Troubleshooting

- Wrong-OTP crashes: the brew ElixirLS is built against a recent OTP; a project
  pinned to a much older Elixir/OTP may need a matching ElixirLS release instead.
- Dialyzer too slow on huge repos: disable it in ElixirLS config and keep the
  rest of the intelligence.
