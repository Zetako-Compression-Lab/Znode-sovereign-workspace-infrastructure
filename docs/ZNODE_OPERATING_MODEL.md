# ZNode Operating Model

This document defines the ZNode V1 operating model for architecture, CTO/CISO review, pilot reporting and client release discussions. It consolidates the reference deployment, data/storage model, security model, backup/recovery model and release/support model.

It intentionally avoids database schema details, table names, columns, indexes, secret values, raw endpoint inventories and exploit-oriented procedures.

## 1. Reference Deployment Profile

Unless another machine profile is explicitly named, ZNode performance and reliability observations should be interpreted against this baseline.

| Area | Reference value |
|---|---|
| Profile name | ZNode Reference Profile V1 |
| Deployment shape | Single host, single node |
| Supported OS | Ubuntu 24.04 LTS or Debian 12 |
| CPU | 4 vCPU / 4 physical or virtual cores minimum |
| RAM | 8 GB minimum, 16 GB recommended for media-heavy pilots |
| Storage | Local SSD or NVMe; no network filesystem for SQLite |
| Network | Public HTTPS endpoint, stable broadband or datacenter uplink, 100 Mbps symmetric minimum recommended |
| GPU | Not required for the current profile |
| Application server | One FastAPI worker only |
| Database | Local SQLite on durable local storage |
| File storage | Local filesystem under the ZNode runtime root |
| Reverse proxy | Caddy terminates HTTPS and proxies to the app on loopback |
| Process supervision | `systemd` on Linux |
| Realtime signaling | Managed z-connect sidecar on loopback |
| Meetings media | Managed SFU sidecar |
| TURN | coturn, either same-host or external, depending on deployment mode |
| Horizontal scaling | Out of scope for this profile |

The reference topology is intentionally narrow:

- Caddy owns public HTTPS and TLS.
- ZNode binds locally behind Caddy.
- FastAPI runs as a single worker.
- SQLite databases live on local durable SSD/NVMe storage.
- User files, previews, media artifacts and backups live under the local runtime root.
- z-connect runs as a managed sidecar for realtime signaling.
- The SFU runs as a managed sidecar for media rooms.
- coturn provides TURN relay when restrictive-network connectivity is required.

## 2. Data And Storage Model

ZNode V1 uses a local metadata/binary split:

- SQLite stores identities, authorization state, metadata, references, lifecycle state, audit trails and policies.
- The local filesystem stores binary payloads such as uploaded files, media, previews and user media.
- SQLite records point to filesystem payloads through storage references.
- Ownership, access rules, integrity metadata, file descriptors and lifecycle state remain in SQLite.
- Backups must include both SQLite databases and corresponding data directories.

SQLite data families include local identity, sessions, permissions, messaging metadata, workspace metadata, server-storage descriptors, meeting/call metadata, audit events, compliance cases, legal holds, retention policies, settings and local operation state.

Filesystem payload families include uploads, message attachments, workspace files, user media, chat media and backup archives.

Binary payloads are not the authority for ownership by themselves. Authorized ZNode routes resolve payload access through local metadata and permission checks.

## 3. Sovereignty And Ownership

The V1 reference deployment is local-first and single-node:

- Runtime state is owned by the deployed ZNode host.
- SQLite databases and file payloads stay under the local runtime root.
- No external database is required for the current reference profile.
- No external object storage backend is part of the current reference profile.
- Browser-accessible payloads are served through ZNode authorization checks, not direct public filesystem exposure.
- Admin, user, group, workspace and compliance ownership are represented in the local application metadata layer.

If a deployment uses PostgreSQL, external object storage, Redis, multi-worker coordination or distributed filesystems, it is no longer the V1 local reference profile and must be documented as a custom profile.

## 4. Security Model

ZNode V1 is designed to protect user data, admin surfaces, support operations and future agent actions inside a bounded private-node deployment.

### Trust Boundaries

| Boundary | Trust decision |
|---|---|
| Public Internet to Caddy | Caddy terminates HTTPS and is the public edge |
| Caddy to ZNode app | App binds locally behind the reverse proxy |
| Browser to ZNode | Browser requests require sessions, CSRF protection where applicable and route-level authorization |
| ZNode to SQLite/filesystem | Local app process owns metadata and payload access |
| ZNode to z-connect | Realtime control is mediated by ZNode and scoped session credentials |
| z-connect to SFU | SFU is a local managed media sidecar, not the product authorization authority |
| Support channel | Customer-authorized, bounded operational access only |
| Agent integrations | Scoped bearer credentials resolved to visible agent principals when configured |

### Authentication And Sessions

ZNode uses server-side authentication and session management. Authenticated browser sessions are separate from agent bearer credentials and support credentials.

Session controls include server-side session state, secure-cookie operation in HTTPS deployments, CSRF validation for browser mutations, explicit logout, admin-managed session termination, restore-time session invalidation and role/permission checks at protected surfaces.

### Roles, Admin Isolation And ACL

ZNode separates user-facing and administrative duties through role and permission gates:

- members/users access collaboration surfaces according to permissions;
- operators access operational workflows without unrestricted admin authority;
- admins access user management, server operations, audit, compliance, settings and security controls;
- the Compliance Center has a dedicated permission and approval boundary;
- agent ledgers and confirmation queues are admin-session only.

Workspace folders and files are governed by local ownership and ACL metadata. Message attachments and workspace payloads resolve through authorized ZNode routes. Compliance exports are explicitly scoped and approval-driven. Legal Hold can block retention/deletion paths for protected objects.

### Rate Limiting And Audit

ZNode includes rate controls for sensitive flows such as login, message mutation and z-connect HTTP/WebSocket traffic. These controls reduce brute force, accidental overload and noisy client behavior in the single-node reference profile.

Audit records support admin review, login/security review, compliance state transitions, export access review, retention/deletion governance, agent execution attempts and denied/failed sensitive operations. Audit records should store safe summaries and metadata rather than raw sensitive payloads.

### Agent Principals

ZNode separates technical agent credentials from visible agent identity:

- agent integrations authenticate technical API access;
- visible agent principals provide local account identity;
- integrations can be linked to principals for auditability;
- disabled principals and revoked integrations fail closed;
- current agent execution remains read-only in the V1 readiness model;
- future sensitive/write actions are expected to use confirmation plus durable action ledger entries.

Agent bearer tokens cannot inspect administrative agent ledgers or confirmation queues.

### Support Access Boundary

ZNode support access is customer-controlled and bounded.

Where support capabilities are enabled, the design goal is that access remains explicit, scoped, locally validated, auditable and revocable. Support must not become arbitrary shell access, arbitrary filesystem access, raw data export or hidden operator access.

Normal client operation must not depend on continuous vendor connectivity.

### Protection Claims

ZNode V1 is intended to protect against unauthenticated route access, cross-user workspace/message access outside permissions, unauthorized admin surface access, CSRF on browser mutations, configured brute-force/noisy traffic patterns, accidental public filesystem exposure, silent/unapproved compliance exports, unscoped agent access and escalation of bounded support capabilities into unrestricted host control.

ZNode V1 does not claim multi-node high availability, protection after host OS or root compromise, upstream DDoS absorption, cryptographic secure erasure of deleted filesystem blocks, protection against legitimate host administrators, FIPS validation, autonomous write-capable agents without confirmation/audit, or hidden government access, master passwords or backdoors.

## 5. Backup And Recovery

ZNode stores metadata and payloads in two local layers:

- SQLite metadata and application state;
- local filesystem payloads and generated artifacts.

Backups and restores must preserve both layers from the same generation.

| Scope | Purpose | Includes |
|---|---|---|
| Runtime identity backup | Full node recovery | Runtime identity/config material, local SQLite databases and runtime data directories |
| Application-data backup | In-app operational restore | Application databases and application data directories, without full node identity material |
| Host/service configuration backup | Rebuild support for managed services | Service configuration, reverse proxy settings and sidecar/TURN configuration managed outside the runtime root |

The full recovery set should include node identity/config material, local SQLite database files, upload payload directories, messaging attachment payloads, workspace payloads, user media, chat media where applicable, backup status metadata and service configuration for z-connect, SFU and TURN when enabled.

If payload encryption is enabled, the matching operator-managed encryption key material must be recoverable. Without it, encrypted payloads cannot be read after restore.

### Consistency

The safest reference procedure is a cold backup during a short maintenance window:

1. stop application writes;
2. create the backup archive;
3. restart the application;
4. copy the archive off-host;
5. verify the archive can be read.

Online backups may be possible, but the official reference profile prefers cold backup for evidence-grade consistency between SQLite metadata and filesystem payloads.

### Restore Workflow

A controlled restore should:

1. identify the target backup generation;
2. stop ZNode services;
3. preserve the current runtime before replacement;
4. decrypt the archive if applicable;
5. restore identity/config material for full node recovery;
6. restore SQLite databases and data directories together;
7. restore or recreate service configuration for Caddy, z-connect, SFU and TURN;
8. start services;
9. validate health, login, storage, messaging, meetings and admin surfaces;
10. require users to sign in again after application-data restore.

### Verification

Restore validation should include service health checks, admin login, normal user login, sample message access, sample workspace listing/download, sample upload if writable, meeting/z-connect health if calls are enabled, audit page availability, backup status visibility and confirmation that old browser sessions no longer authenticate after application-data restore.

### RPO/RTO

RPO and RTO must be validated with timed restore drills before being published.

| Metric | Status |
|---|---|
| Restore time benchmark | Future benchmark required |
| Validated RPO | Future operational validation required |
| Validated RTO | Future operational validation required |
| Last restore drill | Must be recorded per pilot/client environment |

Until measured, ZNode should report backup/restore mechanisms, not guaranteed recovery times.

## 6. Retention, Legal Hold And Deletion

Retention is policy-driven. ZNode tracks retention policy scopes such as audit logs, security, messages, calls, meetings, calendar, workspace, system and debug logs.

Policies define archive behavior, flush mode, age thresholds and content mode. Automatic deletion is not assumed for every scope; unsupported or manual scopes remain dry-run/manual until a reliable persisted delete target exists.

Legal Hold is part of the governance model. When a hold is active, deletion and retention workflows must treat the held object as protected.

Deletion is lifecycle-aware:

- metadata deletion or soft deletion is recorded where history, auditability or user-visible tombstones are needed;
- payload deletion happens through metadata-resolved records;
- retention flushes use configured policy scopes and are audited;
- compliance exports and legal holds can block or preserve data that would otherwise be eligible for deletion.

Deletion should be reported as application-level removal and best-effort filesystem cleanup, not guaranteed secure erasure of underlying storage blocks.

## 7. Release And Support Model

### Release Channels

| Channel | Purpose | Expected use |
|---|---|---|
| Pilot Release | Controlled validation with selected pilot users or nodes | Feature proving, operational learning, telemetry validation and acceptance criteria |
| Client Release | Customer-facing supported release | Stable deployment, documented upgrade path and client-controlled adoption |
| Internal/Integration Build | Engineering validation only | Not presented as a supported client release |

Pilot observations should not be represented as general production benchmarks unless measured on the official reference profile and explicitly validated.

### Pilot Release

A Pilot Release may include limited feature flags, extra observability, tighter operator monitoring, explicit acceptance criteria, anonymized pilot usage reports and known limitations documented before deployment.

Pilot releases are suitable for learning and validation, not for broad capacity or SLA claims.

### Client Release

A Client Release should include release notes, upgrade instructions, rollback notes, compatibility notes, migration notes where applicable, backup recommendation before upgrade, support boundary statement and sanitized known limitations.

Client releases should avoid exposing internal branch names, experimental notes, debug-only configuration or implementation details that are not part of the supported product contract.

### Integrated Support

Integrated Support is customer-controlled and bounded. It may include health review, backup status review, service status review, sanitized diagnostics and guided upgrade/rollback support.

Support must not imply hidden access, arbitrary shell access, silent data export, unrestricted log collection, bypass of customer admin controls or support-side control without local verification and audit.

### Upgrade And Rollback

A controlled upgrade should confirm current and target releases, review notes, verify backup status, create or confirm a fresh backup, apply the upgrade, run health checks, verify core user/admin workflows and record the result.

Rollback readiness includes a known previous working release, preserved runtime backup, preserved service configuration, documented restore path, expected session invalidation behavior and post-rollback validation checklist.

If an upgrade includes irreversible data migration, release notes must state that rollback requires restoring from backup rather than simply reinstalling the previous version.

### Release Sanitization

Before a release is shared outside engineering, sanitize secrets, tokens, internal hostnames not meant for the client, raw logs, stack traces, development-only notes, test credentials, private customer data and internal branch/process details not relevant to support.

## 8. Client Control

The client/operator controls:

- release adoption timing;
- backup and restore approval;
- support access enablement;
- pilot telemetry participation;
- off-host backup custody;
- admin accounts and permissions;
- legal/compliance export decisions.

ZNode support can guide and automate bounded maintenance, but the client remains the owner of the sovereign node.

## 9. Reporting Rule

Every pilot or client deployment should be accompanied by:

- reference profile or custom deployment profile;
- release channel;
- release notes;
- backup status;
- rollback plan;
- support boundary;
- known limitations;
- acceptance criteria or validation checklist.

Every published benchmark or pilot observation should also include exact CPU or cloud instance type, RAM size, disk type, OS version, public network path or region, coturn mode and whether measurements are cold or warm/keep-alive.
