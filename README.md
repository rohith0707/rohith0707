# Rohith Balsa

**Applied AI Engineer · Production LLM, Agentic Systems & AI Evaluation**

I build AI systems that have to work after the demo — agent workflows, RAG, tool use, evaluation, reliability controls, and the backend infrastructure around them.

My focus is the engineering layer between foundation models and reliable AI products: **how to choose a model, evaluate an agent, handle failure, control cost/latency, and make the system observable.**

[LinkedIn](https://www.linkedin.com/in/rohithbalsa) · [GitHub](https://github.com/rohith0707)

---

## What I build

**Agentic systems** — orchestration, tool calling, state, human-in-the-loop workflows  
**LLM applications** — RAG, natural-language-to-SQL, structured outputs, context engineering  
**AI evaluation** — regression suites, failure analysis, quality measurement, model comparison  
**AI infrastructure** — routing, reliability, observability, latency and cost controls  
**Production engineering** — Python, FastAPI, Docker, Kubernetes, AWS  
**Data systems** — SQL, Snowflake, Spark, Kafka, Airflow

---

## Featured work

### Agent Eval Router

**Python-first evaluation and routing infrastructure for LLM and agent workloads.**

[![Repository](https://img.shields.io/badge/GitHub-Agent--Eval--Router-181717?logo=github)](https://github.com/rohith0707/agent-eval-router)

The project explores a production question:

> **Given a task and explicit quality, latency, and cost constraints, which model or agent should run it — and can we measure and explain that decision?**

```text
Task
  ↓
Task requirements
  ↓
Routing policy
  ↓
Candidate models / agents
  ↓
Execution
  ↓
Evaluation
  ├── Quality
  ├── Reliability
  ├── Latency
  ├── Cost
  └── Failure class
  ↓
Decision + Trace
  ↓
Benchmark / Regression feedback
```

The goal is **not another chatbot**. It is a measurable control layer for AI systems.

**Stack:** Python · FastAPI · Pydantic · Next.js · PostgreSQL · Docker

---

### GenQuery AI — Natural Language → SQL

[![Repository](https://img.shields.io/badge/GitHub-GenQuery--AI-181717?logo=github)](https://github.com/rohith0707/GenQuery-AI)

An LLM application for querying relational data using natural language, designed around the system surrounding the model rather than a single prompt.

```text
Question
   ↓
Intent Understanding
   ↓
Schema Retrieval
   ↓
SQL Generation
   ↓
Validation / Safety
   ↓
Read-only Execution
   ↓
Result
```

Engineering areas:

- schema-aware retrieval and RAG
- natural-language-to-SQL
- multi-model support
- read-only execution
- validation and guardrails
- retries and provider fallback
- semantic caching
- telemetry and feedback
- evaluation / regression testing

**Stack:** Python · LangChain · GPT-4 · RAG · Snowflake

---

## Engineering principles

### 01 — Measure before optimizing

An agent that looks good in a demo is not necessarily good in production. I care about task success, failure modes, latency, cost, reliability, and regressions.

### 02 — Use an agent only when it adds value

Not every workflow needs autonomy. Deterministic logic is often the better engineering choice when the problem is well-defined.

### 03 — Evaluation is part of the product

Datasets, graders, regression cases, and failure analysis belong beside the application and deployment pipeline — not in a spreadsheet after launch.

### 04 — Design for failure

Models produce invalid outputs. Retrieval misses context. Tools time out. Providers rate-limit. Production systems need validation, budgets, retries, fallbacks, and observability.

### 05 — Optimize for outcomes

The objective is not a better demo or a larger benchmark number. It is a more useful, reliable, and economically viable AI product.

---

## Selected experience

**AI Engineer — KPMG**  
Production agent orchestration, evaluation, human-in-the-loop workflows, reliability, and enterprise AI governance.

**AI Engineer & Data Engineer — PowerSchool**  
Production LLM applications, natural-language-to-SQL, RAG, evaluation, data platforms, and cloud infrastructure.

---

## Technical focus

**AI:** Python · LangGraph · LangChain · RAG · Tool Calling · Structured Outputs · LLM Evaluation  
**Backend:** FastAPI · REST APIs · Docker · Kubernetes · CI/CD  
**Data:** SQL · Snowflake · Apache Spark · Kafka · Airflow  
**Cloud:** AWS · S3 · Lambda · RDS · Glue  
**Observability:** LangSmith · Tracing · Regression Testing · Cost / Latency Tracking

---

## What you'll find here

Working systems, evaluation experiments, benchmarks, architecture notes, and engineering trade-offs.

I am particularly interested in:

- agent evaluation
- model routing
- AI observability
- agent reliability
- tool-use safety
- retrieval quality
- inference cost optimization
- production AI platforms

---

## Current direction

**Applied AI → Agentic Systems → Evaluation → AI Infrastructure**

The goal is to build AI systems that can be **measured, debugged, improved, and trusted in production.**

---

## Connect

[LinkedIn](https://www.linkedin.com/in/rohithbalsa) · [Email](mailto:balsarohith5@gmail.com)
