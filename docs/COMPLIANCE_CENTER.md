# ZNode Compliance Center

ZNode's Compliance Center is intended to give administrators a structured view of governance, policy, security posture, and operational evidence inside a sovereign workspace deployment.

The objective is not to claim that software alone makes an organization compliant. The objective is to provide controls and evidence that can support an organization's own compliance and governance program.

## Current governance snapshot

The current Compliance Center model includes:

- **9 scoped exportable data types**;
- **6 defined compliance case states**;
- explicit `admin:compliance` authorization;
- second-administrator approval before export generation;
- retention-policy controls;
- legal-hold support;
- explicitly scoped exports;
- audit-trail recording for compliance state transitions.

The 9 scoped exportable data types currently represented are:

1. messages;
2. attachment metadata;
3. file binaries;
4. meetings;
5. calls;
6. calendar data;
7. workspace permissions;
8. audit events;
9. login events.

The 6 compliance case states are:

`draft → pending approval → approved → export ready → closed / rejected`

The public significance of this workflow is that compliance export is treated as a governed administrative action rather than an unrestricted data dump.

## Governance model

```text
Policy
  ↓
Enforcement
  ↓
Evidence
  ↓
Review
```

### Policy

Administrators define how the environment should behave through supported configuration, roles, permissions, access controls, retention or sharing rules where available, and operational policies.

### Enforcement

ZNode applies supported controls through the product's authorization, administration, session, workspace, storage, meeting, and agent-governance layers.

### Evidence

Relevant administrative, security, policy, login and operational events can be surfaced as evidence for investigation, review, or audit workflows where supported.

### Review

The Compliance Center is intended to make important governance information easier to inspect rather than leaving it distributed across independent product areas.

## Areas of governance

Public documentation progressively covers ZNode capabilities related to:

- identity and access governance;
- administrator and operator roles;
- permissions and policy enforcement;
- guest and external-user governance;
- audit events;
- security events;
- session and authentication posture;
- retention and legal hold;
- storage and sharing controls;
- meeting and communication governance;
- scoped data export;
- agent access, scopes, confirmations, and action records;
- operational diagnostics and evidence;
- configuration review.

## Compliance positioning

ZNode should not be interpreted as making a blanket statement such as "install ZNode and your organization is compliant."

Compliance depends on factors outside the software itself, including:

- deployment location;
- infrastructure ownership;
- organizational procedures;
- data classification;
- employee behavior;
- customer configuration;
- contracts;
- legal jurisdiction;
- incident-response practice;
- retention and deletion policy;
- external processors and integrations.

Where ZNode documentation references frameworks or regulations, the intended language is therefore:

- **supports controls relevant to...**
- **provides evidence useful for...**
- **can help implement...**

rather than claiming certification or automatic compliance unless a formal certification or assessment specifically supports that statement.

## Sovereignty and compliance

ZNode treats sovereignty and compliance as related but different concerns.

**Sovereignty** addresses who controls the infrastructure, data path, deployment, and operational boundaries.

**Compliance governance** addresses how that environment is configured, controlled, evidenced, and reviewed.

A sovereign deployment with poor governance can still be poorly controlled. A well-governed SaaS service may still not satisfy an organization's sovereignty requirements. ZNode is designed to address both dimensions.

## Agent governance

Where agent capabilities are enabled, governance becomes particularly important because automated actors can perform operations at machine speed.

ZNode's current agent-ready architecture includes:

- explicit agent principals;
- **6 read-only tools** in the current registry;
- **0 write tools** enabled by default;
- scoped tool permissions;
- authenticated integrations;
- execution controls;
- action ledgering;
- confirmation-queue foundations for sensitive actions.

The design principle is that an automated actor should remain identifiable, scoped and auditable rather than bypassing ordinary governance.

## Future evidence

As the public repository grows, this document will link to validated evidence covering:

- available policy controls;
- audit-event taxonomy;
- governance screenshots or diagrams;
- security-event categories;
- agent-governance evidence;
- administrative review workflows;
- timed export/report measurements;
- framework-control mappings where they can be made accurately.
