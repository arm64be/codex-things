# Optimization Principles

## The Core Loop

Optimization work should create a short chain of evidence:

1. A user-visible target or operational target.
2. A reproducible baseline.
3. A profile or trace that points to a dominant cost.
4. A change that directly attacks that cost.
5. A comparison showing the change helped without breaking correctness.

If any link is missing, gather it before making broad changes.

## Go Faster By Doing Less

Look for eliminated work before clever work:

- Avoid computing values no caller uses.
- Avoid fetching rows, columns, files, fields, or payloads the path does not need.
- Avoid repeated parsing, serialization, reflection, regex compilation, allocation, and logging in hot paths.
- Avoid rendering, diffing, validating, scanning, sorting, or copying more data than the user can observe.
- Avoid synchronous waits for noncritical work such as analytics, notifications, cache warming, or prefetches.

## Cache Skeptically

Caching can be the right answer, but it is not free. Require:

- Repeated expensive work with a measurable hit rate.
- A bound on memory, storage, and object count.
- Correct invalidation or acceptable staleness.
- Stampede protection for cold keys.
- Metrics for hit rate, evictions, stale reads, and backend fallback cost.

Prefer removing work or narrowing data before adding a cache. Prefer one simple cache close to the hot path before layered caches with unclear ownership.

## Monoliths, Services, And Boundaries

Distributed boundaries are performance features only when they remove more cost than they add. Account for:

- Serialization, TLS, DNS, connection pools, retries, backoff, and observability overhead.
- Extra cache and consistency layers created by splitting data ownership.
- Tail latency from fan-out and partial failure.
- Deployment coordination and schema/version negotiation.

When a hot path repeatedly crosses service boundaries to assemble one user-visible operation, a co-located module or monolith path may be the optimization.

## Abstraction Cost

Abstractions are worth keeping when they reduce real complexity. Challenge them in hot paths when they cause:

- Extra allocation, boxing, dynamic dispatch, reflection, proxying, middleware chains, or generic serialization.
- Repeated conversions between nearly identical shapes.
- Lost compiler/runtime optimization opportunities.
- Hidden IO, locks, retries, or logging.

Remove or specialize the smallest layer that the profile justifies. Do not flatten architecture globally because one path is hot.

## Data Shape Beats Micro-Tuning

Many wins come from changing the shape of data:

- Better index or query plan.
- Streaming instead of buffering.
- Compact structs, arrays, or columnar layouts for tight loops.
- Fewer joins or round trips.
- Precomputed stable summaries.
- Smaller payloads and lower cardinality tags.

Micro-optimizations matter after the algorithm, data shape, and wait states have been handled.
