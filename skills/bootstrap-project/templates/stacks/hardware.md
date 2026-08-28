## Stack notes — Hardware (KiCad / OpenSCAD)

- Source of truth: {{source_files}}. Generated outputs (gerbers, STLs, BOMs) are build artifacts — regenerate, don't hand-edit.
- OpenSCAD: parameters at the top of the file; keep printable tolerances as named constants.
- KiCad: never renumber references on existing boards; DRC clean before any export.
