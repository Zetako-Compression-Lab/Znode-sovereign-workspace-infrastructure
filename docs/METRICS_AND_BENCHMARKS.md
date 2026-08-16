# ZNode Metrics and Benchmarks

This document defines how Zetako publishes ZNode performance and operational evidence.

The objective is to make each public number understandable in context rather than presenting isolated headline figures.

## Current engineering snapshot

The current repository/tooling snapshot provides the following directly measurable engineering figures:

| Metric | Current value | Evidence type |
|---|---:|---|
| Active product code | **285,103 lines** | Static count excluding tests/docs/reports/build/dist/node_modules/caches |
| Active product files | **671** | Static count |
| Automated test suite | **1,613 tests** | Pytest collection |
| Test files | **134** | Static count across root, z-connect and zmedia tests |
| HTTP routes | **327** | Static route scan |
| WebSocket routes | **2** | Static route scan |
| Known Python dependency vulnerabilities | **0 detected** | `pip-audit` against `requirements.txt` |
| Agent tools exposed | **6 read-only** | Current agent tool registry |
| Compliance export data types | **9** | Current compliance model |
| Compliance case states | **6** | Current compliance model |
| Supported production room profile | **10 participants** | Configuration/profile limit, not load-test ceiling |
| z-connect HTTP rate limit | **60 req/s** | Configured default |
| z-connect WebSocket rate limit | **120 messages/s** | Configured default |

These metrics describe codebase/configuration state rather than end-to-end performance.

## Existing observability

ZNode already exposes useful request and runtime observability, including:

- `X-Request-ID`;
- `X-Response-Time-Ms`;
- `X-ZNode-DB-Query-Count`;
- `X-ZNode-DB-Query-Time-Ms`;
- `Server-Timing`;
- an admin-protected `/metrics` endpoint with route/status counters and average timing.

Current `/metrics` counters are not percentile histograms. Public p50/p95/p99 results will therefore be based on real request samples or histogram instrumentation rather than derived from averages.

## Live latency baseline

A first instrumented external baseline has now been published for the public development deployment.

The key result is that the measured cold-request wall time is dominated by the external network/TCP/TLS/public ingress path rather than ZNode application processing:

| Endpoint | Total p50 | ZNode backend p50 | DB p50 |
|---|---:|---:|---:|
| `GET /healthz` | **2,304.53 ms** | **1.14 ms** | **0.00 ms** |
| `GET /login` | **1,191.04 ms** | **21.53 ms** | **8.02 ms** |

The detailed report preserves DNS, TCP, TLS, first-byte, download, backend, and database timings so the same methodology can be repeated after infrastructure optimization.

See [`LIVE_LATENCY_BASELINE.md`](LIVE_LATENCY_BASELINE.md).

## Evidence classes

### MEASURED BENCHMARK

A controlled test with:

- named ZNode version or commit/release;
- hardware specification;
- operating system and relevant runtime versions;
- workload definition;
- concurrency level;
- network conditions where relevant;
- measurement method;
- included and excluded operations;
- repeated-run statistics where appropriate.

### PILOT OBSERVATION

An anonymized metric from a real pilot environment.

Pilot observations are useful because they describe real usage, but they are not treated as reproducible laboratory benchmarks.

### ARCHITECTURAL TARGET

A planned capacity, scaling objective, or design target that has not yet been validated under a controlled test.

Targets will be labeled clearly and will not be presented as achieved capacity.

### CONFIGURED PROFILE

A supported or default configuration value such as a room participant limit or rate-limit setting.

A configured profile is **not** automatically equivalent to measured capacity.

## Metric families being measured

### API

Planned public measurements:

- request latency p50 / p95 / p99;
- request throughput;
- error rate;
- authentication latency;
- selected high-value endpoint measurements.

Initial scenarios include login, send message, open conversation, search, file upload/download, and meeting creation.

### Messaging

Planned public measurements:

- client→server→client message delivery latency;
- sustained messages per second;
- concurrent WebSocket connections;
- connection setup time;
- reconnection behavior after interruption;
- message fan-out behavior for group spaces.

The current z-connect WebSocket rate-limit default is **120 messages/s**. This is a configured abuse-control value, not a validated throughput ceiling.

### Meetings and SFU

The current supported production room profile is **10 participants per room**.

The production configuration requires SFU topology by default and does not treat higher room sizes as supported production capacity unless explicitly overridden.

Dedicated benchmarks will measure:

- concurrent meeting rooms;
- participants at supported profiles;
- SFU CPU utilization;
- SFU memory utilization;
- ingress/egress bandwidth;
- join latency;
- media latency;
- jitter and packet loss;
- behavior under defined participant mixes.

### Files and storage

Current configured controls include per-user upload and storage quotas, ACL checks, preview/thumbnail paths, and cleanup behavior.

Public performance measurements will cover:

- upload throughput;
- download throughput;
- small-file versus large-file behavior;
- concurrent file operations;
- preview-generation latency;
- storage growth under representative workloads.

### Infrastructure footprint

Planned measurements:

- idle CPU;
- idle RAM;
- base storage footprint;
- CPU/RAM under 10, 25 and 50 active-user scenarios where applicable;
- process/service footprint;
- network use;
- GPU requirement or non-requirement for defined features.

The current V1 production profile is documented around a single-node deployment with a single FastAPI worker, SQLite and local filesystem storage.

### Deployment

The public repository currently records an internally measured **3 min 40 sec precompiled deployment example**.

A dedicated deployment report will add the exact machine profile, artifact state, included/excluded steps, and health criteria.

Additional measurements will include:

- clean-host deployment time;
- time to green healthcheck;
- restart time;
- upgrade time;
- rollback time where supported;
- service recovery behavior.

### Reliability

Planned evidence:

- service availability;
- API error rate;
- crash-free runtime;
- unexpected service restarts;
- failed jobs/queues;
- recovery time after controlled failure scenarios;
- MTTR when sufficient incident history exists.

### Pilot usage

Where publication is privacy-safe, anonymized aggregate pilot metrics may include:

- registered users;
- active users;
- sessions;
- messages;
- group activity;
- meetings;
- meeting minutes;
- file operations;
- storage consumption;
- peak and typical infrastructure utilization;
- latency and error observations.

## Statistical reporting

Where latency distributions matter, Zetako publishes percentiles rather than averages alone.

Typical reporting fields include:

- p50;
- p95;
- p99;
- error rate;
- sustained throughput;
- test duration;
- sample count.

Percentiles will be calculated from real samples. Error rate is reported as failed requests divided by total requests, and throughput is reported over an explicit measurement window.

## Comparison principle

A result is only directly comparable to another result when the workload, measurement boundary, and environment are sufficiently similar.

For that reason, ZNode reports avoid mixing:

- service-only timing with end-to-end user timing;
- LAN and Internet latency without labeling network conditions;
- idle-resource figures with active-load figures;
- pilot observations with synthetic load tests;
- configured profile limits with validated load ceilings;
- architectural targets with validated capacity.

## Benchmark campaign

The next evidence campaign is structured around:

1. API scenario load tests for p50/p95/p99, errors and request rate;
2. two-client messaging/WebSocket scenarios for delivery latency, throughput and reconnect behavior;
3. SFU runs at supported 5- and 10-participant profiles with CPU/RAM/network/WebRTC stats;
4. file transfer matrices across representative size buckets;
5. controlled deployment timing;
6. runtime footprint sampling at idle and defined active-user loads;
7. anonymized pilot usage and reliability exports.

Each published result will retain its measurement boundary and environment so future product versions can be compared on equivalent terms.
