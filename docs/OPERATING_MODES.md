# ZNode Operating Modes

ZNode can be deployed under different operational relationships between the customer and Zetako. The purpose of this document is to make those relationships explicit without assuming that every deployment uses the same support or access model.

The guiding principle is:

> **Supportability should not require surrendering sovereignty.**

## Client Mode

Client Mode is intended for organizations that want ZNode to operate as customer-controlled infrastructure.

In this model:

- the customer controls the deployment environment;
- Zetako does not assume permanent operational access simply because ZNode is installed;
- administrative authority remains with the customer according to the deployment and contractual model;
- any support access should be explicit, scoped, and revocable;
- customer data and operational responsibilities remain governed by the customer's own policies and deployment choices.

Public documentation will be expanded as access-control mechanisms are formally published.

## Pilot Mode

Pilot Mode is intended for evaluation, validation, product feedback, technical measurement, and operational learning.

Depending on the pilot agreement, Zetako may perform activities such as:

- deployment and update support;
- diagnostics;
- operational monitoring;
- performance measurement;
- issue investigation;
- agreed pilot telemetry collection;
- product validation and regression investigation.

Pilot access is not presented as equivalent to a production customer deployment. Exact responsibilities and allowed access depend on the pilot agreement and configuration.

Pilot metrics published by Zetako will be anonymized and separated from controlled benchmark results.

## Integrated Support Mode

Integrated Support Mode is intended for customers that explicitly choose a deeper operational support relationship.

The target model is that support access is:

- authorized;
- scoped;
- attributable;
- auditable;
- revocable;
- separated from ordinary end-user activity.

The final public documentation will describe the exact mechanisms available in supported ZNode releases, including what can be accessed, who can authorize access, how access is logged, and how it is removed.

## Access questions every deployment should answer

A deployment model should make the following points explicit:

1. Who controls the host infrastructure?
2. Who controls ZNode administrative roles?
3. Is Zetako operational access enabled at all?
4. If enabled, who authorizes it?
5. Is access permanent, time-limited, or session-specific?
6. What systems and data are in scope?
7. Are support actions recorded in audit logs?
8. How can access be revoked?
9. What telemetry leaves the customer environment, if any?
10. What obligations are contractual rather than technical?

## Public documentation principle

This repository will document supported access models accurately rather than implying that all deployments are identical.

Where a mechanism has not yet been publicly verified, it will be described as a design or operating principle rather than as a validated production guarantee.
