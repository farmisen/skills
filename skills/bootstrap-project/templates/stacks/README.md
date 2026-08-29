# Stack templates — taxonomy and schema

Three composable axes; a project picks one or more from each as it applies:

- **languages/** — ts, python, rust, go, c-cpp, elixir, ruby
- **domains/**  — web-frontend, web-node, fastapi, rails, mobile, desktop,
  browser-extension, embedded, game, shader, hardware, infra-devops, data-ml,
  agent-skills, mcp-server
- **shapes/**   — application, library, cli (+ `monorepo.md`, an auto-detected modifier)

Examples: an encrypted pager = rust + embedded + hardware · a Next.js app =
ts + web-frontend + web-node · a Rust secrets CLI = rust + cli.
Framework files (fastapi, rails) are deliberate specializations of their language;
when picked, they subsume the generic web-node/web-frontend questions they answer.

## Per-file schema (every template follows it)

1. **Toolchain & pinning** — what is pinned, where, and how an agent respects it.
2. **Commands** — nuances beyond the generic table: single-test, watch, codegen.
3. **Agent failure modes** — things agents habitually get wrong in this stack.
   The highest-value section; every line must name a real mistake, not a platitude.
4. **Verification norm** — what "proven" means here, concretely.
5. **Dependency policy** — how deps are added and vetted in this stack.
6. **Interview add-ons** — 2–5 stack-specific questions, each with how to infer a
   default from the repo. Consumed by the skill's interview; NEVER rendered.

## Render rules (bloat guardrail)

Depth lives in the templates and the interview — not in the generated file. Only
lines confirmed by the interview or verified against the repo reach AGENTS.md;
empty or unconfirmed sections are dropped, never stubbed. The generated file keeps
its ~120-line budget regardless of how many templates were composed: when axes
overlap (ts + web-node), merge, don't repeat. Every carried line must still pass:
"would removing this cause the agent to make a mistake?"
