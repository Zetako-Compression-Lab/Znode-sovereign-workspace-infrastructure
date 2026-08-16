# ZNode Security and Testing

ZNode's public security documentation is intended to provide evidence of engineering discipline without disclosing information that would weaken deployed systems.

## Current test-suite snapshot

ZNode currently tracks **more than 1,500 automated tests** across the product and its supporting services.

A detailed public breakdown will be added as the suite is classified for publication. Planned categories include:

- backend unit and service tests;
- API and integration tests;
- frontend tests;
- end-to-end user workflows;
- authentication tests;
- authorization and permission tests;
- policy and role regression tests;
- storage and file-operation tests;
- meeting and media tests;
- agent execution tests;
- confirmation and action-ledger tests;
- deployment and migration validation;
- security regression tests.

The raw test count is useful, but it is not sufficient by itself. Future reports will add execution scope, pass/fail status, release context, and category coverage.

## Security evidence planned for publication

Public defensive evidence may include:

- authentication and MFA behavior;
- session-management rules;
- role and permission boundaries;
- privilege-separation tests;
- guest-access restrictions;
- administrative action auditing;
- rate-limit and abuse-control behavior;
- dependency and package-security posture;
- security regression coverage;
- secure deployment guidance;
- incident and diagnostic logging behavior;
- agent scope and confirmation enforcement.

## Security publication boundary

Zetako will not publish material that would meaningfully reduce the security of real deployments.

Examples of information that remains private include:

- credentials, keys, tokens, or secrets;
- deployment-specific IPs or internal hostnames;
- exploitable configuration details;
- unpublished vulnerability reproduction steps;
- sensitive attack paths before remediation;
- customer-specific infrastructure information;
- internal defensive mechanisms where disclosure would create avoidable risk.

## Security testing philosophy

The public record should distinguish between:

### Functional security tests

Tests that confirm expected authorization and security behavior, such as rejecting an unauthorized action or enforcing a role boundary.

### Security regression tests

Tests added after a defect or security weakness is identified, so that the same class of failure cannot silently return.

### Adversarial testing

Deliberate attempts to break assumptions, bypass controls, abuse inputs, or force incorrect state transitions.

### External assessment

Where independent security reviews or penetration tests can be disclosed, they will be documented separately with scope, date, methodology, and remediation status.

## Agent security

Agent-capable infrastructure introduces a separate security surface. ZNode's public evidence will therefore cover:

- agent identity;
- tool scopes;
- execution authorization;
- action logging;
- confirmation requirements;
- rejection behavior for unauthorized tools or scopes;
- separation between human and agent principals.

## Future public reports

This document will eventually link to versioned evidence covering:

- test-suite snapshots;
- security test matrices;
- dependency audit summaries;
- permission-boundary validation;
- deployment-hardening guidance;
- security review summaries;
- remediation evidence where appropriate.
