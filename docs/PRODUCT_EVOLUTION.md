# ZNode Product Evolution

ZNode did not begin as a public open-source project. The production code remains proprietary. This page reconstructs the product evolution from the private development history and publishes only high-level, non-sensitive engineering milestones.

As of the current public review, the main development history contains **867 commits**, beginning on **26 April 2026** with `Initial commit - Zetako Node`.

The purpose of this page is not to expose implementation detail. It is to show how the product evolved from a self-hosted communication node into a broader sovereign workspace and operational infrastructure platform.

---

## Evolution at a glance

```text
self-hosted local-first node
        ↓
private messaging + file exchange
        ↓
structured workspace
        ↓
group collaboration
        ↓
voice / video / meetings
        ↓
SFU-based real-time communication
        ↓
policy + audit + compliance governance
        ↓
agent-ready controlled execution
        ↓
maintenance + pilot operations
        ↓
client / pilot release packaging
        ↓
sovereign workspace infrastructure
```

---

## April 2026 — Initial ZNode baseline

The first commit in the current repository history is dated **26 April 2026**.

The initial engineering baseline already described ZNode as a **self-hosted, local-first node runtime** for organizations that wanted private messaging, file exchange and operator-controlled deployment without relying on an external chat transport.

The first repository snapshot already included or documented:

- local authentication and protected routes;
- role-based administrator, operator and member surfaces;
- direct messaging with local SQLite persistence;
- a structured Document Workspace V1;
- file transfer and server-side storage;
- a CLI-managed runtime lifecycle;
- shared installer foundations;
- Linux and Windows single-host deployment baselines;
- local reverse-proxy / HTTPS deployment patterns;
- backup and recovery tooling;
- early voice-call integration work.

An important architectural decision was already visible at this stage: messaging had moved away from an external Matrix/Synapse transport and into the ZNode runtime itself.

This initial baseline is therefore better understood as the start of the current engineering history, not as a minimal prototype.

---

## Late April 2026 — Deployment and cryptographic hardening

Immediately after the initial baseline, engineering work expanded deployment tooling and safer development/runtime onboarding.

The same period also introduced an AEAD payload-encryption path as part of the broader security evolution.

The public lesson from this phase is that deployment and cryptographic boundaries were being treated as product architecture concerns very early in the current repository history.

---

## May 2026 — Runtime stabilization and productization

By early May, the repository shows dedicated work around stabilizing the **ZNode v2 runtime and RTC flows**.

The product then expanded rapidly across several areas:

- group-chat MVP and call presence;
- theme and visual-configuration systems;
- operator theme preferences;
- session restoration and debugging tools;
- continued runtime and deployment hardening.

This phase marks the transition from a collection of communication capabilities toward a more unified application platform.

---

## May–June 2026 — Group collaboration becomes a core workflow

Group collaboration evolved from an initial group-chat implementation into a more complete workflow.

Representative milestones include:

- group chat MVP and presence;
- group creation integrated directly into messaging;
- operator-side group management;
- mentions;
- media-permission warm-up;
- group avatars;
- group call invitation and join behavior;
- theme-aware group communication surfaces.

The product was no longer just a direct-message system. It was becoming an organizational communication workspace.

---

## June–July 2026 — Branding and controlled organization themes

The theme system expanded from user preference into organization-level branding.

Engineering milestones included:

- multiple visual palettes;
- operator theme preferences;
- premium branded themes;
- company-specific themes;
- managed-theme behavior across operator surfaces;
- branded outbound communications;
- alignment between ZNode themes and organization branding.

This work matters commercially because a self-hosted workspace can become part of the customer's own digital environment rather than looking like a third-party SaaS portal.

---

## July 2026 — Compliance Center and governance layer

A major product milestone was the introduction of the **Admin Compliance Center**.

From this point, ZNode increasingly treats administration as governance rather than simple configuration.

The broader direction includes:

- policy visibility;
- retention-policy enforcement;
- audit evidence;
- security-policy controls;
- administrative access controls;
- compliance state presentation;
- background policy and alert workers.

The public governance model is:

```text
Policy → Enforcement → Evidence → Review
```

ZNode does not claim that software alone makes an organization compliant. The objective is to provide technical controls and evidence that can support an organization's compliance and governance program.

See [`COMPLIANCE_CENTER.md`](COMPLIANCE_CENTER.md).

---

## July 2026 — Agent-ready infrastructure

In July 2026, ZNode introduced an **agent-ready execution model** built around explicit control boundaries.

Publicly describable milestones include:

- a scoped internal tool registry;
- controlled agent execution interfaces;
- action logging / ledgering;
- visible agent principals;
- confirmation queues for actions requiring approval;
- classification of agent API routes and permissions.

The design principle is important:

> An agent should be a visible, scoped and auditable actor inside ZNode — not an invisible bypass around normal access controls.

This creates a foundation for future AI-assisted workflows while keeping governance inside the node.

---

## July 2026 — SFU-based group communication matures

Real-time communication evolved from early call integration into a more structured RTC architecture.

Engineering work included:

- centralized RTC session presentation;
- persistent in-app call / meeting state;
- SFU-based group communication;
- explicit remote-track handling;
- stabilization of group-call audio;
- participant-aware media routing;
- quality diagnostics and recovery behavior.

This is the basis for the SFU performance and capacity reports that will be published separately in this repository.

---

## August 2026 — Media processing becomes an operational subsystem

Media handling was progressively separated into dedicated execution paths instead of remaining incidental request processing.

Milestones include:

- media execution planning;
- FFmpeg execution paths;
- leased media workers;
- leased scheduling;
- progress and cancellation handling;
- adaptive execution budgets;
- explicit worker limits;
- dedicated regression tests.

The architectural direction is to keep expensive media work **bounded, observable and recoverable**.

---

## August 2026 — Maintenance becomes a product capability

A dedicated **Admin Maintenance Control Center** was introduced and expanded.

Engineering work added:

- service-health signals;
- managed-service controls;
- permission-aware maintenance operations;
- job recovery;
- backup-state warnings;
- diagnostic behavior;
- operational action hierarchy.

This is an important evolution in the ZNode model: a sovereign node must not only run independently; it must also expose enough operational state to be understood and maintained safely.

---

## August 2026 — Pilot operations and remote maintenance model

Pilot deployment work introduced a separate operational model for evaluation environments.

Representative milestones include:

- pilot remote-maintenance tooling;
- pilot enrollment;
- command-agent recovery;
- runtime scheduling and enrollment hardening;
- non-blocking pilot bootstrap;
- pilot telemetry administration;
- certificate-expiry telemetry.

These capabilities form part of the engineering basis for the public distinction between:

- **Client Mode**;
- **Pilot Mode**;
- **Integrated Support Mode**.

See [`OPERATING_MODES.md`](OPERATING_MODES.md).

---

## August 2026 — Workspace evolves from storage toward collaboration

The Workspace continued moving beyond basic file storage.

Recent engineering milestones include:

- file previews;
- browser-style workspace navigation;
- desktop drag-and-drop upload;
- list and grid views;
- read-only preview flows;
- folder sharing;
- internal workspace sharing;
- stronger move and permission coverage;
- continued desktop and mobile interaction refinement.

The direction is toward a controlled organizational workspace rather than a simple storage bucket.

---

## August 2026 — Meetings become a structured workflow

Meetings also evolved from basic RTC behavior into a dedicated product workflow.

Recent milestones include:

- guided meeting creation;
- scheduled meetings;
- recurring meeting support;
- calendar integration;
- responsive meeting/calendar panels;
- screen sharing;
- mobile meeting scheduling;
- administrator-controlled media-quality settings;
- quality-degradation detection and alerts.

Future public reports will connect these product capabilities to measurable SFU and latency evidence.

---

## August 2026 — Security hardening continues across the stack

Security is not represented as a single completed milestone.

The development history contains repeated hardening work, including:

- security-plan implementation and follow-up fixes;
- authentication and route-protection regression coverage;
- SFU-sidecar audit remediation;
- workspace permission hardening;
- rate-limit separation for authenticated real-time traffic;
- deployment-boundary testing;
- continued security regression testing.

The public repository will publish methodology and aggregate validation evidence while avoiding exploit-enabling implementation detail.

See [`SECURITY_AND_TESTING.md`](SECURITY_AND_TESTING.md).

---

## Current engineering direction — separate Client and Pilot releases

The current development tree contains distinct **client packaging** and **pilot packaging** branches.

The release engineering includes dedicated workflows, bootstrap/install paths and sanitization boundaries intended to distinguish customer-controlled deployments from pilot-specific operational tooling.

These branches are still active engineering work, so this public repository describes them as a **current engineering direction**, not as a universal release guarantee.

The important evolution is the emergence of explicit deployment profiles:

### Client deployment

Production-oriented, customer-controlled node package.

### Pilot deployment

Evaluation-oriented package with explicitly scoped pilot operations and telemetry.

### Integrated support

A controlled support relationship whose permissions depend on configuration and agreement rather than an assumption of permanent vendor access.

---

## From communication product to sovereign infrastructure

The development history shows a consistent broadening of the product boundary:

```text
private communications
        ↓
collaboration
        ↓
real-time media
        ↓
workspace
        ↓
governance
        ↓
agent control
        ↓
operations
        ↓
release engineering
```

The current ZNode direction can therefore be summarized as:

> **Sovereign workspace infrastructure that combines communication, collaboration, governance and operational control inside a deployable customer-controlled node.**

---

## Publication note

Zetako's public GitHub presence is recent. ZNode's engineering history is not.

This public repository is intended to expose selected **evidence of product maturity and evolution** while the proprietary implementation remains private.

Future revisions of this page will add validated release milestones, test-suite growth, deployment-time evolution, performance improvements and pilot measurements as those figures are prepared for publication.
