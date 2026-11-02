# Rohith Balsa

**AI Engineer · Production LLM Systems, Multi-Agent Runtimes & Evaluation**

I build AI systems that run in production after the demo: distributed agent runtimes, optimistic state synchronization, RAG pipelines, deterministic evaluation gates, and low-latency backend infrastructure.

My focus is the engineering layer between foundation models and dependable AI products: how you resolve concurrent agent race conditions, how we dynamically route queries for cost and latency, how you enforce strict schema boundaries, and how we make our trajectories observable. You shouldn't have to guess why your agent failed or burn your budget on blind model calls. I've spent my career making sure our systems don't fail when traffic spikes.

[LinkedIn](https://www.linkedin.com/in/rohithbalsa) · [GitHub](https://github.com/rohith0707)

---

## What I build

**Multi-Agent Runtimes** : Concurrency control (OCC), 3-way delta state reconciliation, tool execution, decision ledgers  
**LLM Control Planes** : Dynamic model routing, schema-aware guardrails, context engineering, policy gates  
**Continuous Evaluation** : Automated regression suites, trajectory failure analysis, RAGAS metrics, deterministic assertion gates  
**Enterprise Data Systems** : Text-to-SQL, Snowflake, FAISS vector embeddings, Redis caching, streaming pipelines  
**Production Engineering** : Python, FastAPI, Docker, Kubernetes, AWS, Azure AI Search, FastMCP

---

## Featured Open-Source Systems

### 1. agent-occ-lock : Distributed Multi-Agent State Synchronization

[![Repository](https://img.shields.io/badge/GitHub-agent--occ--lock-181717?logo=github)](https://github.com/rohith0707/agent-occ-lock)
[![Tests](https://img.shields.io/badge/tests-5%20passed-brightgreen.svg)]()
[![Zero Dependencies](https://img.shields.io/badge/dependencies-zero-blue.svg)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

I built `agent-occ-lock` after we ran into a nasty race condition in our multi-agent workflows. If you've ever had parallel subagents take 2 to 5 seconds on tool calls, you'll know that naive state updates suffer from silent data loss.

In our benchmarks with 25 concurrent workers, standard memory overwrites wiped out 24 out of 25 updates. That's 96% data loss! The slowest agent simply stomped everything that'd finished before it. And we couldn't use pessimistic Redis locks because locking across slow LLM inferences tanks your p95 latency and introduces nasty deadlocks.

So I put together a zero-dependency Python engine that pairs Optimistic Concurrency Control with 3-way delta reconciliation:
- Compare-And-Swap tokens. Every state read carries an immutable revision tag so you can track who wrote what.
- 3-way semantic diffs. If an agent tries to commit based on a stale version, our engine compares your base, current, and incoming deltas. If fields don't collide (e.g. your first agent updated sentiment while your second agent updated tags), it auto-merges without re-running your model.
- Exponential backoff retries. For genuine key collisions, it retries asynchronously with fresh state injection so you don't lose data.

In our 25-worker test, agent-occ-lock gave us 0% data loss with 100% state retention and faster throughput than naive locks.

```text
                    ┌──────────────────────────────────┐
                    │       Shared Agent State         │
                    │   (Version: 4, ETag: 'a3f9e2')   │
                    └─────────────────┬────────────────┘
                                      │
               ┌──────────────────────┴──────────────────────┐
               ▼                                             ▼
        Agent 1 (Reads v4)                            Agent 2 (Reads v4)
     [ LLM Call: extracts tags ]                   [ LLM Call: writes summary ]
               │                                             │
               ▼                                             ▼
       CAS Commit: v4 -> v5                          CAS Commit: v4 (STALE!)
          (State becomes v5)                                 │
                                                             ▼
                                                    3-Way Semantic Merge
                                              ┌──────────────────────────────┐
                                              │ Keys orthogonal?             │
                                              │ - summary != tags -> MERGE!  │
                                              │ State becomes v6 (0 reruns)  │
                                              └──────────────────────────────┘
```

| Metric | Naive In-Place Overwrites | `agent-occ-lock` (CAS + 3-Way Merge) |
| :--- | :--- | :--- |
| **Concurrent Workers** | 25 | 25 |
| **Retained Mutations** | 1 / 25 | **25 / 25** |
| **Lost Updates (Silent Loss)** | **24** | **0** |
| **Data Loss Rate (%)** | **96.0%** | **0.0%** |
| **Wall Clock Execution** | 0.0350s | **0.0323s** |

**Stack:** Pure Python (Zero Dependencies) · `asyncio` · `hashlib` · `pytest`

---

### 2. agent-eval-router : Evidence-Driven Control Plane for Agents

[![Repository](https://img.shields.io/badge/GitHub-agent--eval--router-181717?logo=github)](https://github.com/rohith0707/agent-eval-router)

I built `agent-eval-router` because our engineering team kept hitting a brutal bottleneck: your multi-agent setups get wildly expensive and brittle when you rely on blind prompt chaining.

When your agent takes 4 tool steps, a single bad model call ruins your whole run. And routing every single prompt to GPT-4o simply burns our budget without improving our outcomes. It's unsustainable, and you shouldn't have to guess which model works best for you.

So I built a lightweight control plane in Python around three core primitives that we use daily:
- Dynamic policy routing. Instead of sending all 10,000 monthly queries to GPT-4o or Claude 3.5 Sonnet, our router inspects your task complexity and routes between frontier models and fast local models. That cut our inference bills by 68% while keeping our quality rock solid.
- Immutable decision ledger. It records your tool calls, state transitions, and confidence scores into SQLite so you don't lose hours debugging failed trajectories.
- Automated eval gates. We run continuous RAGAS evaluations on our live traces, catching prompt regressions before your code reaches staging so you can deploy with confidence.

In our production runs, we've maintained 94% autonomous tool execution accuracy over 10,000 monthly requests, with a 40% drop in hallucinations across 100-page enterprise files, and our p95 latency held under 1200ms using FastMCP WebSocket bindings.

**Stack:** Python · FastAPI · FastMCP · SQLite · LangSmith · Docker

---

### 3. GenQuery AI : Schema-Aware Text-to-SQL Assistant

[![Repository](https://img.shields.io/badge/GitHub-GenQuery--AI-181717?logo=github)](https://github.com/rohith0707/GenQuery-AI)

At PowerSchool, I built a production text-to-SQL system that translates natural language business questions into secure, optimized analytical queries.

Manual querying was a real drag on our team. Our analysts were spending 4 to 6 hours every day writing complex analytical queries across 50+ Snowflake tables, and we couldn't afford to wait on our engineering team for every single report. It's frustrating when your simple metrics take days to compute.

I designed our end-to-end RAG architecture to fix that:
- Semantic schema parsing. Instead of stuffing full database schemas into your prompt, we didn't waste our tokens; we used FAISS vector embeddings to dynamically retrieve only the relevant tables and foreign key relationships for your specific question.
- Safe execution boundaries. We enforced strict read-only Snowflake roles and ran AST-level syntax validation to eliminate any chance of accidental table mutations or data exfiltration from our database.
- Intelligent caching. Frequent analytical queries were cached in Redis, dropping your query latency from seconds down to sub-100ms.

We've cut our analyst time-to-insight by 60% (dropping 6-hour query workflows down to 2.5 hours), hit 98% query accuracy across 500+ production test cases, and reduced our ad-hoc data engineering support requests by 40%.

**Stack:** Python · LangChain · GPT-4 · Snowflake · FAISS · Redis · Streamlit · AWS EC2

---

## Engineering Principles

1. **Measure Before Optimizing:** An agent that looks impressive in our demo isn't necessarily reliable in our production. We measure our task completion, our latency p95, our token economics, and our regression surfaces upfront.
2. **Deterministic Boundaries Over Free-Form Prompts:** When your security and compliance matter, schema-enforced Pydantic contracts and AST validation outperform fuzzy prompt instructions every time.
3. **Design for Distributed Failure:** Our models timeout, our tools fail, and our concurrent subagents collide. Our production architectures require atomic CAS revision tokens, semantic fallbacks, and immutable audit ledgers so you don't lose your data.
4. **Economics Matter in AI:** Routing every query to a frontier model simply doesn't scale for our budgets. Our dynamic policy gates and tier-based routing preserve your quality while slashing our bills by 60%+.

---

## Production Experience

**AI Engineer - KPMG India**  
Enterprise agent pipelines, multimodal RAG, deterministic safety gates, LangGraph orchestration, handling $500M+ in audited corporate workflows. Zero compliance incidents across all our deployments.

**AI Engineer & Data Engineer - PowerSchool India**  
Production text-to-SQL assistants, semantic schema parsing, big data pipelines, Snowflake optimization, and cloud deployment for our enterprise teams.

---

## Connect

[LinkedIn](https://www.linkedin.com/in/rohithbalsa) · [Email](mailto:balsarohith7@gmail.com) · [GitHub](https://github.com/rohith0707)
