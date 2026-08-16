# ZNode Live Latency Baseline

This report records an external latency baseline for the public development deployment before infrastructure/network optimization.

The purpose is to separate **end-to-end public request latency** from **time actually spent inside the ZNode application**.

The baseline is intentionally preserved so future infrastructure changes can be compared using the same measurement boundary.

## Scope

Target deployment: `https://dev.zetako.ai`

Measurement method:

- unauthenticated public endpoints only;
- 20 cold requests per endpoint;
- `/usr/bin/curl`;
- response headers captured;
- DNS, TCP, TLS, first-byte and total timings recorded;
- backend processing time taken from `X-Response-Time-Ms`;
- database time taken from ZNode database timing headers where applicable.

These measurements are **external observations of the full request path**. They are not backend-only FastAPI benchmarks.

## Key result

The measured public latency is overwhelmingly outside the ZNode application process.

| Endpoint | HTTP | Total p50 | ZNode backend p50 | Outside application p50 | SQLite / DB p50 | Interpretation |
|---|---:|---:|---:|---:|---:|---|
| `GET /healthz` | HTTP/2 `200` | **2,304.53 ms** | **1.14 ms** | **2,303.48 ms** | **0.00 ms** | Application health path is effectively immediate; latency is dominated by the external connection path |
| `GET /login` | HTTP/2 `200` | **1,191.04 ms** | **21.53 ms** | **1,170.84 ms** | **8.02 ms** | ZNode backend and SQLite are a small fraction of observed wall time |

This distinction is important when interpreting public latency numbers. A 1–2 second cold external request does **not** mean ZNode spends 1–2 seconds processing that request.

## Timing decomposition

### `GET /healthz`

| Phase | p50 | p95 | p99 |
|---|---:|---:|---:|
| DNS lookup | 3.41 ms | 4.23 ms | 4.26 ms |
| TCP connect after DNS | 796.31 ms | 1,079.06 ms | 1,250.39 ms |
| TLS handshake after TCP | 1,193.35 ms | 2,113.60 ms | 2,290.85 ms |
| Post-TLS wait to first byte | 235.39 ms | 960.87 ms | 1,076.38 ms |
| Download/body transfer | 0.96 ms | 1.03 ms | 1.10 ms |
| Total wall time | **2,304.53 ms** | **3,304.15 ms** | **3,441.99 ms** |
| ZNode backend `X-Response-Time-Ms` | **1.14 ms** | **1.49 ms** | **1.53 ms** |

The health endpoint performs no database work in this baseline. Optimizing ZNode application logic would therefore have negligible impact on the observed cold external latency.

### `GET /login`

| Phase | p50 | p95 | p99 |
|---|---:|---:|---:|
| DNS lookup | 3.56 ms | 3.94 ms | 4.06 ms |
| TCP connect after DNS | 251.45 ms | 597.99 ms | 743.79 ms |
| TLS handshake after TCP | 357.98 ms | 1,587.41 ms | 1,799.20 ms |
| Post-TLS wait to first byte | 238.30 ms | 300.93 ms | 313.10 ms |
| Download/body transfer | 194.18 ms | 305.30 ms | 305.32 ms |
| Total wall time | **1,191.04 ms** | **2,586.68 ms** | **2,986.22 ms** |
| ZNode backend `X-Response-Time-Ms` | **21.53 ms** | **29.47 ms** | **30.40 ms** |

SQLite/database work on this request measured approximately **8.02 ms p50**. Within ZNode's supported single-node architecture, this baseline does not identify SQLite as the dominant latency source.

## Where the time is going

The first optimization target is the public connection path rather than FastAPI application processing.

Current priorities are:

1. **TCP/TLS path** — routing, host region, TLS terminator, certificate chain, OCSP behavior and handshake cost.
2. **Edge/proxy placement** — reduce distance between users and the TLS/public ingress boundary.
3. **HTTP/3 validation** — the server advertises HTTP/3 support; compare QUIC against the HTTP/2 cold-request baseline.
4. **Connection reuse** — compare cold handshakes with warm keep-alive/session-resumed requests.
5. **Login application polish** — optional optimization of the ~21.53 ms backend p50 and ~8.02 ms DB p50 after infrastructure latency is addressed.

## Baseline for future comparison

This report should not be overwritten after optimization.

Future measurements should preserve the same endpoint and methodology where possible and publish a comparison such as:

| Endpoint | Baseline total p50 | Optimized total p50 | Baseline backend p50 | Optimized backend p50 | Total improvement |
|---|---:|---:|---:|---:|---:|
| `/healthz` | 2,304.53 ms | TBD | 1.14 ms | TBD | TBD |
| `/login` | 1,191.04 ms | TBD | 21.53 ms | TBD | TBD |

The comparison should also track p95/p99 and connection failures so improvements in reliability are visible alongside latency improvements.

## Interpretation boundary

This report measures one development deployment from one external test path using cold requests.

It does not establish:

- global user latency;
- warm-connection latency;
- authenticated workflow latency;
- application throughput;
- SFU/media capacity;
- production SLA or availability.

Those measurements are tracked separately under the broader ZNode benchmark program.

---

**Summary:** the initial external baseline shows that ZNode application processing is fast on the measured public endpoints — approximately **1.14 ms p50** for `/healthz` and **21.53 ms p50** for `/login` — while the dominant latency currently sits in the network/TCP/TLS/public ingress path.