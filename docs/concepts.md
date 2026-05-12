# Concepts

This document explains the core ideas behind `ai-research-workflow`.
Read this once; the rest of the repo is implementation detail.

## 1. The team-lead is the main conversation, not a spawned agent

A "team" has exactly one orchestrator: the main agent loop the user is
talking to. Spawned subagents are *workers*. They never make planning
decisions, never re-prioritize, never reach back to the user.

**Why:** every spawned agent that thinks it can plan introduces a chance
for two agents to make contradictory decisions about the same artifact.
Centralizing planning in the user-facing loop keeps the project coherent.

**How to apply:** in your team's `CLAUDE.md`, write a "Control Plane" section
that explicitly says the team-lead owns user alignment, scope control, task
decomposition, and phase transitions. The roster table only describes workers.

## 2. Files, not messages, are the source of truth

Messages between agents are dispatch (kicking off work). State lives on disk.

- A team-lead never asks an agent "what's your status?" via message; it reads
  `agent-name/progress.md`.
- An agent never asks the team-lead "what's the latest plan?" via message; it
  reads `task_plan.md`.

**Why:** messages are ephemeral, ordered, and only delivered when the recipient
is idle. Files are persistent, queryable, and survive agent crashes / context
overflow.

**How to apply:** every team has these standard files at minimum:

| File | Owner | Read by |
|------|-------|---------|
| `task_plan.md` | team-lead | all agents |
| `progress.md` | each agent (in own dir) | team-lead, optionally peers |
| `findings.md` | each agent (in own dir) | team-lead, optionally peers |
| `decisions.md` | team-lead | all agents |
| `team-snapshot.md` | team-lead | recovery only |

## 3. `findings` ≠ `progress`

Two log files per agent, separated by purpose:

- `progress.md` — chronological log of what the agent did, in time order.
- `findings.md` — current beliefs about the world. State, not history.

**Why:** the team-lead almost always wants the latter ("what is true *now*?")
not the former ("what happened in what order?"). Mixing them forces every
reader to scan the full history to extract current state.

## 4. Phase protocols

Phase transitions are mechanical, not discretionary. Each team's `CLAUDE.md`
declares a "Phase Protocol" table:

| Phase | Trigger | Action |
|-------|---------|--------|
| Phase 0 → 1 | All agents report `phase=0 done` in their `progress.md` | Team-lead spawns Phase 1 agents with prompts from `team-snapshot.md` |
| Phase 1 → 2 | ... | ... |

**Why:** without explicit triggers, teams drift — some agents move on, others
stay behind, work duplicates or gets lost.

## 5. The snapshot pattern

Every team has a `team-snapshot.md` containing:

- The full onboarding prompt for each agent (the *exact* string that should be
  passed to spawn that agent fresh)
- Current phase + what the agent should do next
- Pointers to the agent's own files (so the agent can self-bootstrap by reading
  its own `task_plan.md` and `progress.md`)

When an agent dies or context overflows, recovery is: `Read team-snapshot.md`
→ spawn replacement with the cached prompt → done.

**Why:** without this, restarting an agent is a five-minute prompt-writing job.
With it, it's thirty seconds of copy-paste.

## 6. The 3-strike escalation

If an agent reports failure 3 times on the same task, the team-lead must:

1. Read the agent's `progress.md` to understand what went wrong
2. Either give a new direction, decompose the task further, or reassign

**Why:** infinite retry loops are the most expensive failure mode in
multi-agent systems. A hard escalation budget prevents them.

## 7. Style, not framework

Nothing here is enforced by code. These are *patterns* — disciplines you and
your team-lead agree to follow. The templates in `templates/` are starting
points, not rigid scaffolds. Adapt freely; just keep the invariants:
**files-as-truth**, **one team-lead**, **mechanical phase transitions**.
