## Stack notes — shape: application

- Users carry state: anything persisted (files, DB rows, prefs, caches) needs a
  compatibility story when its shape changes — migrate, don't strand ({{data_compat}}).
- Feature flags: {{flags_convention}} — user-visible changes ride the repo's flag
  mechanism when one exists; no half-shipped UI on the default path.
- "Works" is observed where users are: {{prod_verification}} (logs/metrics/error tracker)
  is part of done for risky changes, not an ops afterthought.

### Interview add-ons
- Persisted user state whose compat matters? (infer: migrations, schema files, save formats)
- Feature-flag mechanism? (infer: flag libs/config)
- How is prod verified after a change ships? (infer: sentry/datadog refs)
