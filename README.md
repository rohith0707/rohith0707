# Rohith Balsa

**AI Engineer building production LLM and agentic systems.**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Rohith%20Balsa-blue?logo=linkedin&style=flat-square)](https://www.linkedin.com/in/rohithbalsa)
[![GitHub Followers](https://img.shields.io/github/followers/rohith0707?label=Followers&style=flat-square)](https://github.com/rohith0707)

---

## What I build

I work on AI systems where reliability matters as much as model quality: agent orchestration, LLM evaluation, RAG, tool calling, structured outputs, backend services, and the infrastructure around them.

My engineering focus:

- **Agentic AI:** orchestration, tool use, state, human-in-the-loop workflows
- **LLM Engineering:** RAG, structured outputs, context management, model integration
- **Evaluation & Reliability:** regression testing, failure analysis, guardrails, tracing
- **Backend & Infrastructure:** Python, FastAPI, Docker, Kubernetes, AWS
- **Data Systems:** Snowflake, Spark, Kafka, Airflow

---

## Featured work

### GenQuery AI — Natural Language → SQL

A production-style LLM application for querying relational data using natural language.

**Engineering problems explored:**

- Schema-aware retrieval before SQL generation
- Intent → schema retrieval → SQL generation → validation workflow
- Read-only execution and SQL safety controls
- Multi-model support and provider fallback
- Semantic caching and telemetry
- Evaluation and regression testing

**Stack:** Python · LangChain · GPT-4 · RAG · Snowflake · Streamlit

→ [View repository](https://github.com/rohith0707/GenQuery-AI)

---

### Agent Evaluation & Routing Orchestrator

**In progress** — building a benchmark-driven system for routing and evaluating LLM/agent workloads.

The goal is to measure AI systems on the dimensions that matter in production:

**Quality · Latency · Cost · Reliability**

Planned capabilities:

- Task classification and model routing
- Agent/tool execution
- Evaluation datasets and regression tests
- Failure categorization
- Quality, latency and token-cost tracking
- Fallback and retry strategies
- Tracing and observability

The project is intentionally focused on **measuring AI systems**, not just building another chatbot.

---

## How I think about production AI

An LLM call is only one component of an AI system.

```text
Model
  +
Context
  +
Tools
  +
State
  +
Evaluation
  +
Guardrails
  +
Observability
  +
Infrastructure
  =
Production AI System
```

The questions I care about are:

- When should a workflow use an agent, and when is deterministic logic better?
- How do we evaluate an agent before production?
- What happens when retrieval returns the wrong context?
- How should a system recover when a tool or model fails?
- How do we balance quality, latency, and inference cost?
- How do we make AI behaviour observable and regression-testable?

---

## Selected engineering experience

**AI Engineer — KPMG**  
Production agent orchestration, evaluation, human-in-the-loop workflows, AI reliability, and enterprise governance.

**AI Engineer & Data Engineer — PowerSchool**  
Production LLM applications, natural-language-to-SQL, RAG, evaluation, data platforms, and cloud infrastructure.

---

## Technical stack

**AI:** Python · LLM Applications · LangGraph · LangChain · RAG · Tool Calling · Structured Outputs · LLM Evaluation

**Backend:** FastAPI · REST APIs · Docker · Kubernetes · CI/CD

**Data:** SQL · Snowflake · Apache Spark · Kafka · Airflow

**Cloud:** AWS · S3 · Lambda · RDS · Glue

**Observability:** LangSmith · Tracing · Evaluation · Regression Testing · Cost/Latency Tracking

---

## Open source & experiments

I'm interested in contributing to and building around:

- Agent evaluation
- Model routing
- AI observability
- Agent reliability
- Tool-use safety
- Retrieval quality
- Inference cost optimization
- Production AI platforms

More projects will be added here as they become reproducible and public.

---

## Connect

[LinkedIn](https://www.linkedin.com/in/rohithbalsa) · [GitHub](https://github.com/rohith0707)
