# ZNode Live Latency Baseline

This report records an external latency baseline for the public development deployment before infrastructure/network optimization.

The purpose is to separate **end-to-end public request latency** from **time actually spent inside the ZNode application**.

The baseline is intentionally preserved so future infrastructure changes can be compared using the same measurement boundary.

## Scope

Target deployment: `https://dev.zetako.ai`

Measurement method:

- unauthenticated public endpoints only;
- 20 cold requests per endpoint for the original baseline;
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

## Optimization work applied after the baseline

The first optimization pass deliberately focused on low-risk changes that improve repeated page access and connection reuse without changing ZNode's core architecture.

| Area | Change | Expected effect |
|---|---|---|
| Static assets | `/static`, public avatar media, and public group avatar media now emit `Cache-Control: public, max-age=31536000, immutable`; deployment proxy configuration applies the same policy to those paths | Repeat page loads avoid unnecessary static transfers and validations |
| Reverse-proxy upstream | Caddy deployment baselines now configure upstream keep-alive with `keepalive 2m` | Reduces connection churn between the TLS terminator and the local application runtime |
| Login settings reads | Login branding and theme settings are obtained through one login-surface settings read rather than separate reads | Reduces database work on `GET /login`; expected gain is small compared with network/TLS cost |
| Regression safety | Static cache behavior and targeted login/health/settings/browser tests were added/rerun | Confirms that optimization changes preserve intended application and security behavior |

These changes should not be interpreted as a completed performance campaign. They are the first controlled improvements applied after the measurement decomposition identified the dominant latency boundary.

## Cold vs warm HTTP/2 observation

A follow-up observation compared new cold connections with requests that reused the same HTTP/2 connection from the same test machine.

This is a separate observation from the 20-request baseline above and uses **8 samples per mode**.

| Endpoint | Mode | Samples | p50 | p95 | Avg | Min | Max |
|---|---|---:|---:|---:|---:|---:|---:|
| `GET /healthz` | Cold, separate curl processes | 8 | **1,384.53 ms** | 3,364.19 ms | 1,717.80 ms | 1,069.02 ms | 3,364.19 ms |
| `GET /healthz` | Warm, same curl process / connection reuse | 8 | **222.08 ms** | 2,075.98 ms | 465.67 ms | 221.27 ms | 2,075.98 ms |
| `GET /login` | Cold, separate curl processes | 8 | **2,595.61 ms** | 4,174.02 ms | 2,731.58 ms | 1,775.17 ms | 4,174.02 ms |
| `GET /login` | Warm, same curl process / connection reuse | 8 | **858.89 ms** | 3,074.41 ms | 921.42 ms | 220.04 ms | 3,074.41 ms |

### Interpretation

Connection reuse materially improves the observed public path, especially for `/healthz`.

For the median sample in this observation:

- `/healthz` improved from **1,384.53 ms cold** to **222.08 ms warm**;
- `/login` improved from **2,595.61 ms cold** to **858.89 ms warm**.

These samples should **not** be treated as a direct before/after product benchmark because they use a smaller follow-up sample set and different connection behavior. Their purpose is diagnostic: they reinforce the conclusion that connection establishment, TLS, and routing distance dominate the current public request path.

The local curl build used for this test supports HTTP/2 but not HTTP/3. A QUIC/HTTP/3 comparison remains a separate future measurement.

## Where the time is going

The first optimization target remains the public connection path rather than FastAPI application processing.

Current priorities are:

1. **TCP/TLS path** — routing, host region, TLS terminator, certificate chain, OCSP behavior and handshake cost.
2. **Edge/proxy placement** — reduce distance between users and the TLS/public ingress boundary.
3. **HTTP/3 validation** — the server advertises HTTP/3 support; compare QUIC against the HTTP/2 cold-request baseline.
4. **Connection reuse** — the warm HTTP/2 observation confirms that reuse can materially reduce the public-path cost.
5. **Login application polish** — optional optimization of the ~21.53 ms backend p50 and ~8.02 ms DB p50 after infrastructure latency is addressed.

## Baseline for future comparison

The original baseline is intentionally retained.

Future measurements should preserve the same endpoint and methodology where possible and publish a comparison such as:

| Endpoint | Baseline total p50 | Optimized total p50 | Baseline backend p50 | Optimized backend p50 | Total improvement |
|---|---:|---:|---:|---:|---:|
| `/healthz` | 2,304.53 ms | TBD | 1.14 ms | TBD | TBD |
| `/login` | 1,191.04 ms | TBD | 21.53 ms | TBD | TBD |

The comparison should also track p95/p99 and connection failures so improvements in reliability are visible alongside latency improvements.

## Interpretation boundary

This report currently contains two measurement sets:

1. the original 20-sample cold external baseline;
2. a smaller 8-sample cold-vs-warm HTTP/2 diagnostic observation.

Neither establishes:

- global user latency;
- authenticated workflow latency;
- application throughput;
- SFU/media capacity;
- production SLA or availability.

Those measurements are tracked separately under the broader ZNode benchmark program.

---

**Summary:** the initial external baseline shows that ZNode application processing is fast on the measured public endpoints — approximately **1.14 ms p50** for `/healthz` and **21.53 ms p50** for `/login` — while the dominant latency currently sits in the network/TCP/TLS/public ingress path. Follow-up warm HTTP/2 observations show that connection reuse materially reduces that external cost.