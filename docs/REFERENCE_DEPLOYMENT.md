# ZNode Reference Deployment Profile

This document defines the official ZNode Reference Profile for V1 metrics,
capacity notes and pilot reports. Unless another machine profile is explicitly
named, ZNode performance and reliability observations should be interpreted
against this baseline.

## Reference Profile

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

## Runtime Topology

The reference topology is intentionally narrow:

- Caddy owns public HTTPS and TLS.
- ZNode binds locally behind Caddy.
- FastAPI runs as a single worker.
- SQLite databases live on local durable SSD/NVMe storage.
- User files, previews, media artifacts and backups live under the local runtime root.
- z-connect runs as a managed sidecar for realtime signaling.
- The SFU runs as a managed sidecar for media rooms.
- coturn provides TURN relay when restrictive-network connectivity is required.

## Baseline Services

| Service | Role | Reference binding |
|---|---|---|
| `caddy` | HTTPS termination and reverse proxy | Public HTTPS ingress |
| `zetako-node.service` | Main ZNode FastAPI app | Local loopback upstream |
| `z-connect.service` | Signaling/control sidecar | Local loopback service |
| `z-connect-sfu.service` | SFU sidecar | Local control service; WebRTC media transport exposed only as required by deployment configuration |
| `coturn` | TURN relay | Public TURN/TURNS transport as configured |

Exact internal port assignments are intentionally omitted from the public reference profile. The important architectural property is that application and control services remain local/internal while Caddy owns the primary public HTTPS ingress.

## Production Constraints

- Do not run multiple FastAPI workers in this profile.
- Do not place SQLite databases on NFS, SMB, object storage mounts or other network filesystems.
- Do not present this profile as horizontally scalable.
- Use PostgreSQL, Redis, external object storage and separate workers only in a future non-V1 profile.
- Keep SFU production rooms within the supported V1 room capacity unless explicitly testing outside the supported profile.

## Metrics Reporting Rule

Every published benchmark or pilot observation should include:

- the profile name: `ZNode Reference Profile V1`
- exact CPU model or cloud instance type
- RAM size
- disk type and approximate free space
- OS version
- public network path or region
- whether coturn is same-host or external
- whether measurements are cold or warm/keep-alive

If any of these differ, report the result as a custom deployment profile rather
than the official reference profile.
