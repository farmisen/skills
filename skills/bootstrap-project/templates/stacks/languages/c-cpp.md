## Stack notes — C/C++

### Toolchain & pinning
- {{c_or_cpp}} {{standard}}; compiler {{compiler_pin}}; build system {{build_system}}
  (out-of-source builds in {{build_dir}}). The warning flags are the contract — never fix
  a warning by weakening `-W` flags.

### Commands
- Configure/build: `{{build_cmd}}`. Single test: `{{single_test_cmd}}` (e.g. `ctest -R name`).
- Sanitizer builds: `{{sanitizer_cmd}}` (ASan/UBSan{{tsan_note}}).

### Agent failure modes
- Ownership drift: raw `new`/`malloc` where the codebase convention is {{ownership_convention}}
  (RAII/smart pointers/arena) — follow the convention, not habit.
- "Works locally" UB: signed overflow, strict-aliasing casts, uninitialized reads — the
  sanitizer run is the arbiter, not a clean exit.
- Include creep: adding headers to headers (use forward declarations); cyclic includes.
- Editing generated build artifacts instead of {{build_system}} sources.
- Fixing a flaky test by loosening it instead of finding the race/UB underneath.

### Verification norm
Proven = clean build with warnings-as-errors + tests + a sanitizer run for anything touching
memory, lifetimes, or concurrency.

### Dependency policy
Ask before adding (global rule); deps arrive via {{cpp_pkg}} (vcpkg/conan/FetchContent/vendored)
per repo convention — never a new system-library assumption undocumented.

### Interview add-ons
- C or C++, standard, compiler pin? (infer: CMakeLists/Makefile flags, CI)
- Build system + build dir convention? (infer: CMakePresets.json, Makefile, meson.build)
- Ownership/memory convention? (infer: smart-pointer prevalence, allocator wrappers)
- Sanitizer targets in CI? (infer: CI config; if none, flag it as a gap)
- Package/vendoring mechanism? (infer: vcpkg.json, conanfile, third_party/)
