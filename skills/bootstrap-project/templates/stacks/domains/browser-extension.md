## Stack notes — Browser extension (MV3)

### Toolchain & commands
- Browsers: {{browsers}}; manifest at {{manifest_path}}; build/load: `{{build_cmd}}`,
  then load unpacked from {{unpacked_dir}}.

### Agent failure modes
- State in service-worker globals — MV3 workers are evicted at will; persist via
  chrome.storage / alarms, design event-driven.
- Widening host_permissions or adding permissions when a narrower one works — every
  broadening is store-review-sensitive and user-visible; treat the permissions diff as
  an API break.
- DOM work in the background context, or messaging without validating sender/origin.
- Inline scripts / eval that CSP rejects; remote code (store policy violation).
- Shipping Chrome-only APIs when {{browsers}} includes Firefox/Safari (namespace,
  manifest divergences).

### Verification norm
Proven = load unpacked in {{browsers}} and exercise the change (content + background +
popup paths as touched); any manifest change shows its permissions diff explicitly.

### Interview add-ons
- Browsers targeted? (infer: manifest keys, webextension-polyfill)
- Store publication cadence / review constraints worth encoding?
- Content-script injection strategy (declarative vs programmatic)? (infer: manifest)
