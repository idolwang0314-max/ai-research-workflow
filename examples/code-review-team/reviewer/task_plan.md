# reviewer — Task Plan

## Mission

Read the PR diff for PR#123. Identify bugs, smells, risks, and nits.
Report findings; do not modify code.

## Scope

**In scope:**
- Read code in the PR's changed files
- Read related files for context (caller / callee paths)
- Append findings to `findings.md`

**Out of scope:**
- Editing any source files
- Deciding what gets fixed (team-lead decides)

## Phases

### Phase 0: Review

- [ ] Read PR description
- [ ] Read each changed file
- [ ] For each issue, append a bullet to `findings.md`:
  - Format: `- [TAG] file.py:line — short description`
  - Tags: `bug` (will break in production), `smell` (code quality),
    `risk` (subtle), `nit` (style)
- [ ] Append final summary line: `Total: N bugs, M smells, K risks, J nits`
- [ ] Mark `phase=0 done` in `progress.md`
