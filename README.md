# Rajeev Ranjan Chaurasia

Software engineer building **backend, distributed and AI systems**. MS Software Engineering at San Jose State, graduating **December 2026**. Looking for full-time software engineering roles starting **January 2027**.

[LinkedIn](https://www.linkedin.com/in/rajeev-ranjan-chaurasia/) · [Portfolio](https://rajeev-chaurasia.vercel.app/) · [Email](mailto:rajeevchaurasia.dev@gmail.com) · [acquit on PyPI](https://pypi.org/project/acquit)

**NVIDIA** (Summer 2026): built an AI-assisted system for Omniverse, NVIDIA's robotics simulation platform, that works out which tests a code change can actually affect, so each change runs only what it needs instead of the full GPU simulation suite. It combines static analysis across Python, C++, containers and generated simulation jobs with deterministic safety checks, so the AI can add evidence but can never skip a test on its own.

**Tekion** (2022 to 2024): backend engineer on accounts receivable and payments for a dealership platform. Kafka and Elasticsearch reporting, Stripe payment links, a pipeline delivering 4M+ statements a month.

What ties my work together: claims that can be checked. Benchmarks recompute from committed raw data, and when a system misses its own target, the repo says so.

## Developer tools

| Project | What it does | Evidence |
| :-- | :-- | :-- |
| [acquit](https://github.com/rajeev-chaurasia/acquit) | Skips the pytest tests a PR cannot affect, with a re-verified proof for every skip. On PyPI | 198 PRs replayed, 0 unsafe skips |
| [benchgrid](https://github.com/rajeev-chaurasia/benchgrid) | Performance test orchestration in Go for scarce, exclusive GPU rigs, with fenced leases | No overlapping runs; the unfenced control duplicates |
| [build-sleuth](https://github.com/rajeev-chaurasia/build-sleuth) | Triages a failing GitHub Actions run and proposes a patch that only counts if the rebuild passes | 0.915 accuracy vs 0.218 regex baseline |
| [tracelab](https://github.com/rajeev-chaurasia/tracelab) | Tells a real performance regression from a noisy machine | 2,042 real runs |

## Backend and distributed systems

| Project | What it does | Evidence |
| :-- | :-- | :-- |
| [pay-rail](https://github.com/rajeev-chaurasia/pay-rail) | Payment intents with idempotency, an immutable double-entry ledger and a transactional Kafka outbox | 1M fault-injected actions, 0 duplicate ledger effects |
| [limit-order-book](https://github.com/rajeev-chaurasia/limit-order-book) | Java matching engine: one writer thread, lock-free ring, nothing for the GC to do | 20.0M cmd/sec, 0 GC cycles |
| [vertex-kv](https://github.com/rajeev-chaurasia/vertex-kv) | Key-value store from scratch in C++17: LSM tree, WAL, Raft with pre-vote | Linearizable reads and writes |
| [rail-yard](https://github.com/rajeev-chaurasia/rail-yard) | Job orchestrator in Go with fenced leases and DAG dependencies | 50k decisions replayed, 100% byte match |

## AI infrastructure

| Project | What it does | Evidence |
| :-- | :-- | :-- |
| [warpline](https://github.com/rajeev-chaurasia/warpline) | Hand-written Triton kernels for LLM inference, profiled with Nsight | 1.52x vs torch.compile |
| [agent-serve](https://github.com/rajeev-chaurasia/agent-serve) | One endpoint over three vLLM replicas, pinning each conversation to its cache | 83% prefix-cache hit rate vs 0% |
| [pen-stock](https://github.com/rajeev-chaurasia/pen-stock) | LLM gateway in one Go binary: failover, spend caps, OpenAI-compatible | 1.02 ms overhead vs LiteLLM's 12.14 ms |
| [medi-agent](https://github.com/sjsu-masters-projects/medi-agent) | Clinical agent platform on FHIR that admits when it has no grounding | Live app |

**Open source:** 5 merged PRs to [RocketRide](https://github.com/rocketride-org/rocketride-server/pulls?q=is%3Apr+author%3Arajeev-chaurasia), an AI pipeline runtime (C++ and Python): runtime delivery fixes, MCP developer tooling, RAG grounding.

**Hackathons:** winner at DeveloperWeek 2026 (Deepgram prize, [ClaraCare](https://github.com/projects-hacks/clara-care)) and two more wins ([ClearClause](https://github.com/projects-hacks/clear-clause), SJ Hopes). Other builds in [projects-hacks](https://github.com/projects-hacks).

**Certification:** [NVIDIA Certified Professional: Agentic AI](https://www.credly.com/badges/86057ac4-f66e-469e-a68a-5104cfd2f886) (2026)
