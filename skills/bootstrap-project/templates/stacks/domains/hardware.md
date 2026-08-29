## Stack notes — Hardware (EDA / CAD: KiCad, OpenSCAD, code-CAD)

### Toolchain & pinning
- Tools: {{eda_cad_tools}} (e.g. KiCad {{kicad_version}} — pin the major version; a file
  saved by a newer minor can strand collaborators). Source of truth: {{source_files}};
  gerbers, STLs, STEP exports and BOMs are build artifacts — regenerate, never hand-edit.

### Agent failure modes
- Verifying CAD visually — a render can look right and be unmanufacturable; export and
  measure numerically ({{measure_cmd}}), every dimensional change.
- Renumbering references on an existing board — annotations are frozen once a board
  exists; new parts get new references.
- Unit confusion (mm vs mil vs inch) and implicit unit defaults in code-CAD; name the
  unit at every interface.
- Magic numbers in OpenSCAD/build123d — parameters at the top, tolerances as named
  constants ({{tolerance_constants}}), never inline.
- Ignoring a failing ERC/DRC "for now" — clean is the only passing state; a waiver is a
  documented rule exception, not a skipped check.
- Designing past the fab: {{fab_constraints}} (min trace/space, layer count, printer
  tolerances) bound every change.

### Verification norm
Proven = ERC/DRC clean ({{drc_cmd}}) + dimensions verified by export-and-measure + for
printed parts, tolerance-checked against {{print_process}}; measured values are labeled
measured, calculated ones calculated.

### Dependency policy
Footprints/symbols/models come from {{lib_convention}} (project-local libs preferred);
never point at a personal global library a collaborator lacks.

### Interview add-ons
- Tools + versions pinned where? (infer: *.kicad_pro, *.scad, pyproject with build123d)
- Fab constraints? (min trace/space, layers, print process + tolerances)
- ERC/DRC automation? (infer: kicad-cli in Makefile/CI)
- Library convention — project-local vs global? (infer: sym-lib-table, fp-lib-table)
