## Stack notes — MCP server

### Toolchain & layout
- Transport(s): {{transports}} (stdio / streamable HTTP); SDK {{mcp_sdk}}. Run locally:
  `{{run_cmd}}`; exercise with {{test_client}} (MCP inspector / harness) before any
  real-client test.

### Agent failure modes
- **stdout is the protocol** in a stdio server — a stray print/console.log corrupts the
  stream and breaks the client mysteriously; all logging goes to stderr ({{log_setup}}).
- Tool descriptions written for humans — the model is the caller: say when to use the
  tool, when not to, and what each parameter means; loose schemas (everything optional,
  untyped strings) produce garbage calls. The schema is the API contract — breaking
  changes need versioning; clients cache tool lists.
- Unbounded results: dumping a full table/mailbox/file into a tool result floods the
  caller's context — cap, paginate, and summarize ({{result_limits}}).
- Trusting inputs: an MCP server is a privilege boundary — path traversal, injection
  into shell/SQL, and confused-deputy calls are the threat model; validate every
  parameter server-side, least-privilege every credential.
- Secrets in tool results or error messages; config via CLI args instead of env
  ({{secret_env_vars}} — never inline in client configs).
- Long operations without timeouts/cancellation handling — a hung tool hangs the agent.

### Verification norm
Proven = inspector session exercising each changed tool (happy + one malformed-input
probe) + a real-client run ({{consumer_clients}}: registration snippets for both Claude
Code and Codex kept working) + no stdout pollution in stdio mode.

### Dependency policy
Servers ship to other people's agent configs: keep the dep tree auditable; pin the MCP
SDK; no postinstall scripts.

### Interview add-ons
- Transport(s) + SDK? (infer: package.json/pyproject deps, server entry)
- Auth model — env token, OAuth, none? (infer: env reads, auth middleware)
- Result-size limits per tool? (state them, or the cap line names the gap)
- Consumer clients to keep registration docs for? (infer: README install sections)
