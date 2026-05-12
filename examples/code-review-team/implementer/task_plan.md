# implementer — Task Plan

## Mission

Apply the accepted fixes from `reviewer/findings.md` (filtered by team-lead)
to the PR. One commit per fix.

## Scope

**In scope:**
- Edit the source files referenced in accepted findings
- Make commits with messages tied to the finding tag

**Out of scope:**
- Adding new features beyond the fix
- Renaming or restructuring files unrelated to the fix
- Running CI / merging (team-lead's job)

## Phases

### Phase 1: Fix

For each accepted finding:
- [ ] Read the relevant file at the specified line
- [ ] Apply the minimum change that resolves the issue
- [ ] Commit: `fix: <tag> <short description> (re: reviewer finding #N)`
- [ ] Append entry to `progress.md`

When all accepted findings processed:
- [ ] Mark `phase=1 done` in `progress.md`
