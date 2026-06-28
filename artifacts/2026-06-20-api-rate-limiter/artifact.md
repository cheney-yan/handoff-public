# Public API Rate Limiter

A per-API-key rate limiter for the public API, with limits configurable per plan and a fail-open design so that an outage in the limiter backend never blocks the request path. The chosen approach is sliding-window counters in Redis, selected because billing is measured in requests per minute and a sliding window maps directly to that model.

## Problem

The public API needs rate limiting that satisfies three requirements:

- **Per-API-key limits** — limits are applied per API key.
- **Configurable per plan** — free plan = 60 requests/min, pro plan = 600 requests/min.
- **Must not block the request path** — if the limiter backend is unavailable, requests must still be served rather than failing.

Billing is computed on requests/min, so the limiter's accounting model needs to align with the per-minute billing model.

## Solution

Implement **sliding-window counters in Redis**, keyed per API key, with per-plan thresholds (free = 60/min, pro = 600/min).

The limiter is **fail-open**: if Redis is unreachable, the request is allowed through and a metric (`limiter_fail_open`) is emitted. This keeps the limiter backend off the critical request path, so a Redis outage degrades to "no limiting" rather than a wave of 5xx errors.

When a key exceeds its limit, the API returns HTTP 429 with a `Retry-After` header.

## User stories

1. **Free user gets rate limited.** As a free user, after 60 requests in a minute I receive an HTTP 429 response with a `Retry-After` header.
2. **Pro user has a higher limit.** As a pro user, I get 600 requests/min before being rate limited.
3. **Ops visibility on backend failure.** As an ops engineer, when Redis is down I see a `limiter_fail_open` metric spike rather than a wave of 5xx errors.

## Decisions

### Chosen: sliding-window counters in Redis
Selected because billing is per-minute (requests/min) and a sliding window maps directly to that accounting model, making it the simplest to reason about for billing alignment.

### Chosen: fail-open behavior
If Redis is unreachable, allow the request and emit a `limiter_fail_open` metric. This satisfies the requirement that the limiter must not block the request path when its backend is down, trading enforcement for availability during an outage.

### Rejected: token bucket in Redis
Token bucket was considered because it gives smoother handling of bursts. It was rejected because billing is per-minute and sliding-window counters map to that billing model directly, whereas token bucket does not align as cleanly.

## Testing

- **Unit tests** for the window math, specifically at minute boundaries.
- **Integration test** for the fail-open path by killing Redis and verifying requests are still served and the metric is emitted.

## Out of scope

For v1:

- Per-endpoint limits.
- Distributed-clock skew handling.

## Open questions

- Should we expose limit headers (e.g. `X-RateLimit-Remaining`) on **every** response, or only when a key is **near** its limit?

---

*Note: a staging Redis connection string containing credentials was provided in the source conversation and has been removed. Use `<redacted>` / a securely-managed secret for the staging Redis URL.*
