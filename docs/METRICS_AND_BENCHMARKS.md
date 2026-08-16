# ZNode Metrics and Benchmarks

This document defines how Zetako intends to publish ZNode performance and operational evidence.

The objective is to make each public number understandable in context rather than presenting isolated headline figures.

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

## Planned metric families

### API

- request latency p50 / p95 / p99;
- request throughput;
- error rate;
- authentication latency;
- selected high-value endpoint measurements.

### Messaging

- message delivery latency;
- sustained messages per second;
- concurrent WebSocket connections;
- connection setup time;
- reconnection behavior after interruption;
- message fan-out behavior for group spaces.

### Meetings and SFU

- concurrent meeting rooms;
- concurrent participants;
- SFU CPU utilization;
- SFU memory utilization;
- ingress/egress bandwidth;
- join latency;
- media latency where measurable;
- jitter and packet loss where measurable;
- behavior under defined participant mixes.

### Files and storage

- upload throughput;
- download throughput;
- small-file versus large-file behavior;
- concurrent file operations;
- preview-generation latency where applicable;
- storage growth under representative workloads.

### Infrastructure footprint

- idle CPU;
- idle RAM;
- CPU under representative load;
- RAM under representative load;
- persistent storage footprint;
- temporary storage behavior;
- network use;
- GPU use or non-requirement for defined features.

### Deployment

- clean-host deployment time;
- precompiled deployment time;
- time to healthy services;
- restart time;
- upgrade time;
- rollback time where supported;
- service recovery behavior.

### Reliability

- service availability;
- error rate;
- crash-free runtime;
- unexpected service restarts;
- failed jobs/queues where applicable;
- recovery time after controlled failure scenarios.

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

Where latency distributions matter, Zetako intends to publish percentiles rather than averages alone.

Typical reporting fields will include:

- p50;
- p95;
- p99;
- minimum/maximum only when useful;
- test duration;
- sample count.

## Comparison principle

A result is only directly comparable to another result when the workload, measurement boundary, and environment are sufficiently similar.

For that reason, ZNode reports will avoid mixing:

- codec/service-only timing with end-to-end user timing;
- LAN and Internet latency without labeling network conditions;
- idle-resource figures with active-load figures;
- pilot observations with synthetic load tests;
- architectural targets with validated capacity.

## Initial figures awaiting full report

The first public repository snapshot records two facts already tracked internally:

- **1,500+ automated tests** in the current ZNode engineering suite;
- a **3 min 40 sec precompiled deployment example**.

These are intentionally marked as initial snapshot figures. Dedicated reports will provide environment, methodology, scope, and repeatability evidence before they are used as stronger performance claims.
