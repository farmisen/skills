## Stack notes — Rails (specializes ruby + web)

### Toolchain & commands
- Rails {{rails_version}}; frontend mode {{frontend_mode}} (Hotwire/importmap/jsbundling).
- Migrations: `bin/rails db:migrate` — schema.rb is generated, never hand-edited.
  Jobs: {{job_backend}}. Boot: `bin/dev`.

### Agent failure modes
- N+1s: any association touched in a view/serializer gets includes/preload — check the
  log's query count on list pages.
- Business logic drifting into controllers or views — it belongs in {{logic_home}}
  (models/services/concerns per this repo's convention).
- Model callbacks for request-specific behavior — callbacks run everywhere (console,
  jobs, seeds); keep request concerns in the request path.
- Skipping strong params, or permitting `!` wholesale.
- Fixing a migration by editing an applied one — new migration, always.
- Fat fixtures/factories that hide coupling; system tests asserting HTML details instead
  of behavior.

### Verification norm
Proven = affected specs + a system/request test through the changed flow + migrations
that run up AND down cleanly (or are explicitly irreversible with a reason).

### Interview add-ons
- Rails version + frontend mode? (infer: Gemfile, importmap/esbuild configs)
- Where does business logic live — services, concerns, fat models? (infer: app/ layout)
- Job backend + queue conventions? (infer: sidekiq/solid_queue/resque in Gemfile)
- Multi-DB or single? (infer: database.yml)
