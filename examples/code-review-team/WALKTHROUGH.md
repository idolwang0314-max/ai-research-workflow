# WALKTHROUGH — code-review-team in action

> A narrated 4-step tour of the same example sitting in this directory. Read this **after** you've skimmed `CLAUDE.md` and `task_plan.md`. By the end, the file layout in this folder should feel inevitable rather than mysterious.

---

## The scenario

A teammate opened **PR#123 — `feat: add rate limiting middleware`**. It introduces a Redis-backed token bucket on the API gateway. You (team-lead) want a structured second opinion before merging.

You decide the team will be:

- `reviewer` — reads the diff, files concerns, **does not write code**
- `implementer` — applies the fixes you accept, one commit per finding

You spawn them once. The conversation history of each subagent eventually dies — but every decision and finding is on disk under this directory, so the work survives.

---

## Step 1 — Spawn reviewer

In your team-lead conversation:

```text
SendMessage(to: "reviewer",
  message: "Review PR#123 (rate limiting middleware).
            Read .plans/code-review-team/reviewer/task_plan.md first.
            Scope: only the files in the diff plus caller/callee paths.
            Write findings to reviewer/findings.md.
            Do not touch source files.")
```

Reviewer reads its `task_plan.md`, walks the diff, and produces `reviewer/findings.md` (see the realistic sample committed alongside this walkthrough). It marks `phase=0 done` in `reviewer/progress.md` and pings team-lead.

**What landed on disk after Step 1**

```
reviewer/
  task_plan.md     ← unchanged (template)
  findings.md      ← NEW, structured bullet list
  progress.md      ← NEW, contains "phase=0 done"
```

---

## Step 2 — Filter findings (team-lead's call)

Reviewer reported 4 issues. You look at them and decide:

| # | Tag | What reviewer said | Your call |
|---|-----|---------------------|-----------|
| 1 | **bug** | Token bucket refill race under concurrent calls — `INCR` before `EXPIRE` | **Accept** |
| 2 | **bug** | Rate-limit key never namespaced; collides with other features in same Redis | **Accept** |
| 3 | **smell** | Magic number `100` used in three places | **Accept** (cheap) |
| 4 | **nit** | Variable naming `rt_l` is cryptic | **Reject** — file is touched constantly, churn cost > readability win right now |

You don't edit `findings.md` (that's reviewer's artifact). You record the decision in your own `decisions.md` (or just in the implementer's dispatch message — see Step 3). The accepted list lives in **one place only** so there's no drift.

---

## Step 3 — Spawn implementer with the filtered list

```text
SendMessage(to: "implementer",
  message: "Apply fixes from reviewer/findings.md items 1, 2, 3 only (skip 4).
            Read .plans/code-review-team/implementer/task_plan.md first.
            One commit per finding, format:
              fix: <tag> <description> (re: reviewer finding #N)
            Update implementer/progress.md as you go.")
```

Implementer reads its `task_plan.md` and the relevant lines of `findings.md`, makes the three fixes, commits each separately, and marks `phase=1 done` in `implementer/progress.md`.

**What landed on disk after Step 3**

```
implementer/
  task_plan.md     ← unchanged
  progress.md      ← NEW, contains 3 commit references + "phase=1 done"
```

Plus 3 real commits in the actual application repository — `findings.md` is unchanged because reviewer owns it.

---

## Step 4 — Verify and merge (team-lead)

You run the project test suite. You decide to merge. You don't need to update any files in this directory unless you want a permanent record; the artifacts above are already a faithful audit trail of what happened and why.

---

## Why the structure looks the way it does

| Convention | What problem it solves |
|------------|-----------------------|
| `reviewer/findings.md` is **read-only** for everyone else | No agent overwrites another's truth |
| `implementer/progress.md` lists commit hashes | After context death, anyone can resume by reading 1 file |
| Team-lead's filtering happens **in the dispatch message**, not in findings.md | One source of truth for "what reviewer thinks", another for "what got actually fixed" |
| Both subagents read their own `task_plan.md` first | They onboard from disk, not from chat history |

---

## How to adapt this for your own team

1. Copy this `code-review-team/` directory to your project's `.plans/<your-team>/`
2. Edit `CLAUDE.md` Team Roster + Phase Protocol for your roles
3. Edit each agent's `task_plan.md` to match its actual mission
4. Spawn agents pointing them at their `task_plan.md`
5. Read each agent's `progress.md` to know where they are — **never rely on chat history**

That's the whole pattern. The repo is small on purpose; everything else (multi-agent voting, hierarchical teams, etc.) is layered on top of this primitive.
