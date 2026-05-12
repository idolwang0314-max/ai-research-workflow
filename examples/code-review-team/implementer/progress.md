# implementer — Progress

## Phase 1: Fix

Accepted findings from team-lead: **#1, #2, #3** (skip #4 nit per team-lead's call).

- Finding #1 (bug, atomic refill): rewrote token-bucket refill as a Redis Lua script. Commit `a9c12fe` — `fix: bug make ratelimit refill atomic (re: reviewer finding #1)`
- Finding #2 (bug, key namespacing): renamed key prefix to `ratelimit:gateway:`. Added a one-time migration guard so existing `rl:` keys are read once at startup and copied. Commit `e7b04d2` — `fix: bug namespace ratelimit keys (re: reviewer finding #2)`
- Finding #3 (smell, magic number): promoted `100` → `DEFAULT_BUCKET_CAPACITY` constant; pulled from config when present. Commit `2d51a98` — `fix: smell extract bucket capacity constant (re: reviewer finding #3)`

All three commits pushed to PR#123. Unit tests in `tests/test_ratelimit.py` still pass; added 2 new tests for concurrent-refill and key-namespacing.

phase=1 done.
