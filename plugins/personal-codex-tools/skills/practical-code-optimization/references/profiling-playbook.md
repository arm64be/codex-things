# Profiling Playbook

## Pick Evidence By Bottleneck

- Latency: wall-clock tracing, spans, request timelines, queue time, p50/p95/p99, and cold vs warm runs.
- CPU: sampling profiler, flamegraph, compiler optimization mode, vectorization reports, and hot-loop assembly only when justified.
- Memory: allocation profiler, heap snapshots, retained objects, peak RSS, GC pause time, and copy counts.
- Database: query plan, actual rows, buffer hits, lock waits, indexes, cardinality estimates, N+1 checks, and transaction scope.
- Network: connection reuse, DNS/TLS, payload size, compression, retries, fan-out, and regional distance.
- Build/startup: module graph, dependency count, incremental cache, generated code, import/class loading, JIT warmup, and bundler stats.

## Evidence Quality

Strong evidence:

- Comes from the same command or request before and after a change.
- Uses realistic data volume and representative configuration.
- Includes multiple runs, warmup rules, machine context, and variance.
- Is paired with correctness tests or invariants.

Weak evidence:

- One manual run on a quiet dev path.
- Profiling debug builds when production uses optimized builds.
- Benchmarking tiny data that fits in cache when production data does not.
- Measuring a synthetic endpoint that skips authentication, serialization, or database work.
- Comparing different machines, container limits, datasets, or runtime flags.

## Useful Commands

Command-level baseline:

```bash
scripts/perf-baseline --runs 10 -- ./your-command --flag
```

Linux wall and resource usage:

```bash
/usr/bin/time -v ./your-command
perf stat ./your-command
perf record -g ./your-command
perf report
```

Language examples:

```bash
go test -bench=. -benchmem ./...
go test -run '^$' -bench BenchmarkName -cpuprofile cpu.out -memprofile mem.out ./pkg
go tool pprof cpu.out
cargo bench
RUSTFLAGS="-C target-cpu=native" cargo test --release
python -m cProfile -o profile.out script.py
py-spy top --pid <pid>
node --prof app.js
java -XX:StartFlightRecording=filename=recording.jfr,duration=60s -jar app.jar
```

Use the tool that answers the current question. Do not collect large traces without a plan for reading them.
