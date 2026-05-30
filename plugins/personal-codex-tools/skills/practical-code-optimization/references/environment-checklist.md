# Environment Checklist

Capture enough context that a faster result can be trusted.

## Machine

- CPU model, core count, governor, turbo state, thermal throttling, and architecture.
- Memory size, swap, NUMA shape, and current pressure.
- Disk type, filesystem, mount options, and free space for IO-heavy work.
- Kernel, OS, container runtime, VM layer, and cgroup limits.

## Workload

- Dataset size, cardinality, skew, file count, row count, and object sizes.
- Cold cache vs warm cache.
- Warmup count and run count.
- Concurrency level, request mix, and queue depth.
- Feature flags, build flags, logging, tracing, and environment variables.

## Dependencies

- Database version, schema, indexes, query plans, connection pool settings, and lock behavior.
- Runtime and compiler versions.
- Network region, DNS, TLS, proxy, load balancer, and service fan-out.
- Third-party APIs and rate limits.

## Production Fit

A local benchmark can still be useful if you state what it does not cover. Mark gaps explicitly: smaller data, no network, debug build, single tenant, no contention, disabled auth, or missing observability overhead.
