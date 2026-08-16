# ZNode Data And Storage Model

This document explains how ZNode stores data in the V1 local-first profile. The goal is to make the SQLite, filesystem and sovereignty model explicit for architecture reviews, compliance reviews and benchmark reports.

## Model Summary

ZNode V1 uses a local metadata/binary split:

- SQLite stores identities, authorization state, metadata, references, lifecycle state, audit trails and policies.
- The local filesystem stores binary payloads such as uploaded files, media, previews and user media.
- SQLite records point to filesystem payloads through storage keys or paths.
- Ownership, access rules, integrity metadata, file descriptors and lifecycle state remain in SQLite.
- Backups must include both SQLite databases and the corresponding data directories, otherwise the node can lose either metadata or payloads.

## Sovereignty And Ownership

The V1 reference deployment is local-first and single-node:

- Runtime state is owned by the deployed ZNode host.
- SQLite databases and file payloads stay under the local runtime root.
- No external database is required for the current reference profile.
- No external object storage backend is part of the current reference profile.
- Browser-accessible payloads are served only through ZNode authorization and routing checks, not through direct public filesystem exposure.
- Admin, user, group, workspace and compliance ownership are represented in the local application metadata layer.

## SQLite Data

SQLite is the system of record for operational metadata and application state. This document intentionally describes data families only; it does not document the database schema, table names, columns, indexes or query layout.

| Data family | Stored in SQLite |
|---|---|
| Users and auth | Local identity, role, permission and session state |
| Messaging | Conversation, message, delivery and attachment metadata |
| Workspace | Folder, file, ownership and permission metadata |
| Server storage | Stored file descriptors, integrity metadata and local payload references |
| Meetings and calls | Meeting, call and room lifecycle metadata |
| z-connect state | Local realtime control state for the single-node sidecar profile |
| Audit | Operational, security, admin and compliance event metadata |
| Compliance | Case lifecycle, approval state, legal holds and export metadata |
| Retention policies | Archive and deletion policy configuration |
| Jobs and operations | Local backup, restore, pruning and media operation state |
| Settings | Branding, policy, integration and managed configuration state |

SQLite should remain on local durable SSD/NVMe storage. Do not place ZNode SQLite files on NFS, SMB, object storage mounts or other network filesystems in the V1 profile.

## Filesystem Payloads

The filesystem stores payload bytes and generated artifacts. Important runtime paths include:

| Runtime path | Contents |
|---|---|
| `data/uploads/` | General upload staging/application upload payloads |
| `data/messaging_files/` | File-transfer payloads attached to messages |
| `data/workspace_files/` | Workspace file payloads |
| `data/user_media/` | User/avatar/public media assets |
| `data/chat_media/` | Chat media inspection, preview and processing payloads |
| Backup archive directory | Runtime or application-data backup archives and local backup job logs |

Binary payloads are not the authority for ownership by themselves. The authoritative ownership and access-control fields live in SQLite, and payload reads must resolve through those metadata records.

## Metadata / Binary Separation

ZNode intentionally separates metadata from binary content:

| Concern | SQLite metadata | Filesystem payload |
|---|---|---|
| Identity | Local ownership and actor references | None |
| Access control | Permissions, policy decisions and legal hold state | None |
| File description | File descriptors and payload references | Actual bytes |
| Integrity | Integrity and encoding metadata | Bytes validated or served |
| Lifecycle | Creation, update, deletion and operation state | Payload exists, is removed, or is regenerated |
| Compliance | Case scope, hold state and export metadata | Export archive and scoped file binaries |

This split lets ZNode answer governance questions from SQLite while keeping large payloads on disk.

## Backup

Backups are sensitive operational data. A usable runtime identity backup must include both identity/config material and data:

- `config/node.json`
- `config/secrets.json`
- `config/network_key.json`
- local SQLite database files
- `data/uploads/`
- `data/messaging_files/`
- `data/workspace_files/`
- `data/user_media/`

Application-data backups from the Server admin page include application databases and data directories such as messages, uploads, workspace files, user media and chat media. They do not include full runtime identity files or host-level service environment files.

If payload encryption is enabled, the matching operator-managed payload encryption key is required to recover encrypted payloads. Losing the key means encrypted file-transfer, server-storage and workspace payloads cannot be recovered.

## Restore

Restore must preserve the metadata/payload pairing:

- Restore SQLite databases and data directories from the same backup generation.
- Restore runtime identity material for full node recovery.
- Restore or recreate z-connect/TURN service configuration when calls are enabled.
- After application-data restore, old browser authentication state is invalidated so users must sign in again.
- Validate the restored node with the runtime status/check commands before returning the service to normal use.

## Retention

Retention is policy-driven. ZNode tracks retention policy scopes for:

- audit logs
- security
- messages
- calls
- meetings
- calendar
- workspace
- system
- debug logs

Policies define archive behavior, flush mode, age thresholds and content mode such as metadata-only, metadata-preview or full-content. Automatic deletion is not assumed for every scope; unsupported or manual scopes remain dry-run/manual until a reliable persisted delete target exists.

## Legal Hold

Compliance legal holds are stored in SQLite and are part of the governance model. Holds can apply to scoped objects such as compliance cases and regulated content objects. When a hold is active, deletion/retention workflows must treat the held object as protected.

Filesystem deletion is documented as best-effort cleanup, not cryptographic secure erasure.

## Deletion

Deletion is lifecycle-aware rather than a blind filesystem unlink:

- Metadata deletion or soft deletion is recorded in SQLite where the product needs history, auditability or user-visible tombstones.
- Workspace payload deletion removes the filesystem bytes only after resolving the SQLite file record.
- Workspace folder deletion rolls back removed payloads if a later payload removal fails during the same operation.
- Retention flushes use configured policy scopes and are audited.
- Compliance exports and legal holds can block or preserve data that would otherwise be eligible for deletion.

Deletion should be reported as application-level removal and best-effort filesystem cleanup, not guaranteed secure erasure of underlying storage blocks.

## Compliance Exports

The Compliance Center exports only explicitly scoped data. Supported data types include:

- messages
- attachment metadata
- file binaries
- meetings
- calls
- calendar
- workspace permissions
- audit events
- login events

Compliance cases track lifecycle, approval state, evidence integrity metadata and event history in SQLite. The generated export artifact is a filesystem payload and must be protected as sensitive data.

## Operational Rule

For every production or pilot report, describe data handling as:

> ZNode V1 stores governance and application metadata in local SQLite and stores binary payloads on the local filesystem under the runtime root. Backups, restores, retention and deletion must operate on both layers together.

If a deployment uses PostgreSQL, external object storage, Redis, multi-worker coordination or distributed filesystems, it is no longer the V1 local reference storage model and must be documented as a custom profile.
