# {{project_name}} Project Guide

{{one_sentence_purpose}}

## Critical rules

<!-- EXAMPLE gates. Keep only what the interview chose - teams differ on how much
     autonomy their agents get. These are the expensive-to-miss rules; keep the section
     short enough that every line stays load-bearing. -->
- **Never `git commit` or `git push` without explicit approval for this change.** Applies to all branches.
- **Never merge a pull request.** Opening the PR is where the work stops.
- **Never commit secrets or env files.** `.env*` stays untracked; never `git add` one.
{{extra_critical_rules}}

## Commands

<!-- Only commands verified to exist. Single-test invocation matters most to agents. -->
- Build: `{{build_cmd}}`
- Test (all): `{{test_cmd}}`
- Test (single): `{{test_single_cmd}}`
- Lint: `{{lint_cmd}}`
- Format: `{{format_cmd}}`
- Typecheck: `{{typecheck_cmd}}`

Run lint + tests before declaring any change done.

## Layout

{{layout_notes}}

## Tracker & status

<!-- e.g. "Status lives in Linear (team ENG), never in a file." or "GitHub Issues; `gh issue list`." -->
{{tracker_notes}}

## Dev journal

<!-- Keep only if the project keeps a journal; delete otherwise. -->
Update `docs/dev-journal/<current YYYY-MM>.md` for each completed work item (newest
entry first; entry format in `docs/dev-journal/README.md`). Never recreate a single
`docs/dev-journal.md` — monthly files only.

## Project-specific gotchas

<!-- Only things an agent cannot derive from the code: env quirks, directories to leave
     alone, where the spec lives, deploy caveats. Delete the section if empty. -->
{{gotchas}}
