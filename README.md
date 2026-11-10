# Rohith Balsa

**AI Engineer · Core LLMs, Distributed Multi-Agent Runtimes & Evaluation**  
AI Engineer @ **KPMG India** (Apr 2024 – Present) · Hyderabad, India  
Author of [`agent-occ-lock`](https://github.com/rohith0707/agent-occ-lock) & [`llm-cost-watch`](https://github.com/rohith0707/llm-cost-watch)  

[LinkedIn](https://www.linkedin.com/in/rohithbalsa) · [Email](mailto:balsarohith7@gmail.com) · [Hacker News](https://news.ycombinator.com/submitted?id=rohith0707)

---

## Architecture & Focus

I build production AI systems designed to operate deterministically under enterprise scale: distributed agent state synchronization, bounded runtime control planes, multimodal RAG platforms, and low-latency evaluation pipelines.

At KPMG, I architect enterprise AI platforms serving **10,000+ monthly queries** with **sub-1.2s p95 latency**, maintaining **94% autonomous tool-calling accuracy** and reducing hallucinations by **40%** through continuous RAGAS evaluation gates.

---

## Featured Open-Source Systems

### 1. `agent-occ-lock` — Distributed Multi-Agent State Synchronization
> **Optimistic Concurrency Control (OCC) and 3-way delta reconciliation for parallel agent swarms.**

[![Repository](https://img.shields.io/badge/GitHub-agent--occ--lock-181717?logo=github)](https://github.com/rohith0707/agent-occ-lock)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Dependencies: Zero](https://img.shields.io/badge/dependencies-zero-brightgreen.svg)]()
[![AgentControl Commercial](https://img.shields.io/badge/Commercial%20License-AgentControl%20($149)-00C853.svg)](https://balsarohith.gumroad.com/l/vnztsy)
[![LeadHunter Pro](https://img.shields.io/badge/LeadHunter%20Pro-5,000%20Leads%20($97)-FF90E8.svg)](https://balsarohith.gumroad.com/l/LeadHunter)

When parallel subagents (LangGraph, CrewAI, AutoGen) execute concurrently with 500ms–3000ms model latency, standard in-memory dictionary writes suffer from silent lost updates. The slowest agent overwrites previous mutations. Holding distributed Redis mutexes forces serial execution and spikes p95 latency.

`agent-occ-lock` provides pure Python 3.13 atomic Compare-And-Swap (CAS) revision tokens and 3-way semantic diffs:
- **CAS Revision Tokens**: Every state read yields an immutable revision tag (`version`, `etag`).
- **3-Way Delta Merge**: Reconciles non-colliding key updates automatically without re-invoking the LLM (0 reruns).
- **Jittered Backoff**: Retries genuine key collisions asynchronously with fresh state injection.

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

#### Benchmark (25 Concurrent Subagent Workers)
| Architecture | Retained Updates | Lost Updates (Data Loss) | Data Loss Rate | Wall Clock Time |
| :--- | :--- | :--- | :--- | :--- |
| **Naive In-Place Overwrites** | 1 / 25 | **24** | **96.0%** | 0.0350s |
| **`agent-occ-lock` (CAS + 3-Way Merge)** | **25 / 25** | **0** | **0.0%** | **0.0323s (Sub-35ms)** |

---

### 2. `llm-cost-watch` — MCP Server for LLM Cost & Latency Intelligence
> **Model Context Protocol (MCP 2.x) server that lets Claude debug LLM spend and latency.**

[![Repository](https://img.shields.io/badge/GitHub-llm--cost--watch-181717?logo=github)](https://github.com/rohith0707/llm-cost-watch)
[![Hacker News](https://img.shields.io/badge/Hacker%20News-Show%20HN-orange)](https://news.ycombinator.com/item?id=50415785)
[![MCP 2.x](https://img.shields.io/badge/MCP-2.x%20Ready-blue)](https://modelcontextprotocol.io)

Instead of navigating passive web dashboards, `llm-cost-watch` connects directly to Claude Desktop and Claude Code, enabling conversational performance debugging:
- **`cost_by_feature`**: Ranks subagents and endpoints by dollar spend and token consumption.
- **`slow_calls`**: Isolates p95 and p99 latency outliers and diagnoses bottlenecks (uncompressed context, recursive tool loops).
- **`routing_suggestions`**: Scans for short structured prompts running on expensive frontier models (GPT-4o, Sonnet) and calculates exact dollar savings from routing to Claude 3.5 Haiku or GPT-4o-mini (up to 94% cost reduction, -70% latency).
- **Zero External Egress**: 100% local JSON/SQLite telemetry—no customer data ever leaves your environment.

```python
# 1-line decorator for automated telemetry
@watch_llm(feature="customer_intent_classifier", task_type="classification", model="gpt-4o")
def classify_intent(message: str) -> str:
    return client.chat.completions.create(...)
```

---

### 3. `agent-eval-router` — Deterministic Runtime Control Plane
> **Bounded state machines, decision ledgers, and deterministic evaluation gates for production agents.**

[![Repository](https://img.shields.io/badge/GitHub-agent--eval--router-181717?logo=github)](https://github.com/rohith0707/agent-eval-router)

Autonomous agents degrade into non-deterministic loops when tools return malformed outputs or unexpected schema structures. `agent-eval-router` bounds multi-agent execution loops with deterministic gates:
- **Dynamic Policy Routing**: Routes incoming requests between frontier models and fast local models based on task complexity, cutting token spend by up to 68%.
- **Cryptographic Decision Ledger**: Append-only SHA-256 ledger recording every tool call, state transition, and confidence score for enterprise compliance.
- **Continuous Assertion Gates**: Enforces RAGAS evaluation metrics (Recall@K $\ge$ 0.92, Faithfulness $\ge$ 0.90) directly inside the CI/CD pipeline.

---

### 4. `GenQuery-AI` — Schema-Aware Text-to-SQL Assistant
> **Self-correcting AST validation engine for natural language analytical queries.**

[![Repository](https://img.shields.io/badge/GitHub-GenQuery--AI-181717?logo=github)](https://github.com/rohith0707/GenQuery-AI)

Engineered at PowerSchool to translate natural language into optimized SQL queries across 50+ enterprise Snowflake tables:
- **Semantic Schema Parsing**: Dynamic FAISS vector retrieval of relevant schema subsets to prevent context window bloat.
- **AST Syntax & Security Guardrails**: Pre-execution Abstract Syntax Tree (AST) inspection enforcing read-only constraints and eliminating SQL injection risks.
- **Production Impact**: Reduced analyst time-to-insight by 60% with 98% query accuracy across 500+ production test suites.

---

## Technical Stack & Standards

| Domain | Production Tooling |
| :--- | :--- |
| **Agentic Frameworks** | LangGraph, Multi-Agent Swarms, FastMCP, Model Context Protocol (MCP 2.x), ReAct, Supervisor-Worker |
| **Models & Optimization** | Claude 3.5 Sonnet, Claude 3.5 Haiku, GPT-4o, GPT-4o-mini, Gemini 1.5 Flash, Prompt Caching |
| **Evaluation & Governance** | RAGAS, LangSmith, Golden Evaluation Sets, Bounded State Machines, Decision Ledgers |
| **Backend & Cloud Scale** | Python 3.11/3.13, FastAPI, Docker, Kubernetes (AKS), PostgreSQL, Snowflake, Azure AI Search, Kafka |

---

## Production Experience

* **AI Engineer — KPMG India** *(Apr 2024 – Present)*  
  Architect of enterprise multi-agent platforms and multimodal RAG serving 10,000+ monthly queries with sub-1.2s p95 latency. Enforced 94% autonomous tool precision and 40% hallucination reduction via continuous RAGAS evaluation suites. Neutralized 99.9% of adversarial prompt injections.

* **AI / Data Engineer — PowerSchool India** *(Jul 2023 – Mar 2024)*  
  Shipped self-correcting AST Text-to-SQL agents unblocking 50+ analysts. Built multi-agent QA engines catching 92% of silent data regressions before ingestion.

---

## Get in Touch

I collaborate with engineering leads, CTOs, and researchers building deterministic agentic infrastructure.
- **Email:** [balsarohith7@gmail.com](mailto:balsarohith7@gmail.com)
- **LinkedIn:** [linkedin.com/in/rohithbalsa](https://www.linkedin.com/in/rohithbalsa)
- **Hacker News:** [@rohith0707](https://news.ycombinator.com/user?id=rohith0707)
