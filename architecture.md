# Architecture

## Monorepo Layout

- `core/config`: YAML parsing, schema types, and validation
- `core/proxy`: HTTP-first ingress proxy runtime
- `resilience/circuit-breaker`: request failure isolation primitive
- `coordinator/crdt`: coordination-safe replicated state primitives
- `cli/dss`: local developer and operator command surface
- `observability/collector`: Python helpers for metrics and tracing configuration

## Initial Request Path

1. A service definition is loaded from `dss.yaml`.
2. The CLI starts `HttpProxyRuntime` for a named service.
3. `HealthChecker` maintains per-target health hints from active probes and runtime feedback.
4. `TargetPool` selects the next eligible upstream target.
5. A per-target `CircuitBreaker` gates the request path and opens on repeated failures.
6. The proxy forwards the request, retries idempotent failures on alternate targets, and streams the response back.

## Near-Term Expansion

- Protocol adapters in `core/protocol`
- Retry and bulkhead primitives under `resilience/*`
- Multi-node topology sync and consensus under `coordinator/*`
- Metrics, traces, and dashboards under `observability/*`
- Kubernetes and Docker integrations under `integrations/*`

## Current Control Plane

- `NodeCoordinator` maintains last-write-wins service state and node heartbeats.
- `CoordinatorHttpServer` exposes registration, merge, heartbeat, and snapshot endpoints.
- The coordinator is intentionally lightweight for now: it is a single-node control-plane stub designed to validate contracts before consensus and replication layers are added.
