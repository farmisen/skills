## Stack notes — Embedded (language set by the language template: c-cpp, rust, …)

### Toolchain & pinning
- Target: `{{target_triple_or_chip}}`. Toolchain pinned in `{{toolchain_pin_file}}`
  (rust-toolchain.toml / arm-gcc version / PlatformIO platform pin) — never build with
  whatever is on PATH when a pin exists.
- Build: `{{build_cmd}}`. Flash: `{{flash_cmd}}` — **never flash, erase, or touch a debug
  probe without explicit approval**: hardware may be connected, mid-experiment, or holding
  state that a reflash destroys.

### Commands
- Host-side tests: `{{host_test_cmd}}`. On-target / emulator tests: `{{target_test_cmd}}`
  ({{emulator_notes}}). Size report: `{{size_cmd}}` — check it on any change touching
  {{size_sensitive_areas}}.

### Agent failure modes
- Assuming a hosted stdlib: no heap (or a fixed pool), no printf by default, no filesystem —
  check what {{runtime_model}} actually provides before reaching for it.
- Dynamic allocation or blocking delays inside {{no_alloc_or_blocking_zones}} (ISRs, RT loops).
- Debugging by adding prints on a constrained target instead of using {{debug_channel}}
  (RTT / defmt / logic analyzer) — and leaving the prints in.
- Trusting host-passing tests as proof: timing, endianness, alignment, and peripheral
  behavior only exist on target or emulator.
- Treating external input (UART/radio/I2C peer) as friendly — parsers here are attack
  surface and get malformed frames; length-check before use, always.

### Verification norm
Proven = builds for the pinned target + host tests + {{on_target_proof}} (on-target or
emulator run, bench measurement where the change affects timing/power). Map-file/size check
when a size budget exists: budget is {{size_budget}}.

### Dependency policy
Every new dependency is scrutinized for footprint and allocation behavior; vendored or
pinned per {{deps_convention}}. No network-fetching build steps in the firmware path.

### Interview add-ons
- Target chip/arch + where the toolchain is pinned? (infer: rust-toolchain.toml,
  platformio.ini, CMake toolchain file, Makefile CROSS_COMPILE)
- Runtime model: bare-metal, RTOS (which), or async (Embassy/RTIC)? (infer: deps/manifest)
- Flash/debug mechanism and is hardware routinely attached? (infer: probe-rs/openocd/esptool
  configs — sets how hard the never-flash gate is)
- On-target test story: emulator (QEMU/renode), HIL rig, or manual bench? (infer: CI config)
- Size/power budget worth enforcing? (state it, or drop the size lines)
