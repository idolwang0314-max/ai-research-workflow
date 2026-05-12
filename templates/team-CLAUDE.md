# {{TEAM_NAME}} — Team Operations Guide

> One-line description of what this team does.

## Team-Lead Control Plane

- team-lead = the main conversation, not a spawned agent
- team-lead owns user alignment, scope control, task decomposition, and phase transitions
- team-lead maintains: `task_plan.md`, `decisions.md`, this `CLAUDE.md`
- **No standalone subagents:** once the team exists, ALL work goes through teammates via SendMessage / spawn

## Team Roster

| Name | Role | Model | Key Capability |
|------|------|-------|---------------|
| {{agent-1}} | {{role}} | {{model}} | {{one-line capability}} |
| {{agent-2}} | {{role}} | {{model}} | {{one-line capability}} |

## Status Check

| What | How |
|------|-----|
| Overview | List active agents |
| Quick scan | Read each agent's `progress.md` in parallel |
| Deep dive | Read agent's `findings.md` then task folder |
| Direction | Read `task_plan.md` |
| Recovery | Read `team-snapshot.md` → spawn from cached prompts |

## Phase Protocol

| Phase | Trigger | Action |
|-------|---------|--------|
| Phase 0 → 1 | {{specific trigger condition}} | {{specific action}} |
| Phase 1 → 2 | ... | ... |

## Key Protocols

| Protocol | Trigger | Action |
|----------|---------|--------|
| 3-strike escalation | Agent reports 3 failures on same task | Read `progress.md`, give new direction |
| Phase advance | Phase complete | Spawn next-phase agents from snapshot |
| Context overflow | Agent reports context full | Recover via `team-snapshot.md` |

## File Structure

```
.plans/{{team_name}}/
  CLAUDE.md            # this file
  task_plan.md         # team-lead owns
  decisions.md         # team-lead owns
  team-snapshot.md     # team-lead owns
  {{agent-1}}/
    task_plan.md
    progress.md
    findings.md
  {{agent-2}}/
    task_plan.md
    progress.md
    findings.md
```

## Style Decisions

| # | Decision | Source | Status |
|---|----------|--------|--------|
| SD-1 | {{e.g., responses in English}} | {{user preference / convention}} | Manual |
