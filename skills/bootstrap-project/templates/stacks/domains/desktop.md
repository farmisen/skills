## Stack notes — Desktop

### Toolchain & commands
- Framework {{desktop_framework}} (Tauri/Electron/native); targets {{target_oses}},
  primary {{primary_os}}. Run: `{{run_cmd}}`; package: `{{package_cmd}}`.

### Agent failure modes
- Process-boundary violations: Electron — Node APIs or secrets in the renderer, IPC
  handlers accepting unvalidated payloads; Tauri — over-broad allowlist/capabilities.
- Blocking the UI thread; long work belongs in the main/worker side with progress events.
- One-OS assumptions: path separators, config/cache dirs ({{config_dirs}}), menu/shortcut
  conventions, case-sensitive filesystems.
- Treating signing/notarization/auto-update config as editable detail — a wrong change
  ships an app that silently stops updating ({{update_mechanism}}).

### Verification norm
Proven = launch and exercise the change on {{primary_os}} + cross-platform build where CI
covers it; packaging step run when the change touches config/assets that ship.

### Interview add-ons
- Framework? (infer: tauri.conf.json, electron in deps)
- Target OSes + primary dev OS? (infer: CI matrix)
- Signing/notarization + auto-update mechanism? (infer: configs; sets the no-casual-edits list)
