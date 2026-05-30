---
name: practical-code-optimization
description: Optimize real-world code by profiling first, reducing work, and accounting for production constraints, compilers, runtimes, databases, networks, and deployment environments. Use when performance, latency, throughput, memory, CPU, build speed, startup time, or scalability work is requested.
---

# Practical Code Optimization

## Start Here

Treat optimization as an evidence loop, not a rewrite instinct.

1. Define the user-visible target: latency percentile, throughput, memory, CPU, startup, build time, cost, or battery.
2. Reproduce the slow path in an environment close enough to matter: hardware, container limits, dataset size, compiler flags, runtime mode, network shape, and dependency versions.
3. Measure before changing code. Use `scripts/perf-env` to capture context and `scripts/perf-baseline -- ...` for command-level timing.
4. Profile the dominant cost. Prefer wall-clock profilers for latency, CPU profilers for compute, allocation/heap tools for memory, tracing for distributed waits, and query plans for database work.
5. Make the smallest change that removes measured work, data movement, contention, or waiting.
6. Re-run the same benchmark/profile and keep correctness tests in the loop.

## Default Biases

- First ask what can be skipped, delayed, narrowed, moved out of the hot path, or represented with less data.
- Do not add caching until repeated expensive work is proven and invalidation is clear.
- Do not add wrappers, services, queues, RPC boundaries, or abstraction layers unless they remove more cost than they add.
- Prefer co-location and monolith paths when cache, network, serialization, consistency, and deployment overhead dominate.
- Prefer simpler algorithms and data layouts before concurrency. Concurrency amplifies contention, scheduling, and memory costs.
- Optimize for the real bottleneck, not the most interesting code.

## Workflow

1. Baseline:
   ```bash
   /path/to/practical-code-optimization/scripts/perf-env
   /path/to/practical-code-optimization/scripts/perf-baseline --runs 10 -- npm test -- --runInBand
   ```
2. Localize:
   - CPU-bound: sample/profile with language-native tools, `perf`, Instruments, Visual Studio profiler, `go tool pprof`, `cargo flamegraph`, `py-spy`, `node --prof`, or equivalent.
   - IO-bound: trace syscalls, database plans, connection reuse, DNS/TLS, disk queueing, object storage, and payload sizes.
   - Memory-bound: inspect allocations, object retention, GC pauses, copying, cache locality, and peak resident set size.
   - Build/startup-bound: inspect compiler mode, incremental cache, dependency graph, bundling, class loading, JIT warmup, and cold path imports.
3. Decide:
   - Remove work: fewer rows, fewer allocations, fewer conversions, fewer calls, fewer renders, fewer files, fewer dependencies.
   - Shape work: better indexes, streaming, pagination, precomputed stable data, tighter data structures, batch boundaries that match actual latency.
   - Move work: background only when freshness permits; closer to data only when it reduces transfer and consistency cost.
   - Cache work: only with measured hit rate, bounded memory, invalidation, stampede handling, and observability.
4. Verify:
   - Compare baseline and changed runs with the same command, warmup, dataset, and machine state.
   - Keep a rollback path for risky runtime, compiler, schema, or concurrency changes.

## References

- See [references/principles.md](references/principles.md) for the optimization decision model.
- See [references/profiling-playbook.md](references/profiling-playbook.md) for profiler selection and evidence quality.
- See [references/runtime-compiler-notes.md](references/runtime-compiler-notes.md) for runtime and compiler checks.
- See [references/environment-checklist.md](references/environment-checklist.md) for production-like measurement context.
