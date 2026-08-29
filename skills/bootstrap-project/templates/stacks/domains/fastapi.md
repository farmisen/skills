## Stack notes — FastAPI (specializes python + web backend)

### Toolchain & commands
- Boot: `{{boot_cmd}}` (uvicorn). Pydantic {{pydantic_v}}; DB: {{db_stack}}.
- API tests: pytest + httpx AsyncClient against the app, not the wire.

### Agent failure modes
- Sync blocking calls inside `async def` routes (requests, time.sleep, sync DB drivers) —
  they stall the event loop for everyone; use the async driver or `def` + threadpool.
- Mixing Pydantic v1 and v2 idioms ({{pydantic_v}} is the repo's — validator/field syntax differs).
- Returning ORM objects directly instead of response schemas — leaks columns as the model grows.
- App state in module scope instead of lifespan/dependency injection; Depends() re-doing
  per-request work that belongs in a singleton dependency.
- BackgroundTasks for work that needs durability — that's the queue's job ({{queue}}).

### Verification norm
Proven = boot + hit the changed endpoint (happy + one failure path) + `/docs` still renders
(schema generation is a compile check in disguise) + affected tests.

### Interview add-ons
- Pydantic version? (infer: pyproject pin)
- DB stack — sync or async driver, SQLAlchemy flavor, migration tool? (infer: deps, alembic/)
- Auth scheme? (infer: security deps, middleware)
- Durable-work queue, if any? (infer: celery/arq/dramatiq in deps)
