# reviewer — Findings for PR#123 (`feat: add rate limiting middleware`)

> Each bullet: `[TAG] file:line — description`. Tags: `bug` (will break in prod), `smell` (code quality), `risk` (subtle), `nit` (style).

- [bug] `middleware/ratelimit.py:42` — Token bucket refill is non-atomic under concurrent calls. We call `INCR` then `EXPIRE` as two round-trips; if the process dies between them, the key has no TTL and the bucket is effectively pinned at limit forever. Fix: use a Lua script or `SET ... EX NX` to make refill atomic.
- [bug] `middleware/ratelimit.py:78` — The Redis key `rl:{user_id}` is not namespaced by feature. Any other code path using `rl:` as a prefix will collide. Suggested key format: `ratelimit:gateway:{user_id}`.
- [smell] `middleware/ratelimit.py:23,55,91` — Literal `100` used in three places for the bucket capacity. Promote to a module constant `DEFAULT_BUCKET_CAPACITY = 100`, or read from config.
- [nit] `middleware/ratelimit.py:14` — Variable `rt_l` for "rate limiter" instance is cryptic; consider `rate_limiter`. Low priority — this file gets touched a lot.

**Summary: 2 bugs, 1 smell, 0 risks, 1 nit.**

phase=0 done.
