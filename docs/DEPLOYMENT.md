# ZNode Deployment

ZNode is designed to be deployable as customer-controlled infrastructure. This document defines the public evidence Zetako intends to publish around deployment speed, host requirements, service readiness, and operating footprint.

## Initial deployment snapshot

An internally measured deployment using precompiled artifacts completed in **3 minutes 40 seconds**.

This is an initial public snapshot, not yet a universal deployment claim. A dedicated benchmark report will publish:

- host CPU and RAM;
- storage type and capacity;
- operating system;
- network conditions;
- ZNode version/build;
- whether package installation was already complete;
- what was included in the timed path;
- what was excluded, such as DNS propagation or external provisioning;
- health criteria used to declare the deployment complete;
- repeated-run measurements.

## Deployment measurements planned for publication

### Clean-host deployment

Time from a documented baseline host state to a healthy ZNode environment.

### Precompiled deployment

Time required when application artifacts are already built and deployment does not need to compile the full application stack on the target host.

### Service readiness

Time between deployment start and successful health validation for the services required by the measured profile.

### Restart

Time required to restart the ZNode stack and restore healthy service state.

### Upgrade

Time and operational behavior when moving between supported versions.

### Recovery

Controlled recovery measurements where relevant, including service restart behavior after failure scenarios.

## Host profile

Future reports will document:

- vCPU count;
- RAM;
- storage;
- operating system;
- architecture;
- network requirements;
- optional or required acceleration;
- observed idle resource footprint;
- observed resource footprint under defined load.

## CPU and GPU

ZNode benchmark reports will explicitly state which functions are CPU-based and whether any measured workflow requires GPU acceleration.

A hardware capability will not be implied merely because it exists in a host. If a benchmark does not use a GPU, that will be stated.

## Media and SFU deployment

Real-time meeting workloads have different resource characteristics from messaging or file operations. SFU tests will therefore be documented separately with:

- participant count;
- room count;
- media configuration;
- CPU;
- RAM;
- network ingress/egress;
- observed latency/jitter/packet loss where measurable;
- host specification.

## Deployment boundary

A fast application deployment does not automatically include every activity necessary to launch a production environment.

Reports will state whether the timer includes or excludes items such as:

- server procurement;
- DNS propagation;
- certificate issuance;
- firewall preparation;
- external backup provisioning;
- customer identity-system preparation;
- application compilation;
- database initialization;
- health validation.

This boundary is necessary so deployment figures remain comparable and meaningful.
