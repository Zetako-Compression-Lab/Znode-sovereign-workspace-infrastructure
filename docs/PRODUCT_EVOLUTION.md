# ZNode Product Evolution

ZNode did not begin as a public open-source project. The production code remains proprietary. This page reconstructs the product evolution from the private development history and publishes only high-level, non-sensitive engineering milestones.

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

## Foundation — self-hosted local-first node

The first engineering baseline already described ZNode as a **self-hosted, local-first node runtime** for organizations that wanted private messaging, file exchange and operator-controlled deployment without relying on an external chat transport.

The initial product foundation included or documented:

- local authentication and protected routes;
- role-based administrator, operator and member surfaces;
- direct messaging with local SQLite persistence;
- a structured Document Workspace;
- file transfer and server-side storage;
- a CLI-managed runtime lifecycle;
- shared installer foundations;
- Linux and Windows single-host deployment baselines;
- local reverse-proxy / HTTPS deployment patterns;
- backup and recovery tooling;
- early voice-call integration work.

An important architectural decision was already visible at this stage: messaging had moved away from an external Matrix/Synapse transport and into the ZNode runtime itself.

This foundation is therefore better understood as the beginning of the current product architecture, not as a minimal prototype.

---

## Deployment and cryptographic hardening

The product then expanded deployment tooling, safer development/runtime onboarding and clearer host boundaries.

An AEAD payload-encryption path was also introduced as part of the broader security evolution.

The important product lesson from this stage is that deployment and cryptographic boundaries were treated as architecture concerns rather than late-stage add-ons.

---

## Runtime stabilization and productization

ZNode evolved toward a more stable application runtime with dedicated work around RTC flows, session restoration, debugging, packaging and operator behavior.

Representative improvements included:

- group-chat MVP and call presence;
- theme and visual-configuration systems;
- operator theme preferences;
- session restoration and debugging tools;
- continued runtime and deployment hardening.

This marked the transition from a collection of communication capabilities toward a more unified application platform.

---

## Group collaboration becomes a core workflow

Group collaboration evolved from an initial group-chat implementation into a more complete organizational workflow.

Representative milestones include:

- group chat and presence;
- group creation integrated directly into messaging;
- operator-side group management;
- mentions;
- media-permission warm-up;
- group avatars;
- group call invitation and join behavior;
- theme-aware group communication surfaces.

The product was no longer just a direct-message system. It was becoming an organizational communication workspace.

---

## Branding and controlled organization themes

The theme system expanded from user preference into organization-level branding and policy-controlled presentation.

Engineering milestones included:

- multiple visual palettes;
- operator theme preferences;
- premium branded themes;
- company-specific themes;
- managed-theme behavior across operator surfaces;
- branded outbound communications;
- alignment between ZNode themes and organization branding.

This matters commercially because a self-hosted workspace can become part of the customer's own digital environment rather than looking like a generic third-party SaaS portal.

---

## Compliance Center and governance layer

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

## Agent-ready infrastructure

ZNode introduced an **agent-ready execution model** built around explicit control boundaries.

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

## SFU-based group communication matures

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

## Media processing becomes an operational subsystem

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

## Maintenance becomes a product capability

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

## Pilot operations and remote maintenance model

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

## Workspace evolves from storage toward collaboration

The Workspace continued moving beyond basic file storage.

Engineering milestones include:

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

## Meetings become a structured workflow

Meetings evolved from basic RTC behavior into a dedicated product workflow.

Milestones include:

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

## Security hardening continues across the stack

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

## Separate Client and Pilot release engineering

The product evolved toward distinct **client packaging** and **pilot packaging** paths.

The release engineering includes dedicated workflows, bootstrap/install paths and sanitization boundaries intended to distinguish customer-controlled deployments from pilot-specific operational tooling.

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
