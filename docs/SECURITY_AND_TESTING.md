# ZNode Security and Testing

ZNode's public security documentation is intended to provide evidence of engineering discipline without disclosing information that would weaken deployed systems.

## Current test-suite snapshot

The current ZNode engineering suite contains **1,613 automated tests across 134 test files**.

Repository coverage includes:

- backend unit and service tests;
- API and integration tests;
- browser and frontend workflow tests;
- authentication and route-protection tests;
- authorization, ACL and permission tests;
- policy and role regression tests;
- storage and file-operation tests;
- meeting, RTC and media tests;
- agent registry, scope, ledger and confirmation tests;
- deployment and migration validation;
- security regression tests.

The current test count describes the automated suite itself. Versioned execution reports can separately publish full-suite pass/fail status, runtime, release context, and category coverage.

## Dependency security snapshot

A dependency audit was executed against the Python dependency set declared in `requirements.txt` using:

```sh
python3 -m pip_audit -r requirements.txt -f json
```

The scan reported **0 known dependency vulnerabilities** for the resolved dependency set at the time of the audit.

This is intentionally scoped. It does **not** mean that the complete application is guaranteed to contain no vulnerabilities; it means that the audit tool did not report a known vulnerability in that dependency set during the scan.

## Security controls represented in the current codebase

Current defensive surfaces include evidence of:

- authentication and protected routes;
- session lifetime and active-session limits;
- administrative access controls;
- inter-user and inter-group permission boundaries;
- workspace ACL and preview controls;
- login and mutation rate limiting;
- z-connect HTTP and WebSocket rate limits;
- audit and administrative event records;
- agent scope and execution controls;
- deployment/runtime hardening;
- regression tests around security-sensitive behavior.

## Security evidence planned for publication

Public defensive evidence may include:

- authentication behavior;
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

The public record distinguishes between:

### Functional security tests

Tests that confirm expected authorization and security behavior, such as rejecting an unauthorized action or enforcing a role boundary.

### Security regression tests

Tests added after a defect or security weakness is identified, so that the same class of failure cannot silently return.

### Adversarial testing

Deliberate attempts to break assumptions, bypass controls, abuse inputs, or force incorrect state transitions.

### External assessment

Where independent security reviews or penetration tests can be disclosed, they will be documented separately with scope, methodology, and remediation status.

## Agent security

Agent-capable infrastructure introduces a separate security surface. ZNode's current public agent model exposes **6 read-only tools** and **0 write tools** in the default registry.

The read-only tools are:

- `audit.search`;
- `calendar.list_events`;
- `meetings.list`;
- `messages.search`;
- `storage.search`;
- `users.list`.

Agent security evidence covers:

- agent identity;
- tool scopes;
- execution authorization;
- action logging;
- confirmation infrastructure;
- rejection behavior for unauthorized tools or scopes;
- separation between human and agent principals.

## Future public reports

This document will eventually link to versioned evidence covering:

- full test-suite execution snapshots;
- security test matrices;
- dependency audit summaries;
- permission-boundary validation;
- deployment-hardening guidance;
- security review summaries;
- remediation evidence where appropriate.
