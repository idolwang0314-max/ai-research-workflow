# {{AGENT_NAME}} — Task Plan

## Mission

One paragraph describing what this agent owns within the team.

## Scope

**In scope:**
- {{specific capability 1}}
- {{specific capability 2}}

**Out of scope:**
- {{things this agent must NOT do — usually planning, scope changes}}

## Working Files

- Owns: `progress.md`, `findings.md`, files under `{{agent_name}}/work/`
- Reads: `../task_plan.md`, `../decisions.md`, peer agents' `findings.md`
- Never modifies: any file outside its own subdirectory, except by explicit team-lead instruction

## Phases

### Phase 0: Bootstrap

- [ ] Read team-level `task_plan.md`
- [ ] Read this `task_plan.md`
- [ ] Initialize `progress.md` with first entry
- [ ] Report `phase=0 done` in `progress.md`

### Phase 1: {{specific phase title}}

- [ ] {{task 1}}
- [ ] {{task 2}}
- [ ] Update `findings.md` with new beliefs
- [ ] Report `phase=1 done` in `progress.md`

### Phase 2: ...

## Failure Protocol

- On failure: write a `progress.md` entry with `status=failed` and the error
- After 3 consecutive failures on the same task: stop and wait for team-lead

## Output Format

- Progress entries: `## YYYY-MM-DD HH:MM — <one-line summary>`
- Findings entries: bullet under the relevant section heading
