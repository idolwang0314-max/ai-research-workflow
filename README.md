# ai-research-workflow

> File-based, multi-agent coordination patterns for the
> [Claude Agent SDK](https://docs.claude.com/en/api/agent-sdk).
> Distilled from real PhD-scale research workflows; reusable templates,
> protocols, and a worked example.

## Why

Multi-agent systems break down in three predictable ways:

1. **Lost state** — agents crash, context overflows, sessions die; in-memory progress disappears.
2. **Coordination drift** — agents run in parallel, don't see each other's work, duplicate or contradict effort.
3. **Re-onboarding cost** — restarting an agent means re-explaining everything; minutes of prompt-writing per restart.

This repo is an opinionated answer:

- **Files, not messages, are the source of truth.** Every agent reads/writes plain `.md` files in a known directory layout. Messages are dispatch-only; state lives on disk.
- **One team-lead per team.** The main conversation orchestrates; subagents are workers, never decision-makers.
- **Phase protocols.** Explicit triggers for "phase X done → start phase Y", so handoff is mechanical.
- **Snapshot pattern.** Every team carries a `team-snapshot.md` containing the prompts to re-spawn each agent; recovery is one read away.

## Repository layout

```
ai-research-workflow/
├── README.md                     # You are here
├── LICENSE                       # MIT
├── docs/
│   └── concepts.md               # Core concepts in 5 minutes
├── templates/                    # Copy these to start a new team
│   ├── team-CLAUDE.md            # Team-lead control plane
│   ├── agent-task_plan.md        # Per-agent task plan
│   ├── agent-progress.md         # Per-agent progress log
│   └── team-snapshot.md          # Recovery / re-spawn prompts
└── examples/
    └── code-review-team/         # Minimal 2-agent worked example
        ├── CLAUDE.md
        ├── task_plan.md
        ├── reviewer/task_plan.md
        └── implementer/task_plan.md
```

## Quick start

1. Copy `templates/team-CLAUDE.md` to `your-project/.plans/your-team/CLAUDE.md`
2. Edit the **Team Roster** table to declare your agents
3. Copy `templates/agent-task_plan.md` into each agent subdirectory
4. Spawn the agents using your SDK of choice (Claude Agent SDK, custom orchestrator, etc.); have each agent read its own `task_plan.md` to bootstrap
5. The team-lead reads `progress.md` files (never relies on messages) for live state

Read [`docs/concepts.md`](docs/concepts.md) for the underlying design.

## Patterns at a glance

| Pattern | What it solves |
|---------|---------------|
| File-based coordination | Survives agent crashes, context overflow, session death |
| Team-lead control plane | Prevents two agents making contradictory decisions |
| Phase protocol | Mechanical handoff between work stages |
| Snapshot recovery | Re-spawn any agent from a 2-minute read |
| 3-strike escalation | Stops infinite retry loops on stuck tasks |
| Findings vs. progress | Separates "what's true now" from "what I did" |

## Status

**v0.1** — public skeleton. Templates and concepts are stable. Examples will grow as patterns get extracted from production use.

## License

MIT. See [`LICENSE`](LICENSE).
