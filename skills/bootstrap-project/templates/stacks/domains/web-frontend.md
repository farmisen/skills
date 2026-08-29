## Stack notes — Web frontend

### Toolchain & commands
- Framework: {{fe_framework}}; rendering model: {{rendering_model}} (SSR/SPA/islands).
- Dev server: `{{dev_cmd}}`. E2E: `{{e2e_cmd}}` ({{e2e_tool}}). Components live in
  {{component_source}} — reuse before writing new ones.

### Agent failure modes
- Fixing visual issues by reading CSS instead of looking — verify in a browser
  (screenshot or e2e), every time.
- Importing server-only code into client components — secrets and heavy deps end up in
  the bundle; respect the {{server_client_boundary}} markers.
- Module-scope state under SSR: it leaks across requests.
- Hydration mismatches from non-deterministic render (dates, random, locale) — compute
  them where the framework says to.
- Accessibility regressions while restyling: removing focus outlines, div-as-button,
  images without alt.

### Verification norm
Proven = typecheck + unit tests + the changed flow exercised in a real browser
({{e2e_tool}} or manual with evidence). {{bundle_budget_line}}

### Interview add-ons
- Framework + rendering model? (infer: next/nuxt/svelte/vite config)
- E2E tool + when it must run? (infer: playwright/cypress config)
- Component source of truth (design system, storybook)? (infer: storybook config, packages)
- Bundle-size budget worth enforcing? (state it, or the line drops)
