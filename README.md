
#Environment
Operating System: Windows 11
Python Version: 3.12
IDE: Visual Studio Code/ cmd
Tool Version: Presidio Hardened Architecture Translucency v0.8.0

#Saluka Subasinghe 
#ID: 100007818

#test 1
pat analyze -r 100 -l 50 -c container --show-all
2026-06-13 20:47:35,793 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CLI_INVOCATION' context={'version': '0.8.0'}
2026-06-13 20:47:35,837 [WARNING] presidio.arch_translucency.security — Dependency CVE audit: VULNERABILITIES FOUND

2026-06-13 20:47:35,839 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CVE_VULNERABILITIES_DETECTED' context={}
⚠ No calibrated model found (.pat-model.json or ~/.pat/model.json). Using default parameters
calibrated for async Python services in the ~50–2000 req/s and ~10–250 ms latency envelope. Results
may be inaccurate outside this range or for non-async (single-threaded / CPU-bound) workloads.
  Run pat calibrate with your measured workload to tune the model.
2026-06-13 20:47:35,847 [INFO] presidio.arch_translucency.audit — Presidio architectural-translucency recommendation applied: layer='container' replicas=1 throughput_gain_pct=0.0
2026-06-13 20:47:35,847 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='Presidio architectural-translucency recommendation applied' context={'layer': 'container', 'replicas': 1}

╭─────────────────────── Presidio Architectural Translucency — Recommendation ───────────────────────╮
│ Recommended layer:  container                                                                      │
│ Optimal replicas:   1                                                                              │
│ Throughput gain:    +0.0%                                                                          │
│ Response-time Δ:    +0.0%                                                                          │
│ Est. throughput:    100 req/s                                                                      │
│ Est. response time: 50.0 ms                                                                        │
│                                                                                                    │
│ New Docker container (process-level isolation, shared kernel)                                      │
╰────────────────────────────────────────────────────────────────────────────────────────────────────╯

                                         All Layers Analysis
╭────────────┬──────────┬───────────────────┬──────────────┬───────────────────┬───────┬─────────────╮
│            │          │        Throughput │              │     Response Time │       │             │
│ Layer      │ Replicas │           (req/s) │ Δ Throughput │              (ms) │  Δ RT │ Recommended │
├────────────┼──────────┼───────────────────┼──────────────┼───────────────────┼───────┼─────────────┤
│ container  │        1 │               100 │        +0.0% │              50.0 │ +0.0% │      ✓      │
├────────────┼──────────┼───────────────────┼──────────────┼───────────────────┼───────┼─────────────┤
│ pod        │        1 │               100 │        +0.0% │              50.0 │ +0.0% │             │
├────────────┼──────────┼───────────────────┼──────────────┼───────────────────┼───────┼─────────────┤
│ deployment │        1 │               100 │        +0.0% │              50.0 │ +0.0% │             │
├────────────┼──────────┼───────────────────┼──────────────┼───────────────────┼───────┼─────────────┤
│ node       │        1 │               100 │        +0.0% │              50.0 │ +0.0% │             │
╰────────────┴──────────┴───────────────────┴──────────────┴───────────────────┴───────┴─────────────╯

Baseline: 100 req/s @ 50.0 ms  (current layer: container)

#Test 2
C:\Users\ROG ZEYPHRUS>pat analyze --requests-per-second 500 --avg-latency-ms 80 --current-layer container
2026-06-13 20:53:19,446 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CLI_INVOCATION' context={'version': '0.8.0'}
2026-06-13 20:53:19,492 [WARNING] presidio.arch_translucency.security — Dependency CVE audit: VULNERABILITIES FOUND

2026-06-13 20:53:19,492 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CVE_VULNERABILITIES_DETECTED' context={}
⚠ No calibrated model found (.pat-model.json or ~/.pat/model.json). Using default parameters
calibrated for async Python services in the ~50–2000 req/s and ~10–250 ms latency envelope. Results
may be inaccurate outside this range or for non-async (single-threaded / CPU-bound) workloads.
  Run pat calibrate with your measured workload to tune the model.
2026-06-13 20:53:19,498 [INFO] presidio.arch_translucency.audit — Presidio architectural-translucency recommendation applied: layer='container' replicas=6 throughput_gain_pct=400.0
2026-06-13 20:53:19,500 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='Presidio architectural-translucency recommendation applied' context={'layer': 'container', 'replicas': 6}

╭─────────────────────── Presidio Architectural Translucency — Recommendation ───────────────────────╮
│ Recommended layer:  container                                                                      │
│ Optimal replicas:   6                                                                              │
│ Throughput gain:    +400.0%                                                                        │
│ Response-time Δ:    +605.5%                                                                        │
│ Est. throughput:    500 req/s                                                                      │
│ Est. response time: 564.4 ms                                                                       │
│                                                                                                    │
│ New Docker container (process-level isolation, shared kernel)                                      │
╰────────────────────────────────────────────────────────────────────────────────────────────────────╯

Baseline: 100 req/s @ 80.0 ms  (current layer: container)

#test 3
C:\Users\ROG ZEYPHRUS>pat demo --replicas 6 --requests 80 --concurrency 12 --cost-per-container-hour 0.05 --output results.png
2026-06-13 22:09:41,162 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CLI_INVOCATION' context={'version': '0.8.0'}
2026-06-13 22:09:41,210 [WARNING] presidio.arch_translucency.security — Dependency CVE audit: VULNERABILITIES FOUND

2026-06-13 22:09:41,210 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CVE_VULNERABILITIES_DETECTED' context={}
C:\Users\ROG ZEYPHRUS\AppData\Local\Programs\Python\Python312\Lib\site-packages\requests\__init__.py:113: RequestsDependencyWarning: urllib3 (2.1.0) or chardet (7.4.3)/charset_normalizer (3.3.2) doesn't match a supported version!
  warnings.warn(
2026-06-13 22:09:41,514 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='DEMO_INVOCATION' context={'replicas': 6}
╭─────────────────────────────────────────────────────────────────────────────── Presidio Architectural Translucency — Live Demo ───────────────────────────────────────────────────────────────────────────────╮
│ Replicas: 6  Requests: 80  Concurrency: 12  Iterations/req: 200,000                                                                                                                                           │
╰───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
Image pat-demo-workload:0.1.0 already present — skipping build.

Variant 1 — Single container

Variant 2 — 6 independent containers

Variant 3 — 6 workers + nginx load balancer
1 — Single container           ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 80/80 0:00:07
2 — 6 containers (round-robin) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 80/80 0:00:01
3 — nginx LB (6 workers)       ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 80/80 0:00:01

                             Architectural Translucency — Measured Results
╭────────────────────────────────┬─────────┬────────────┬─────────┬─────────┬─────────┬────────┬───────╮
│                                │         │ Throughput │ Avg Lat │ p95 Lat │   CPU % │        │       │
│ Variant                        │ Workers │    (req/s) │    (ms) │    (ms) │ (total) │ Errors │ Best? │
├────────────────────────────────┼─────────┼────────────┼─────────┼─────────┼─────────┼────────┼───────┤
│ 1 — Single container           │       1 │       10.4 │    1020 │    7339 │      69 │      0 │       │
├────────────────────────────────┼─────────┼────────────┼─────────┼─────────┼─────────┼────────┼───────┤
│ 2 — 6 containers (round-robin) │       6 │       61.0 │     179 │     211 │      98 │      0 │       │
├────────────────────────────────┼─────────┼────────────┼─────────┼─────────┼─────────┼────────┼───────┤
│ 3 — nginx LB (6 workers)       │       7 │       61.3 │     177 │     206 │      98 │      1 │   ✓   │
╰────────────────────────────────┴─────────┴────────────┴─────────┴─────────┴─────────┴────────┴───────╯

╭───────────────────────────────────────────────────────────────────────────────────── Architectural Translucency Insight ──────────────────────────────────────────────────────────────────────────────────────╮
│ Best layer : 3 — nginx LB (6 workers) (61.3 req/s, 177 ms avg)                                                                                                                                                │
│ Baseline   : 1 — Single container (10.4 req/s, 1020 ms avg)                                                                                                                                                   │
│ Speedup    : 5.90× throughput,  83% latency reduction                                                                                                                                                         │
│                                                                                                                                                                                                               │
│ Architectural translucency insight:                                                                                                                                                                           │
│   The nginx load-balancer layer adds a thin routing tier but enables a single stable endpoint — closer to a Kubernetes Service/Deployment model. The scheduling overhead (β·ln δ) is offset by better         │
│ connection reuse across replicas.                                                                                                                                                                             │
╰───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯

Plot saved → results.png

╭─────────────────────────────────────────────────────────────────────────────────── HPA Lag Projection (if load spikes 3×) ────────────────────────────────────────────────────────────────────────────────────╮
│ Measured baseline:  3 — nginx LB (6 workers)                                                                                                                                                                  │
│   61.3 req/s  ·  177 ms avg latency                                                                                                                                                                           │
│                                                                                                                                                                                                               │
│ Hypothetical spike:  3× → 184.0 req/s                                                                                                                                                                         │
│                                                                                                                                                                                                               │
│ TROUGH  (0 s – 45 s  =  HPA poll 15 s  +  pod startup 30 s)                                                                                                                                                   │
│   Throughput    184.0 req/s  (100 % of spike demand)                                                                                                                                                          │
│   p99 latency   8,862 ms                                                                                                                                                                                      │
│   Missed reqs   ~0                                                                                                                                                                                            │
│                                                                                                                                                                                                               │
│ STEADY STATE  (after 45 s  —  5 replicas)                                                                                                                                                                     │
│   Throughput    184.0 req/s                                                                                                                                                                                   │
│   p99 latency   16,257 ms                                                                                                                                                                                     │
│                                                                                                                                                                                                               │
│ → Set HPA minReplicas = 5 to pre-provision and eliminate the trough.                                                                                                                                          │
╰───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
HPA plot saved → results-hpa.png


                              Cost Efficiency — Measured Variants
╭────────────────────────────────┬────────────┬────────────┬─────────┬─────────────┬───────────╮
│                                │            │ Throughput │ Cost/hr │    Cost/req │           │
│ Variant                        │ Containers │    (req/s) │   (USD) │       (USD) │ Best ROI? │
├────────────────────────────────┼────────────┼────────────┼─────────┼─────────────┼───────────┤
│ 1 — Single container           │          1 │       10.4 │ $0.0500 │ $1.3355e-06 │     ✓     │
├────────────────────────────────┼────────────┼────────────┼─────────┼─────────────┼───────────┤
│ 2 — 6 containers (round-robin) │          6 │       61.0 │ $0.3000 │ $1.3672e-06 │           │
├────────────────────────────────┼────────────┼────────────┼─────────┼─────────────┼───────────┤
│ 3 — nginx LB (6 workers)       │          7 │       61.3 │ $0.3500 │ $1.5855e-06 │           │
╰────────────────────────────────┴────────────┴────────────┴─────────┴─────────────┴───────────╯
╭──────────────────────────────────────────────────────────────────────────────────────────── v0.5.0 Cost Analysis ─────────────────────────────────────────────────────────────────────────────────────────────╮
│ Cost assumption:  $0.0500 / container / hour                                                                                                                                                                  │
│                                                                                                                                                                                                               │
│ Best measured variant:  1 — Single container                                                                                                                                                                  │
│   Cost/req  $1.3355e-06  ·  Cost/hr  $0.0500                                                                                                                                                                  │
│                                                                                                                                                                                                               │
│ Analytical best-ROI layer:  container                                                                                                                                                                         │
│   Replicas  2  ·  Throughput gain  +35.5%                                                                                                                                                                     │
│   Cost/req  $4.5300e-07  ·  Cost/hr  $0.1000                                                                                                                                                                  │
│   ROI score  78366960.0                                                                                                                                                                                       │
│                                                                                                                                                                                                               │
│ Run pat cost --cloud aws with your region and instance type for a live-priced full breakdown.                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
──────────────────────────────────────────────────────────╯
2026-06-13 22:11:37,320 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='DEMO_COMPLETE' context={'variants': 3}

# test 4: cost analysis
C:\Users\ROG ZEYPHRUS>pat cost --requests-per-second 500 --avg-latency-ms 80 --current-layer container --cost-per-container-hour 0.02 --cost-per-pod-hour 0.05 --cost-per-deployment-hour 0.10 --cost-per-node-hour 0.50
2026-06-14 22:52:05,353 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CLI_INVOCATION' context={'version': '0.8.0'}
2026-06-14 22:52:05,395 [WARNING] presidio.arch_translucency.security — Dependency CVE audit: VULNERABILITIES FOUND

2026-06-14 22:52:05,395 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CVE_VULNERABILITIES_DETECTED' context={}
⚠ No calibrated model found (.pat-model.json or ~/.pat/model.json). Using default parameters calibrated for async Python
services in the ~50–2000 req/s and ~10–250 ms latency envelope. Results may be inaccurate outside this range or for
non-async (single-threaded / CPU-bound) workloads.
  Run pat calibrate with your measured workload to tune the model.
2026-06-14 22:52:05,401 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='COST_INVOCATION' context={'layer': 'container', 'rps': 500.0}

╭──────────────────────────────── Presidio Architectural Translucency — Cost Analysis ─────────────────────────────────╮
│ Best ROI layer:  container                                                                                           │
│ Replicas:        6                                                                                                   │
│ Throughput gain: +400.0%                                                                                             │
│ Cost/hour:       $0.1200                                                                                             │
│ Cost/request:    $6.6667e-08                                                                                         │
│ ROI score:       6000000000.0                                                                                        │
│                                                                                                                      │
│ New Docker container (process-level isolation, shared kernel)                                                        │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯

                                            All Layers — Cost vs Performance
╭─────────────────────┬──────────┬────────────┬────────────┬──────────┬────────────┬────────────┬───────────┬──────────╮
│                     │          │ Throughput │          Δ │          │    Cost/hr │   Cost/req │           │          │
│ Layer               │ Replicas │    (req/s) │ Throughput │     Δ RT │      (USD) │      (USD) │ ROI score │ Best ROI │
├─────────────────────┼──────────┼────────────┼────────────┼──────────┼────────────┼────────────┼───────────┼──────────┤
│ container (current) │        6 │        500 │    +400.0% │  +605.5% │    $0.1200 │ $6.6667e-… │ 60000000… │    ✓     │
├─────────────────────┼──────────┼────────────┼────────────┼──────────┼────────────┼────────────┼───────────┼──────────┤
│ pod                 │        6 │        500 │    +400.0% │  +818.3% │    $0.3000 │ $1.6667e-… │ 24000000… │          │
├─────────────────────┼──────────┼────────────┼────────────┼──────────┼────────────┼────────────┼───────────┼──────────┤
│ deployment          │        6 │        500 │    +400.0% │ +1789.6% │    $0.6000 │ $3.3333e-… │ 12000000… │          │
├─────────────────────┼──────────┼────────────┼────────────┼──────────┼────────────┼────────────┼───────────┼──────────┤
│ node                │        7 │        500 │    +400.0% │  +922.2% │    $3.5000 │ $1.9444e-… │ 20571428… │          │
╰─────────────────────┴──────────┴────────────┴────────────┴──────────┴────────────┴────────────┴───────────┴──────────╯

Baseline: 100 req/s @ 80.0 ms  (current layer: container)
ROI score = throughput-gain-% / cost-per-request  (higher = better performance-per-dollar)

# test 5 AWS cost alaysis
C:\Users\ROG ZEYPHRUS>pat cost --requests-per-second 500 --avg-latency-ms 80 --current-layer container --cloud aws --region us-east-1 --instance-type m5.large
2026-06-14 22:53:15,125 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CLI_INVOCATION' context={'version': '0.8.0'}
2026-06-14 22:53:15,172 [WARNING] presidio.arch_translucency.security — Dependency CVE audit: VULNERABILITIES FOUND

2026-06-14 22:53:15,172 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CVE_VULNERABILITIES_DETECTED' context={}
⚠ No calibrated model found (.pat-model.json or ~/.pat/model.json). Using default parameters calibrated for async Python
services in the ~50–2000 req/s and ~10–250 ms latency envelope. Results may be inaccurate outside this range or for
non-async (single-threaded / CPU-bound) workloads.
  Run pat calibrate with your measured workload to tune the model.
2026-06-14 22:53:17,261 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='COST_INVOCATION' context={'layer': 'container', 'rps': 500.0}

╭──────────────────────────────── Presidio Architectural Translucency — Cost Analysis ─────────────────────────────────╮
│ Best ROI layer:  container                                                                                           │
│ Replicas:        6                                                                                                   │
│ Throughput gain: +400.0%                                                                                             │
│ Cost/hour:       $0.0360                                                                                             │
│ Cost/request:    $2.0000e-08                                                                                         │
│ ROI score:       20000000000.0                                                                                       │
│                                                                                                                      │
│ New Docker container (process-level isolation, shared kernel)                                                        │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯

                                            All Layers — Cost vs Performance
╭─────────────────────┬──────────┬────────────┬────────────┬──────────┬────────────┬────────────┬───────────┬──────────╮
│                     │          │ Throughput │          Δ │          │    Cost/hr │   Cost/req │           │          │
│ Layer               │ Replicas │    (req/s) │ Throughput │     Δ RT │      (USD) │      (USD) │ ROI score │ Best ROI │
├─────────────────────┼──────────┼────────────┼────────────┼──────────┼────────────┼────────────┼───────────┼──────────┤
│ container (current) │        6 │        500 │    +400.0% │  +605.5% │    $0.0360 │ $2.0000e-… │ 20000000… │    ✓     │
├─────────────────────┼──────────┼────────────┼────────────┼──────────┼────────────┼────────────┼───────────┼──────────┤
│ pod                 │        6 │        500 │    +400.0% │  +818.3% │    $0.0720 │ $4.0000e-… │ 10000000… │          │
├─────────────────────┼──────────┼────────────┼────────────┼──────────┼────────────┼────────────┼───────────┼──────────┤
│ deployment          │        6 │        500 │    +400.0% │ +1789.6% │    $0.5760 │ $3.2000e-… │ 12500000… │          │
├─────────────────────┼──────────┼────────────┼────────────┼──────────┼────────────┼────────────┼───────────┼──────────┤
│ node                │        7 │        500 │    +400.0% │  +922.2% │    $0.6720 │ $3.7333e-… │ 10714285… │          │
╰─────────────────────┴──────────┴────────────┴────────────┴──────────┴────────────┴────────────┴───────────┴──────────╯

Baseline: 100 req/s @ 80.0 ms  (current layer: container)
Pricing source: AWS EC2 on-demand · us-east-1 · m5.large ($0.0960/hr  →  container $0.0060 · pod $0.0120)
ROI score = throughput-gain-% / cost-per-request  (higher = better performance-per-dollar)

# Test 6 SLO Analysis
C:\Users\ROG ZEYPHRUS>pat slo --requests-per-second 50 --avg-latency-ms 80 --p99-target-ms 500 --spike-multiplier 3.0
2026-06-14 22:54:40,382 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CLI_INVOCATION' context={'version': '0.8.0'}
2026-06-14 22:54:40,430 [WARNING] presidio.arch_translucency.security — Dependency CVE audit: VULNERABILITIES FOUND

2026-06-14 22:54:40,431 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CVE_VULNERABILITIES_DETECTED' context={}
⚠ No calibrated model found (.pat-model.json or ~/.pat/model.json). Using default parameters calibrated for async Python
services in the ~50–2000 req/s and ~10–250 ms latency envelope. Results may be inaccurate outside this range or for
non-async (single-threaded / CPU-bound) workloads.
  Run pat calibrate with your measured workload to tune the model.
2026-06-14 22:54:40,436 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='SLO_INVOCATION' context={'rps': 50.0, 'p99_target_ms': 500.0}

SLO: p99 ≤ 500 ms  |  Workload: 50.0 req/s  |  Spike: 3.0× → 150.0 req/s
HPA timing: 15 s poll + 30 s startup = 45 s trough

╭────────────┬────────┬───────┬─────────────┬──────────────┬───────────────┬─────────────╮
│ Layer      │ Before │ After │  Steady p99 │   Trough p99 │ Cost/hr (USD) │ SLO verdict │
├────────────┼────────┼───────┼─────────────┼──────────────┼───────────────┼─────────────┤
│ container  │      1 │     2 │  5,190 ms ✗ │ 120,017 ms ✗ │       $0.0400 │ Fails SLO ✗ │
├────────────┼────────┼───────┼─────────────┼──────────────┼───────────────┼─────────────┤
│ pod        │      1 │     2 │  5,855 ms ✗ │ 120,042 ms ✗ │       $0.1000 │ Fails SLO ✗ │
├────────────┼────────┼───────┼─────────────┼──────────────┼───────────────┼─────────────┤
│ deployment │      1 │     2 │  7,422 ms ✗ │ 120,083 ms ✗ │       $0.2000 │ Fails SLO ✗ │
├────────────┼────────┼───────┼─────────────┼──────────────┼───────────────┼─────────────┤
│ node       │      1 │     2 │ 12,975 ms ✗ │ 120,150 ms ✗ │       $1.0000 │ Fails SLO ✗ │
╰────────────┴────────┴───────┴─────────────┴──────────────┴───────────────┴─────────────╯

╭─────────────────────────────────────────────────── Recommendation ───────────────────────────────────────────────────╮
│ No layer meets the p99 ≤ 500 ms SLO at 50.0 req/s.                                                                   │
│ container is closest (steady p99 5,190 ms).                                                                          │
│   → Reduce avg latency below 278 ms, or                                                                              │
│   → Pre-provision 2 replicas and re-evaluate.                                                                        │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯

#test 7 SLO high traffic analysis
C:\Users\ROG ZEYPHRUS>pat slo --requests-per-second 500 --avg-latency-ms 120 --p99-target-ms 400 --spike-multiplier 5.0
2026-06-14 22:55:43,422 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CLI_INVOCATION' context={'version': '0.8.0'}
2026-06-14 22:55:43,463 [WARNING] presidio.arch_translucency.security — Dependency CVE audit: VULNERABILITIES FOUND

2026-06-14 22:55:43,463 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CVE_VULNERABILITIES_DETECTED' context={}
⚠ No calibrated model found (.pat-model.json or ~/.pat/model.json). Using default parameters calibrated for async Python
services in the ~50–2000 req/s and ~10–250 ms latency envelope. Results may be inaccurate outside this range or for
non-async (single-threaded / CPU-bound) workloads.
  Run pat calibrate with your measured workload to tune the model.
2026-06-14 22:55:43,468 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='SLO_INVOCATION' context={'rps': 500.0, 'p99_target_ms': 400.0}

SLO: p99 ≤ 400 ms  |  Workload: 500.0 req/s  |  Spike: 5.0× → 2500.0 req/s
HPA timing: 15 s poll + 30 s startup = 45 s trough

╭────────────┬────────┬───────┬──────────────┬──────────────┬───────────────┬─────────────╮
│ Layer      │ Before │ After │   Steady p99 │   Trough p99 │ Cost/hr (USD) │ SLO verdict │
├────────────┼────────┼───────┼──────────────┼──────────────┼───────────────┼─────────────┤
│ container  │      8 │    39 │ 180,133 ms ✗ │ 180,079 ms ✗ │       $0.7800 │ Fails SLO ✗ │
├────────────┼────────┼───────┼──────────────┼──────────────┼───────────────┼─────────────┤
│ pod        │      8 │    32 │ 180,315 ms ✗ │ 180,198 ms ✗ │       $1.6000 │ Fails SLO ✗ │
├────────────┼────────┼───────┼──────────────┼──────────────┼───────────────┼─────────────┤
│ deployment │      9 │    20 │ 180,548 ms ✗ │ 180,414 ms ✗ │       $2.0000 │ Fails SLO ✗ │
├────────────┼────────┼───────┼──────────────┼──────────────┼───────────────┼─────────────┤
│ node       │      8 │     8 │ 180,712 ms ✗ │ 180,712 ms ✗ │       $4.0000 │ Fails SLO ✗ │
╰────────────┴────────┴───────┴──────────────┴──────────────┴───────────────┴─────────────╯

╭─────────────────────────────────────────────────── Recommendation ───────────────────────────────────────────────────╮
│ No layer meets the p99 ≤ 400 ms SLO at 500.0 req/s.                                                                  │
│ container is closest (steady p99 180,133 ms).                                                                        │
│   → Reduce avg latency below 222 ms, or                                                                              │
│   → Pre-provision 39 replicas and re-evaluate.                                                                       │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯

#test 8; what if 
C:\Users\ROG ZEYPHRUS>pat what-if --current-rps 50 --spike-rps 200 --avg-latency-ms 80 --current-layer container
2026-06-14 22:56:28,932 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CLI_INVOCATION' context={'version': '0.8.0'}
2026-06-14 22:56:28,969 [WARNING] presidio.arch_translucency.security — Dependency CVE audit: VULNERABILITIES FOUND

2026-06-14 22:56:28,970 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='CVE_VULNERABILITIES_DETECTED' context={}
⚠ No calibrated model found (.pat-model.json or ~/.pat/model.json). Using default parameters calibrated for async Python
services in the ~50–2000 req/s and ~10–250 ms latency envelope. Results may be inaccurate outside this range or for
non-async (single-threaded / CPU-bound) workloads.
  Run pat calibrate with your measured workload to tune the model.
2026-06-14 22:56:28,973 [INFO] presidio.arch_translucency.audit — SECURITY_EVENT event='WHAT_IF_INVOCATION' context={'layer': 'container', 'spike_rps': 200.0}

╭──────────────────────────────────── HPA Scale Event · container  1 → 3 replicas ─────────────────────────────────────╮
│ Load:  50.0 → 200.0 req/s  (4.0×)                                                                                    │
│ Trough window:  45 s  (HPA poll 15 s  +  pod startup 30 s)                                                           │
│                                                                                                                      │
│ TROUGH  (0 s – 45 s)                                                                                                 │
│   Throughput    98.0 req/s  (49 % of demand)                                                                         │
│   Avg latency   8,001 ms                                                                                             │
│   p99 latency   120,017 ms                                                                                           │
│   Missed reqs   ~4,590                                                                                               │
│                                                                                                                      │
│ STEADY STATE  (after 45 s  —  3 replicas)                                                                            │
│   Throughput    200.0 req/s                                                                                          │
│   Avg latency   255 ms                                                                                               │
│   p99 latency   3,827 ms                                                                                             │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯

                        Scale-event timeline
╭─────────┬──────────┬────────────┬──────────┬────────────┬────────╮
│    Time │ Replicas │ Throughput │  Avg Lat │    p99 Lat │ Phase  │
├─────────┼──────────┼────────────┼──────────┼────────────┼────────┤
│   0.0 s │        1 │       98.0 │ 8,001 ms │ 120,017 ms │ TROUGH │
├─────────┼──────────┼────────────┼──────────┼────────────┼────────┤
│  15.0 s │        1 │       98.0 │ 8,001 ms │ 120,017 ms │ TROUGH │
├─────────┼──────────┼────────────┼──────────┼────────────┼────────┤
│  30.0 s │        1 │       98.0 │ 8,001 ms │ 120,017 ms │ TROUGH │
├─────────┼──────────┼────────────┼──────────┼────────────┼────────┤
│  44.9 s │        1 │       98.0 │ 8,001 ms │ 120,017 ms │ TROUGH │
├─────────┼──────────┼────────────┼──────────┼────────────┼────────┤
│  45.0 s │        3 │      200.0 │   255 ms │   3,827 ms │ STEADY │
├─────────┼──────────┼────────────┼──────────┼────────────┼────────┤
│  55.0 s │        3 │      200.0 │   255 ms │   3,827 ms │ STEADY │
├─────────┼──────────┼────────────┼──────────┼────────────┼────────┤
│  75.0 s │        3 │      200.0 │   255 ms │   3,827 ms │ STEADY │
├─────────┼──────────┼────────────┼──────────┼────────────┼────────┤
│ 105.0 s │        3 │      200.0 │   255 ms │   3,827 ms │ STEADY │
╰─────────┴──────────┴────────────┴──────────┴────────────┴────────╯

# Generated Graphs

# Figure 1 – Performance Comparison
<img width="1416" height="1165" alt="results-hpa" src="https://github.com/user-attachments/assets/5e9bfad0-d28d-493b-858f-27a051c42db1" />
This graph compares the measured performance of the evaluated architectural variants. It visualizes throughput and latency differences between the tested deployment approaches.

# Figure 2 – HPA Projection

See results-hpa.png
<img width="1416" height="1165" alt="demo-results-hpa" src="https://github.com/user-attachments/assets/421975b4-e5d7-4524-8929-7b341a715599" />
This graph illustrates the Horizontal Pod Autoscaler (HPA) response to a sudden increase in workload. It shows the temporary performance degradation during scale-out and the eventual recovery once additional replicas become available.

