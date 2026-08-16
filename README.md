# Rohith Balsa

**AI Engineer · Production LLM & Agentic Systems**

I build AI systems that have to survive after the demo: agent workflows, RAG, tool use, evaluation, guardrails, and the backend/infrastructure around them.

[LinkedIn](https://www.linkedin.com/in/rohithbalsa) · [GitHub](https://github.com/rohith0707)

---

## What I work on

**Agentic AI** — orchestration, tool calling, state, human-in-the-loop workflows  
**LLM engineering** — RAG, structured outputs, context management, model integration  
**Evaluation** — regression tests, failure analysis, quality measurement, tracing  
**Production engineering** — Python, FastAPI, Docker, Kubernetes, AWS  
**Data systems** — SQL, Snowflake, Spark, Kafka, Airflow

---

## Featured project

### GenQuery AI — Natural Language → SQL

[![Repository](https://img.shields.io/badge/GitHub-GenQuery--AI-181717?logo=github)](https://github.com/rohith0707/GenQuery-AI)

A production-oriented LLM application for querying relational data using natural language.

**The interesting part is the system around the model:**

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

Engineering areas covered:

- schema-aware retrieval
- RAG and SQL generation
- multi-model support
- read-only execution
- validation and guardrails
- retries and provider fallback
- semantic caching
- telemetry and feedback
- evaluation / regression testing

**Stack:** Python · LangChain · GPT-4 · RAG · Snowflake

---

## Current build: Agent Evaluation & Routing

I'm building a benchmark-driven system for evaluating and routing LLM/agent workloads.

The goal is not another chatbot. The goal is to make model and agent decisions measurable:

```text
Task
 ↓
Classify
 ↓
Route
 ↓
Execute
 ↓
Evaluate
 ├── Quality
 ├── Latency
 ├── Cost
 └── Reliability
 ↓
Retry / Fallback
```

Planned areas:

- task and model routing
- agent/tool execution
- evaluation datasets
- regression testing
- failure categorization
- tracing
- token and cost measurement
- fallback strategies

This project is being developed as a public engineering artifact, with the benchmark methodology documented alongside the code.

---

## Engineering principles

### 01 — Measure before optimizing

An agent that looks good in a demo is not necessarily good in production. I care about task success, failure modes, latency, cost, and regressions.

### 02 — Prefer deterministic systems where they are enough

Not every workflow needs an autonomous agent. Reliability often comes from knowing where to use an LLM and where not to.

### 03 — Treat evaluation as part of the product

Evaluation is not a final QA step. It belongs next to the application code and deployment pipeline.

### 04 — Design for failure

Tools fail. Retrieval is imperfect. Models produce invalid outputs. Production systems need validation, retries, fallbacks, limits, and observability.

---

## Selected experience

**AI Engineer — KPMG**  
Production agent orchestration, evaluation, human-in-the-loop workflows, reliability, and enterprise AI governance.

**AI Engineer & Data Engineer — PowerSchool**  
Production LLM applications, natural-language-to-SQL, RAG, evaluation, data platforms, and cloud infrastructure.

---

## Technical stack

**AI:** Python · LangGraph · LangChain · RAG · Tool Calling · Structured Outputs · LLM Evaluation  
**Backend:** FastAPI · REST APIs · Docker · Kubernetes · CI/CD  
**Data:** SQL · Snowflake · Apache Spark · Kafka · Airflow  
**Cloud:** AWS · S3 · Lambda · RDS · Glue  
**Observability:** LangSmith · Tracing · Regression Testing · Cost / Latency Tracking

---

## What you'll find here

I use this profile to publish **working systems, benchmarks, experiments, and engineering notes** — not a collection of tutorial clones.

The long-term focus is AI infrastructure and product engineering around:

- agent evaluation
- model routing
- AI observability
- agent reliability
- tool-use safety
- retrieval quality
- inference cost optimization

---

## Connect

[LinkedIn](https://www.linkedin.com/in/rohithbalsa) · [Email](mailto:balsarohith5@gmail.com)
