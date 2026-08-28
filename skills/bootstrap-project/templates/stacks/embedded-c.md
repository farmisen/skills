## Stack notes — Embedded C/C++

- Target/toolchain: {{toolchain}}. Build via `{{build_cmd}}`; flash via `{{flash_cmd}}`.
- Never touch flashing/deploy targets without explicit approval — hardware may be connected.
- Memory constraints matter: no dynamic allocation in {{no_alloc_zones}}; check map file on size regressions.
