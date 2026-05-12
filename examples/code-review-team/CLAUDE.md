# code-review-team — Team Operations Guide

> Two-agent team: a `reviewer` finds issues in a pull request, an
> `implementer` applies the fixes. Demonstrates Phase 0 → Phase 1
> handoff via files-as-truth.

## Team-Lead Control Plane

- team-lead = the main conversation
- team-lead owns: PR scope, accepting/rejecting findings, merge decision
- team-lead reads `reviewer/findings.md` to decide which issues are real

## Team Roster

| Name | Role | Model | Key Capability |
|------|------|-------|---------------|
| reviewer | Code Reviewer | sonnet | Reads PR diff, identifies bugs / smells / risks |
| implementer | Code Fixer | sonnet | Applies confirmed fixes |

## Status Check

| What | How |
|------|-----|
| Reviewer state | Read `reviewer/progress.md` + `reviewer/findings.md` |
| Implementer state | Read `implementer/progress.md` |
| Issues to fix | Read `reviewer/findings.md` (filtered by team-lead) |

## Phase Protocol

| Phase | Trigger | Action |
|-------|---------|--------|
| Phase 0 → 1 | reviewer marks `phase=1 done` in own `progress.md` | team-lead reviews `findings.md`, accepts/rejects each, then spawns implementer with the accepted list |
| Phase 1 → done | implementer marks `phase=1 done` | team-lead runs tests, decides merge |

## File Structure

```
.plans/code-review-team/
  CLAUDE.md
  task_plan.md
  reviewer/
    task_plan.md
    progress.md
    findings.md
  implementer/
    task_plan.md
    progress.md
```
