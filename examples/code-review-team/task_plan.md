# code-review-team — Project Plan

## Goal

Review pull request PR#123 (adds rate limiting middleware), produce a list
of confirmed issues, and apply fixes.

## Phases

### Phase 0: Review
- Reviewer reads the PR diff and writes `reviewer/findings.md` with one
  bullet per issue, tagged `bug | smell | risk | nit`.
- Reviewer marks `phase=0 done`.

### Phase 1: Fix
- Team-lead filters `reviewer/findings.md` → accepted issues passed to
  implementer.
- Implementer applies each fix as a single commit.
- Implementer marks `phase=1 done`.

### Phase 2: Verify
- Team-lead runs the project test suite, decides merge.

## Decisions

| # | Decision | Source |
|---|----------|--------|
| D-1 | Reviewer must not modify code, only report | example invariant |
