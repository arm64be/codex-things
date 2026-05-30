# Runtime And Compiler Notes

## General Checks

- Confirm debug vs release mode, optimization level, assertions, sanitizers, source maps, and logging level.
- Record runtime versions and dependency versions.
- Check whether the workload is cold-start, warm steady-state, or mixed.
- Watch for cgroup CPU and memory limits inside containers.
- Check whether the process is CPU-saturated, IO-waiting, lock-waiting, or GC-pausing before changing algorithms.

## Compiled Languages

- Compare debug and release builds before profiling hot loops.
- Inspect compiler flags, target CPU, link-time optimization, panic/exception mode, and symbol visibility.
- For C/C++/Rust, check allocation frequency, copies, bounds checks, virtual calls, inlining, branch predictability, and data locality.
- Use sanitizers for correctness, not for performance numbers.
- Measure compile-time optimizations separately from runtime optimizations.

## Managed Runtimes

- Separate startup, warmup, and steady-state measurements.
- Account for JIT compilation, tiered optimization, GC, class loading, reflection, dynamic dispatch, and allocation rate.
- Tune heap and GC only after reducing allocation pressure and object lifetime problems.
- Keep p95/p99 pause behavior visible; average throughput can hide bad tail latency.

## Interpreted And Dynamic Languages

- Remove repeated imports, parsing, regex construction, object conversion, and attribute lookups in hot paths before reaching for native extensions.
- Use vectorized libraries only when data sizes justify conversion and transfer costs.
- Check whether slow code is actually in the interpreter, a database driver, serialization, or a C extension.
- Avoid broad rewrites to another language unless profiling proves a narrow native boundary is insufficient.

## Frontend And UI

- Measure interaction latency, main-thread blocking, bundle size, render count, layout thrash, network waterfalls, and memory growth.
- Reduce rendered nodes, subscriptions, derived state churn, and payload size before memoizing everything.
- Memoization is a cache: require stable keys, measured re-render cost, and bounded retained memory.
