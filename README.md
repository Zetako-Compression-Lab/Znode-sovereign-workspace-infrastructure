# ZNode

**Sovereign workspace infrastructure for organizations that need control over communications, collaboration, data, administration, and operational access.**

ZNode is Zetako's self-hosted workspace platform for private organizational communication and collaboration. It combines messaging, group spaces, meetings, voice/video, file storage, administrative controls, auditability, policy enforcement, compliance-oriented governance, and controlled support operations inside a deployable sovereign infrastructure.

This repository is the **public technical evidence layer for ZNode**.

It does **not** publish the proprietary production source code. Instead, it documents how ZNode is built, deployed, operated, measured, tested, secured, and governed. Over time this repository will contain reproducible benchmark reports, deployment measurements, capacity tests, SFU measurements, latency data, anonymized pilot observations, security-validation summaries, and product-engineering evidence.

**Zetako S.à r.l. · Luxembourg · [zetako.ai](https://zetako.ai/) · contact@zetako.ai**

---

## What ZNode is

ZNode is designed as a private workspace that an organization can operate under its own infrastructure and governance model.

Current product areas include:

- direct and group messaging;
- private group spaces;
- file storage and workspace collaboration;
- voice and video communication;
- meetings and meeting scheduling workflows;
- guest and external-user workflows where enabled;
- administrative roles and policy controls;
- audit and operational event records;
- compliance-oriented governance tooling;
- diagnostics and operator tooling;
- controlled support-access models;
- agent-ready infrastructure with scoped tool execution, action logging, and confirmation controls.

ZNode is developed as infrastructure rather than as a thin hosted front end. The public evidence in this repository is intended to make that distinction measurable.

---

## Public engineering snapshot

The repository is being opened while ZNode is already under active development and pilot use. The first public snapshot includes the following internally measured or tracked figures:

| Metric | Current public snapshot | Evidence status |
|---|---:|---|
| Automated tests | **1,500+** | Current internal suite count; detailed breakdown to be published |
| Precompiled deployment | **3 min 40 sec** | Measured deployment example; environment and methodology report to be published |
| Source availability | Proprietary | Public repository contains technical evidence and documentation only |
| Deployment model | Self-hosted / controlled infrastructure | Deployment profiles to be documented |
| Media architecture | SFU-based real-time communication | Capacity and resource benchmarks to be published |

These figures are **not universal guarantees**. Every performance number published by Zetako will be tied to its hardware, software version, workload, network conditions, measurement scope, and test methodology.

---

## What this repository will publish

### 1. Deployment evidence

ZNode deployment measurements will include:

- clean-host deployment time;
- deployment using precompiled artifacts;
- time to healthy services;
- restart and update behavior;
- host requirements;
- CPU, RAM, storage, and network prerequisites;
- deployment topology and service dependencies;
- rollback and recovery evidence where applicable.

The initial internally measured precompiled deployment example is **3 minutes 40 seconds**. A full report will document the exact host specification, included and excluded setup steps, artifact state, and health criteria.

See [`docs/DEPLOYMENT.md`](docs/DEPLOYMENT.md).

### 2. Performance and capacity

Future public reports will separate controlled benchmarks from real pilot observations.

Planned measurements include:

- API latency: p50 / p95 / p99;
- message-delivery latency;
- WebSocket connection and reconnection behavior;
- sustained messaging throughput;
- concurrent active users;
- file upload/download throughput;
- meeting creation and join latency;
- concurrent meeting-room capacity;
- SFU CPU and memory behavior;
- SFU bandwidth characteristics;
- media latency, jitter, and packet-loss observations where measurable;
- host CPU/RAM behavior at defined load levels.

See [`docs/METRICS_AND_BENCHMARKS.md`](docs/METRICS_AND_BENCHMARKS.md).

### 3. Pilot observations

ZNode pilots provide operational evidence that is different from a laboratory benchmark.

Where publication is appropriate and privacy-safe, this repository will publish **anonymized aggregate observations** such as:

- active users;
- sessions;
- messages;
- meetings;
- meeting minutes;
- file operations;
- storage use;
- API latency;
- request/error rates;
- CPU and memory utilization;
- service availability.

Pilot observations will always be labeled separately from controlled benchmark results.

---

## Deployment and operating modes

ZNode is designed to support different operational relationships between the customer and Zetako. The exact access rights and responsibilities depend on the deployment contract and configuration.

The public model distinguishes three operating modes:

### Client Mode

The organization operates ZNode as its own infrastructure. Zetako does not assume permanent operational access merely because the software is deployed.

Support access, when required, is intended to be explicit, controlled, and scoped to the support action being performed.

### Pilot Mode

A pilot deployment is operated for evaluation, validation, feedback, and engineering observation under agreed pilot conditions.

A pilot may permit Zetako to perform a broader set of operational and diagnostic actions than a fully customer-controlled production environment, but that access must remain defined by the pilot agreement and deployment configuration.

### Integrated Support Mode

Integrated support is intended for customers that explicitly choose an operational support relationship with Zetako.

The design goal is that **supportability should not require surrendering sovereignty**. Public documentation will describe how support access is authorized, scoped, logged, revoked, and separated from ordinary user activity.

See [`docs/OPERATING_MODES.md`](docs/OPERATING_MODES.md).

---

## Compliance Center and governance model

Sovereignty answers **where infrastructure and data are controlled**. Governance answers **how that environment is administered, evidenced, and reviewed**.

ZNode's Compliance Center is intended to centralize compliance-oriented operational controls and evidence around areas such as:

- access governance;
- administrative roles and permissions;
- policy configuration;
- auditability;
- security-event visibility;
- guest and external-user governance;
- operational evidence;
- agent governance where agent capabilities are enabled.

The conceptual model is:

```text
Policy
  ↓
Enforcement
  ↓
Evidence
  ↓
Review
```

ZNode does not claim that installing software automatically makes an organization compliant with a regulation or certification framework. Compliance depends on deployment, configuration, procedures, organizational responsibilities, legal context, and operational practice.

Public documentation will therefore distinguish between:

- controls ZNode provides;
- evidence ZNode can produce;
- configuration choices made by the customer;
- external compliance obligations that remain the customer's responsibility.

See [`docs/COMPLIANCE_CENTER.md`](docs/COMPLIANCE_CENTER.md).

---

## Security and testing

ZNode's public security documentation will focus on **security posture and validation evidence without publishing operational details that would weaken deployed systems**.

The current automated test suite contains **more than 1,500 tests**. Future public breakdowns will distinguish categories such as:

- backend unit/service tests;
- API and integration tests;
- frontend tests;
- end-to-end workflows;
- authentication and authorization tests;
- permission and policy regression tests;
- meeting/media tests;
- storage tests;
- agent execution and confirmation tests;
- security regression tests;
- deployment/migration validation.

Security publications will focus on defensive evidence: authentication controls, permission boundaries, session behavior, administrative isolation, auditability, regression coverage, dependency posture, and security-test methodology.

Sensitive exploit paths, credentials, internal secrets, and deployment-specific defensive details will not be published.

See [`docs/SECURITY_AND_TESTING.md`](docs/SECURITY_AND_TESTING.md).

---

## Agent-ready governance

ZNode includes an agent-ready architecture intended to let automated systems operate through explicit internal contracts rather than unrestricted application access.

The architecture includes concepts such as:

- scoped tool registries;
- authenticated agent integrations;
- execution gateways;
- visible agent principals;
- action ledgers;
- confirmation queues for sensitive actions.

The public evidence layer will document the governance and security model without exposing proprietary implementation details or credentials.

---

## How Zetako will publish metrics

Performance claims are easy to make misleading. ZNode therefore uses three labels for public measurements.

### MEASURED BENCHMARK

A controlled, reproducible test with named hardware, software version, workload, methodology, and measurement boundary.

### PILOT OBSERVATION

An anonymized observation from a real operating environment. Useful for understanding actual behavior, but not presented as a controlled benchmark.

### ARCHITECTURAL TARGET

A design objective, planned capacity, or expected scaling direction that has **not yet been validated as a measured result**.

Targets will never be presented as validated capacity.

---

## Planned evidence structure

```text
/
├── README.md
└── docs/
    ├── DEPLOYMENT.md
    ├── OPERATING_MODES.md
    ├── COMPLIANCE_CENTER.md
    ├── SECURITY_AND_TESTING.md
    └── METRICS_AND_BENCHMARKS.md
```

Future additions are expected to include dedicated reports for:

- deployment benchmarks;
- infrastructure footprint;
- API latency;
- messaging performance;
- SFU and meeting capacity;
- storage/file throughput;
- pilot telemetry snapshots;
- reliability observations;
- security-test summaries;
- test-suite evolution;
- version-to-version engineering changes.

---

## Public vs. private

### Public in this repository

- product architecture at a non-sensitive level;
- deployment model and operating modes;
- benchmark methodology;
- validated performance/capacity reports;
- anonymized pilot metrics;
- security-validation summaries;
- test-suite statistics;
- compliance/governance model;
- technical diagrams and evidence;
- version evolution and engineering milestones.

### Private / controlled distribution

- proprietary ZNode source code;
- production credentials and secrets;
- customer data;
- customer-specific deployment details;
- sensitive security implementation details;
- internal operational tooling that would weaken security if disclosed;
- unpublished security findings;
- protected commercial and customer information.

---

## Publication principle

ZNode's public technical record follows a simple rule:

> **Measured claims should be inspectable. Unmeasured claims should be labeled as targets. Security evidence should increase trust without reducing security.**

This repository will grow with the product. The objective is not to publish every internal implementation detail, but to provide customers, partners, engineers, security teams, and investors with enough technical evidence to understand what ZNode can do and how those claims were established.

---

## About Zetako

**Zetako S.à r.l.** is a Luxembourg deep-tech company developing sovereign software infrastructure and proprietary data-compression technologies.

**Website:** [zetako.ai](https://zetako.ai/)  
**Contact:** contact@zetako.ai  
**Location:** Luxembourg

---

<sub>Performance and capacity figures in this repository represent specific environments, product versions, workloads, and measurement scopes. They must be interpreted together with their associated methodology. Public documentation does not grant rights to reproduce, reverse engineer, or redistribute ZNode's proprietary implementation.</sub>
