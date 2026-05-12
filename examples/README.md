# Examples

Each subdirectory is a self-contained example team illustrating the patterns
from the parent repo.

| Example | Agents | Demonstrates |
|---------|--------|-------------|
| [`code-review-team/`](code-review-team/) | reviewer + implementer | Phase 0 → Phase 1 handoff via files-as-truth; team-lead as filter between agents |

## Reading order

Start with [`code-review-team/`](code-review-team/) — it's the minimal 2-agent
example. Inside it:

1. **[`CLAUDE.md`](code-review-team/CLAUDE.md)** — the static team contract
   (roles, phase protocol, file layout).
2. **[`task_plan.md`](code-review-team/task_plan.md)** — the project-level
   plan team-lead maintains.
3. **[`WALKTHROUGH.md`](code-review-team/WALKTHROUGH.md)** — a narrated tour
   showing the same team in motion across the 4 lifecycle steps. Read this to
   see *why* the file layout is shaped the way it is.
4. **`reviewer/`** and **`implementer/`** — per-agent `task_plan.md` (template)
   plus realistic `findings.md` / `progress.md` artifacts captured from a
   simulated run, so you can see the on-disk state at each phase.
