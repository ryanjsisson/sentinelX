# Data Layer & Observability

> If it's not logged, it didn't happen. If it's not measured, it doesn't matter.

## Core Principle

Observability ships with the first feature. Every agent treats logging, metrics, and tracing as first-class requirements — not afterthoughts bolted on after an incident.

## Data Layer Rules

### Schema Is a Contract

- Database schemas, API responses, and event payloads are **versioned interfaces**.
- Schema changes follow the same rigor as API changes: backward-compatible, documented, reviewed.
- Breaking changes use a two-phase migration pattern (see Infra doc).

### One Source of Truth Per Entity

- Each domain entity lives in exactly one authoritative location.
- Derived data (caches, materialized views, denormalized read models) is explicitly labeled as derived.
- Every derived store has a defined refresh/invalidation strategy. No stale-by-default.

### Query Discipline

- No raw SQL in service layers. All queries go through the repository layer.
- Every query that touches production has an `EXPLAIN` plan reviewed before merge.
- N+1 queries are banned. Use joins, batch fetches, or eager loading.

## Logging

- **Structured JSON only.** No `print()` statements. No unstructured strings.
- Every log entry contains at minimum: `timestamp`, `severity`, `service`, `trace_id`, `message`.
- Logs are searchable — any specific request is locatable within 30 seconds.

## Metrics — Three Required Levels

| Level | Examples | Purpose |
|---|---|---|
| **System** | CPU, memory, disk, container health | Infrastructure is alive and healthy. |
| **Application** | Request latency (p50/p95/p99), error rates, queue depth | Services are performing within bounds. |
| **Business** | Signups, conversions, pipeline throughput | The product is functioning as intended. |

- Dashboards for all three levels exist from day one.
- Alerts fire on **anomalies**, not just static thresholds.

## Tracing

- Every request receives a **trace ID at the edge**.
- That ID propagates through every service call, queue message, and background job.
- When something fails, one ID traces from entry point to root cause. No cross-referencing timestamps across log streams.

## Incident Response Pattern

```
1. Alert fires           → automated notification with trace ID
2. Dashboard confirms    → scope the blast radius
3. Trace ID follows      → pinpoint root cause
4. Fix deploys           → through the standard pipeline (no hotfix shortcuts)
5. Post-mortem written   → ADR-style doc: what happened, why, what changed
```