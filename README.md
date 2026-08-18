# Rajeev Ranjan Chaurasia

<p align="left">
  <a href="https://www.linkedin.com/in/rajeev-ranjan-chaurasia/" target="_blank">
    <img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"/>
  </a>
  <a href="https://rajeev-chaurasia.vercel.app/" target="_blank">
    <img src="https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=vercel&logoColor=white" alt="Portfolio"/>
  </a>
  <a href="https://pypi.org/project/acquit" target="_blank">
    <img src="https://img.shields.io/badge/PyPI-acquit-3775A9?style=for-the-badge&logo=pypi&logoColor=white" alt="PyPI"/>
  </a>
  <a href="https://devpost.com/rajeevranjan-chaurasia" target="_blank">
    <img src="https://img.shields.io/badge/Devpost-003E54?style=for-the-badge&logo=devpost&logoColor=white" alt="Devpost"/>
  </a>
  <a href="mailto:rajeevchaurasia.dev@gmail.com">
    <img src="https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"/>
  </a>
</p>

**Software Engineer · MS Software Engineering @ San Jose State**

I've written a GPU kernel in Triton, a matching engine in Java, a job orchestrator in Go, a document review queue in React, a Chrome extension that runs a model without touching the network, and a voice agent that phones elderly people for a daily check-in.

> The range is on purpose. I'd rather be the engineer who can follow a problem down to whatever layer it actually lives on than the one who can only work at their favourite altitude.

Before grad school: two and a half years at **Tekion Corp** on accounts receivable and payments for a dealership platform. Last summer: **NVIDIA**, writing CI infrastructure for a robotics simulation platform.

The one habit that shows up in everything I build is a preference for **claims that can be checked**. Benchmarks recompute from committed raw samples. Evaluation harnesses get tested against deliberately broken inputs. Where a system misses its own target, the repo publishes the miss instead of quietly moving the target.

---

## Systems & Performance

| Project | What it is | Result |
| :--- | :--- | :--- |
| [**limit-order-book**](https://github.com/rajeev-chaurasia/limit-order-book) | Java matching engine, one writer thread, lock-free MPSC ring | `20.0M` cmd/sec · `0` GC cycles |
| [**warpline**](https://github.com/rajeev-chaurasia/warpline) | Hand-written Triton GPU kernels | `14.33x` vs eager · `1.52x` vs `torch.compile` |
| [**railyard**](https://github.com/rajeev-chaurasia/rail-yard) | Durable job orchestrator in Go with fenced leases | 50k decisions replayed at **100% byte match** |
| [**vertex-kv**](https://github.com/rajeev-chaurasia/vertex-kv) | C++17 LSM storage engine + Raft over gRPC | Leader election, log replication |
| [**omni-stream**](https://github.com/rajeev-chaurasia/omni-stream) | 60 Hz C++ sensor loop, bounded queue backpressure | `59.1` / `60.2 Hz` held under load |
| [**pulse**](https://github.com/rajeev-chaurasia/pulse) | Reciprocal-match engine on a stateful Flink job | gRPC ingest → WebSocket push |

<details>
<summary>Deep dive: limit-order-book, warpline, railyard</summary>

<br>

**limit-order-book** — one thread owns all book state, orders arrive through a lock-free MPSC ring, and the match path is built so the garbage collector has nothing to do.

| Metric | Optimized | Naive baseline |
| :--- | :--- | :--- |
| Throughput | `20,034,337` cmd/sec | `8,603,846` cmd/sec |
| Allocation | `0.73` B/op | `86.78` B/op |
| GC cycles | `0` | `34` |

The baseline is a fair idiomatic Java implementation held to the identical contract by a shared test suite, not a strawman. The trick is intrusive structures: resting orders **are** their own FIFO queue nodes, price levels **are** their own AVL tree nodes, so there's no wrapper object to allocate.

---

**warpline** — fused RMSNorm + residual at 64×8192:

| Baseline | Time | Speedup |
| :--- | :--- | :--- |
| warpline | `0.011968 ms` | — |
| PyTorch eager | `0.171504 ms` | `14.33x` |
| Best `torch.compile` | `0.018144 ms` | `1.52x` |

The 14x is the fun number. The 1.52x is the honest one, because `torch.compile` is what anyone serious would actually compare against. It's the only repo I own where the "claimed but not yet verified" list is completely empty.

---

**railyard** — a SQLite control plane hands **fenced** leases to a worker pool, so a resurrected zombie worker cannot write over its own replacement. Roughly 60% of the codebase exists purely to prove the scheduler is deterministic.

| Check | Result |
| :--- | :--- |
| Scheduler replay | 50,000 decisions across 3 clean processes, **100% byte match** |
| Chaos run | 20 worker kills + 1 server kill over 5,000 jobs |
| Outcome integrity | **0 lost, 0 duplicate** |

It publishes a documented **miss** on 2 of its own 5 release targets. `evidence_valid` and `qualified` are separate fields on purpose, so a clean miss cannot be laundered into a pass.

</details>

---

## AI & Agents

| Project | What it is | Result |
| :--- | :--- | :--- |
| [**build-sleuth**](https://github.com/rajeev-chaurasia/build-sleuth) | CI triage agent that verifies its own patch in a container | `0.915` accuracy vs `0.218` regex baseline |
| [**medi-agent**](https://github.com/sjsu-masters-projects/medi-agent) | Clinical RAG that admits when it has no grounding | 31 golden cases with the LLM forced to fail |
| [**agent-serve**](https://github.com/rajeev-chaurasia/agent-serve) | Session-affinity routing across three vLLM replicas | `83%` prefix-cache hit rate vs `0%` |
| [**agent-perf**](https://github.com/rajeev-chaurasia/agent-perf) | Serving-engine benchmarks under locked GPU clocks | 216-cell sweep across 3 engines |
| [**penstock**](https://github.com/rajeev-chaurasia/pen-stock) | Multi-provider LLM gateway, one Go binary | `1.02 ms` overhead vs LiteLLM's `12.14 ms` |
| [**edgedrive-vlm**](https://github.com/rajeev-chaurasia/edgedriveVLM) | 4B vision-language model compressed for in-car edge hardware | Measures what compression costs |

<details>
<summary>Deep dive: build-sleuth, medi-agent, penstock</summary>

<br>

**build-sleuth** — your build goes red. The expensive question isn't *how do I fix this*, it's *is this even my fault*: a real regression, a flaky test, and a runner outage look identical in the log.

It reads the failing run, classifies it, ranks the culprit file, drafts a patch, then applies that patch and reruns the original job **inside a container**. It only opens a draft PR if the build actually goes green.

| Metric | Result |
| :--- | :--- |
| Classification accuracy | `0.915` (regex baseline: `0.775`) |
| Macro-F1 | `0.771` (regex baseline: `0.218`) |
| Dataset | 71 hand-labeled cases, two independent blind annotators |
| Cost | ~`$0.0008` per triage |
| Fix funnel | 13 of 20 patches applied · 6 passed the build |

The eval harness is itself tested: every case runs against the maintainer's real fix **and** a deliberately corrupted copy. That negative control caught a bug where a fuzzy patch fallback was accepting patches git would reject.

The first real failure it ever saw was a live GitHub Actions outage, and it declined to write a patch. That was the correct answer, and the behaviour I designed for.

*In progress: exposing the pipeline as an MCP server over stdio.*

---

**medi-agent** *(team project — I own the backend agent, model routing, RAG, and auth layers)*

pgvector over FDA DailyMed drug labels, 768-dimension embeddings on an ivfflat cosine index. The design decision I like is the **SQL metadata prefilter that runs before ranking**, restricting candidates to the patient's own active medications, so the vector search physically cannot surface a label for a drug they aren't taking. Below a `0.72` similarity floor it returns nothing and the prompt branches to make the model admit it lacks grounding.

Triage safety is the best-tested part: **31 golden routing cases** in English and Spanish, asserted with the **LLM forced to fail**, so the deterministic fallback carries the emergency path alone.

---

**penstock** — streaming passthrough, provider fallback chains, half-open circuit breaking, per-tenant spend caps. One Go binary, `1.02 ms` of added latency per request against LiteLLM's `12.14 ms`, measured over 2,400 samples per arm with raw output committed.

It ships a semantic cache that is **disabled by default**, because a 257-probe labeled study found no safe similarity threshold: opposite-meaning questions outscored genuine paraphrases (AUC `0.39`), and `43.9%` of hits at the shipped floor answered the inverted question. Killing my own feature with my own evidence is probably the most useful thing in that repo.

</details>

---

## Data Engineering

| Project | What it is | Result |
| :--- | :--- | :--- |
| [**millrace**](https://github.com/rajeev-chaurasia/millrace) | CDC warehouse pipeline, Postgres → Kafka → Spark → dbt → DuckDB | **39 of 39** reconciliation checks required to publish |
| [**flow-engine**](https://github.com/rajeev-chaurasia/flow-engine) | DAG workflow engine, TS scheduler + Python worker pool | Execution-time workflow snapshotting |
| [**cinestream**](https://github.com/rajeev-chaurasia/cinestream-recommendation-engine) | Recommender blending collaborative filtering with content similarity | Redis feature store, Kafka event stream |
| [**volt-stream**](https://github.com/rajeev-chaurasia/volt-stream) | EV fleet telemetry over gRPC streams into Kafka | Time-series store + live map |

<details>
<summary>Deep dive: millrace, flow-engine</summary>

<br>

**millrace** — PostgreSQL → Debezium → Kafka → Spark → Parquet → dbt → DuckDB. The publication gate at the end recomputes row counts, per-column SHA-256 checksums, partition counts, and business aggregates against an independent source-of-truth path. **39 of 39 checks** have to agree or the published views don't swap.

---

**flow-engine** — a TypeScript scheduler resolves a JSON dependency graph and farms steps to Python workers over Redis. Three-colour DFS cycle detection distinguishes a diamond (a node legitimately reached twice) from a real back edge. Every execution embeds a frozen snapshot of the workflow it started with, so a long-running job can't change shape mid-flight because someone edited the definition.

</details>

---

## Product & Full-Stack

| Project | What it is | Result |
| :--- | :--- | :--- |
| [**cross-foot**](https://github.com/rajeev-chaurasia/cross-foot) | Document extraction that scores its own confidence per field | `96.02%` precision at `6.38%` human review |
| [**acquit**](https://pypi.org/project/acquit) | Test selection with a machine-checkable proof per skip | 198 PRs replayed, **0 unsafe skips** |
| [**shop-sentinel**](https://github.com/rajeev-chaurasia/shop-sentinel) | Chrome extension scoring scam risk via on-device Gemini Nano | Nothing about the page leaves the browser |
| [**mirra-ai**](https://github.com/rajeev-chaurasia/mirra-ai) | Photos → skin analysis, virtual try-on, purchase justification | 202 commits in 7 days |
| [**stock-portfolio-engine**](https://github.com/rajeev-chaurasia/stock-portfolio-engine) | Turns an amount + strategies into a weighted allocation | Share counts, leftover cash, risk metrics |
| [**atomic-ledger**](https://github.com/rajeev-chaurasia/atomic-ledger) | Double-entry ledger and settlement service | Debits = credits enforced on every fee path |

<details>
<summary>Deep dive: cross-foot, acquit</summary>

<br>

**cross-foot** — routes dealer statements (PDF, CSV, XLSX) to deterministic extractors or a vision model depending on file signature, then scores how much it trusts every individual field it read and sends only the shaky ones to a human review queue.

| Metric | Result |
| :--- | :--- |
| Auto-accept precision | `96.02%` at a `6.38%` review rate |
| Test set | 1,880 held-out fields |
| Calibration error (amounts) | `0.039`, against a `0.05` ceiling |
| Test suite | 827 Python + 163 frontend, gated on every PR |

Per-family Platt scaling is fit **over the logit** rather than the raw confidence, so the identity transform stays expressible and a family that's already calibrated survives being fit unchanged.

A test walks the AST of every module and fails the build if scoring code can even *reach* the answer key. It exists because a weaker, import-only version of that guard once let ground truth leak into the review database.

---

**acquit** — most test-selection tools skip tests because a heuristic thinks they're probably fine. acquit attaches a machine-checkable proof to every skip, and when it can't prove something, it runs the test. Shipped to **PyPI** and the **GitHub Marketplace**.

| Study | Result |
| :--- | :--- |
| Replay across `click`, `flask`, `rich`, `httpx` | 198 merged PRs, **0 unsafe skips** |
| `httpx` selective PRs | median `96.8%` of test files skipped |
| Analysis overhead | p50 `0.432s`, p95 `0.478s` per PR |
| Mutation arm on `black` | 105 mutants, **100% detection parity** vs full suite |

</details>

---

## Built at Hackathons

| Project | Award | What it is |
| :--- | :--- | :--- |
| [**ClaraCare**](https://github.com/projects-hacks/clara-care) | **Winner** — Deepgram "Voice Operator" prize, DeveloperWeek 2026 | A companion that phones elderly parents for a daily check-in |
| [**ClearClause**](https://github.com/projects-hacks/clear-clause) | **Winner** | Redacts a contract PDF before any model sees it, then flags rights-stripping clauses in place |
| [**SJ Hopes**](https://github.com/rajeev-chaurasia/sj-hopes) | **Winner** — civic hackathon | Shelter beds on a map, client registration, and paid micro-tasks for caseworkers |
| [**fair-med**](https://github.com/rajeev-chaurasia/fair-med) | | Audits a medical bill against the Medicare fee schedule, then drafts the dispute letter |
| [**smart-pay**](https://github.com/projects-hacks/smart-pay) | | Neo N3 escrow where a vision agent scores before/after job photos behind hard GPS gates |
| [**meet-mind**](https://github.com/rajeev-chaurasia/meet-mind) | | On-device Apple Silicon meeting copilot: mic + whiteboard camera, three-agent Gemma loop |

<details>
<summary>The ClaraCare bug worth knowing about</summary>

<br>

Telephony audio comes off Twilio at 8 kHz mulaw straight into the Deepgram Voice Agent socket, with six function-calling tools firing mid-conversation.

The sharpest thing in that codebase came from a bug. The API has two steering channels: one silently updates the agent's instructions, the other makes it **speak**. Confusing them meant internal guidance text got read aloud to a patient mid-call.

They're now strictly separated, with forced speech queued behind a speaking-state flag that keeps only the newest message and discards the rest, because stale conversational context is worse than none. There's also a prefix filter that drops steering strings if the model echoes them back, so they never land in the saved transcript.

</details>

---

## Toolkit

| | |
| :--- | :--- |
| **Languages** | ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![Go](https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white) ![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white) ![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=cplusplus&logoColor=white) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white) ![SQL](https://img.shields.io/badge/SQL-CC2927?style=flat-square&logo=databricks&logoColor=white) ![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white) |
| **Systems & Concurrency** | `Lock-Free Structures` · `VarHandle / Memory Ordering` · `Object Pooling` · `Raft Consensus` · `LSM Trees` · `Fenced Leases` · `OpenMP` · `Backpressure` |
| **Backend & APIs** | ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white) ![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white) ![gRPC](https://img.shields.io/badge/gRPC-244c5a?style=flat-square&logo=grpc&logoColor=white) ![Node](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white) <br> `REST` · `WebSockets` · `Server-Sent Events` · `Protobuf` · `Idempotency & Retry Ladders` |
| **Frontend** | ![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black) ![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white) ![Redux](https://img.shields.io/badge/Redux_Toolkit-764ABC?style=flat-square&logo=redux&logoColor=white) ![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white) ![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white) <br> `React Native / Expo` · `Chrome Extensions` · `PWA` |
| **AI & Agents** | ![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?style=flat-square&logo=langchain&logoColor=white) ![CrewAI](https://img.shields.io/badge/CrewAI-FF5A5F?style=flat-square) ![MCP](https://img.shields.io/badge/MCP-1F1F1F?style=flat-square) ![Triton](https://img.shields.io/badge/Triton-76B900?style=flat-square&logo=nvidia&logoColor=white) <br> `RAG & Grounding` · `Tool Calling` · `Multi-Agent Orchestration` · `Eval Harnesses` · `Vision-Language Models` · `On-Device Inference` |
| **LLM Serving** | ![vLLM](https://img.shields.io/badge/vLLM-4B0082?style=flat-square) ![SGLang](https://img.shields.io/badge/SGLang-2E7D32?style=flat-square) ![TensorRT](https://img.shields.io/badge/TensorRT--LLM-76B900?style=flat-square&logo=nvidia&logoColor=white) <br> `KV-Cache Locality` · `Session Affinity` · `Admission Control` · `TTFT Benchmarking` |
| **Data & Streaming** | ![Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white) ![Spark](https://img.shields.io/badge/Spark-E25A1C?style=flat-square&logo=apachespark&logoColor=white) ![Flink](https://img.shields.io/badge/Flink-E6526F?style=flat-square&logo=apacheflink&logoColor=white) ![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white) ![dbt](https://img.shields.io/badge/dbt-FF694B?style=flat-square&logo=dbt&logoColor=white) <br> `Debezium CDC` · `Parquet` · `Star Schema` · `Event-Driven Architecture` |
| **Databases** | ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white) ![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white) ![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white) ![DuckDB](https://img.shields.io/badge/DuckDB-FFF000?style=flat-square&logo=duckdb&logoColor=black) <br> `pgvector` · `Composite Indexing` · `Cache-Aside` · `Time-Series` |
| **Infra & DevOps** | ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white) ![Helm](https://img.shields.io/badge/Helm-0F1689?style=flat-square&logo=helm&logoColor=white) ![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonwebservices&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white) ![GitLab CI](https://img.shields.io/badge/GitLab_CI-FC6D26?style=flat-square&logo=gitlab&logoColor=white) |
| **Observability** | ![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white) ![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat-square&logo=grafana&logoColor=white) ![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-000000?style=flat-square&logo=opentelemetry&logoColor=white) <br> `SLOs & Alert Rules` · `Structured Logging` · `Incident RCA` |
| **Testing & Quality** | ![pytest](https://img.shields.io/badge/pytest-0A9EDC?style=flat-square&logo=pytest&logoColor=white) ![JUnit](https://img.shields.io/badge/JUnit-25A162?style=flat-square&logo=junit5&logoColor=white) ![Vitest](https://img.shields.io/badge/Vitest-6E9F18?style=flat-square&logo=vitest&logoColor=white) <br> `JMH Benchmarking` · `Mutation Testing` · `Chaos Testing` · `Property-Based Testing` · `mypy` · `ruff` |

---

## Certification

**[NVIDIA Certified Professional: Agentic AI](https://www.credly.com/badges/86057ac4-f66e-469e-a68a-5104cfd2f886)** · Issued August 2026 · Valid through August 2028

Agent architecture and development · cognition and memory · evaluation and tuning · deployment at scale · data integration · human oversight · operations and monitoring

---

Happy to talk about lock-free data structures, GPU kernels, agent infrastructure, or the right amount of rigor for a weekend hackathon — [rajeevchaurasia.dev@gmail.com](mailto:rajeevchaurasia.dev@gmail.com)
