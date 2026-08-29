## Stack notes — Node backend / API

### Toolchain & commands
- Runtime {{node_runtime}}, framework {{be_framework}}; boot locally: `{{boot_cmd}}`.
  DB/ORM: {{orm}}; migrations: `{{migrate_cmd}}` (never hand-edit applied migrations).

### Agent failure modes
- Floating promises — every async call is awaited or explicitly detached with a reason;
  an unhandled rejection in a handler is an outage, not a warning.
- Blocking the event loop in handlers (sync fs/crypto/zlib, heavy JSON) — offload or stream.
- Validating nothing at the route boundary — external input parses through {{validator}}
  before it touches logic; internal types stay trusted.
- Reading process.env ad hoc at import time instead of the config module ({{config_module}}).
- Secrets or PII into logs and error responses; stack traces to clients.
- ORM N+1s in list endpoints — check the query count, not just the result.

### Verification norm
Proven = typecheck + tests + booting the server and hitting the changed route with a real
request (status, shape, and an error-path probe). {{serverless_note}}

### Interview add-ons
- Framework + runtime? (infer: deps, engines)
- Validation library at boundaries? (infer: zod/valibot/joi in deps)
- ORM + migration workflow? (infer: prisma/drizzle/knex configs)
- Deploy target — long-lived server or serverless? (infer: vercel/netlify/docker configs;
  serverless adds cold-start + statelessness rules)
