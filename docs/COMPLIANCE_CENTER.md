# ZNode Compliance Center

ZNode's Compliance Center is intended to give administrators a structured view of governance, policy, security posture, and operational evidence inside a sovereign workspace deployment.

The objective is not to claim that software alone makes an organization compliant. The objective is to provide controls and evidence that can support an organization's own compliance and governance program.

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

Relevant administrative, security, policy, and operational events can be surfaced as evidence for investigation, review, or audit workflows where supported.

### Review

The Compliance Center is intended to make important governance information easier to inspect rather than leaving it distributed across independent product areas.

## Areas of governance

Public documentation will progressively cover ZNode capabilities related to:

- identity and access governance;
- administrator and operator roles;
- permissions and policy enforcement;
- guest and external-user governance;
- audit events;
- security events;
- session and authentication posture;
- storage and sharing controls;
- meeting and communication governance;
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

ZNode's agent-ready architecture includes concepts such as:

- explicit agent principals;
- scoped tool permissions;
- authenticated integrations;
- execution gateways;
- action ledgers;
- confirmation queues for actions that require human approval.

Future public documentation will show how those controls are represented in the Compliance Center and audit model without exposing sensitive implementation details.

## Future evidence

As the public repository grows, this document will link to validated evidence covering:

- available policy controls;
- audit-event taxonomy;
- governance screenshots or diagrams;
- security-event categories;
- agent-governance evidence;
- administrative review workflows;
- export/report capabilities where supported;
- framework-control mappings where they can be made accurately.
