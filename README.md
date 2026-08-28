# skills

Agent skills by [farmisen](https://github.com/farmisen), usable in Claude Code, Codex CLI,
and any agent that speaks the [Agent Skills](https://agentskills.io) standard.

## Skills

### bootstrap-project

Generates - or migrates - a project's coding-agent configuration. It observes the repo
first (stack, commands, existing config), interviews you only about what it cannot
infer (purpose, tracker, git gates, MCP, dev journal), then renders a lean `AGENTS.md`
(canonical, read natively by Codex and 25+ tools) plus a one-line `CLAUDE.md` import
shim for Claude Code, and optional `.mcp.json` / `.codex` config with env-var-only
secrets.

Existing `AGENTS.md`/`CLAUDE.md`? It runs a **migration**: every block is triaged
(keep / retire-with-reason / personal / ask), kept factual claims are re-verified
against the repo so stale statements do not survive, and nothing is deleted until you
approve the new file and the retire list side by side. It also detects a legacy
single-file `docs/dev-journal.md` and can split it into sustainable monthly files
under `docs/dev-journal/`.

## Install

Claude Code (plugin, auto-updating):

    /plugin marketplace add farmisen/skills
    /plugin install farmisen-skills@farmisen

Any agent via [skills.sh](https://skills.sh) (copies/symlinks, editable):

    npx skills add farmisen/skills

Codex CLI: the skills.sh install covers it (skills land in `~/.agents/skills`), or
symlink `skills/bootstrap-project` there manually. A `.codex-plugin` manifest is
included for Codex's plugin system as it matures.

## License

MIT
