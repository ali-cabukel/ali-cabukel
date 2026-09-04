# Hi, I'm Ali

**Lead AI Engineer — production LLM systems, agents, and ML platforms.**

I build and ship GenAI systems at scale: agentic pipelines, RAG applications, and the MLOps tooling that keeps them running in production. Currently at Vodafone Group in London, where I lead cross-functional delivery teams across customer experience, contact centre, pricing, and sales AI products.

The repos here are where I work through the problems that show up when models meet production — features that must match between training and serving, prompts that have to keep behaving after a model change, and models that fail quietly.

[Build log & notes →](https://ali-cabukel.github.io)

---

## Featured — end-to-end MLOps

**[ecommerce-conversion-pipeline](https://github.com/ali-cabukel/ecommerce-conversion-pipeline)** — real-time purchase-conversion scoring, built as a full production stack rather than a notebook.

Kafka events land session features in Redis via Feast `push`, joined at serve time with batch user/product/seller features materialised from a dbt warehouse. Airflow runs the daily path — dbt → Feast → drift check → train → validate — and blocks promotion when test ROC-AUC falls below threshold. Evidently compares live feature distributions (PSI) against the snapshot written at training time. BentoML serves `/predict`; Prometheus and Grafana track latency, error class, and score distribution.

Point-in-time correct features, offline/online parity, a promotion gate, drift detection, and pre-commit security scanning (bandit, gitleaks, sqlfluff).

`Kafka` · `dbt` · `Airflow` · `Feast` · `Redis` · `MLflow` · `BentoML` · `Evidently` · `Prometheus` · `Grafana` · `DVC`

---

## Libraries

Small, focused tools for production LLM and ML work.

| Project                                                     | What it does                                              |
| ----------------------------------------------------------- | --------------------------------------------------------- |
| **[jsonguard](https://github.com/ali-cabukel/jsonguard)**   | Extract, repair, and validate JSON from LLM responses     |
| **[promptreg](https://github.com/ali-cabukel/promptreg)**   | Pytest-style regression tests for prompts                 |
| **[modeldebug](https://github.com/ali-cabukel/modeldebug)** | Diagnostic checks that explain why an ML model is failing |

## Agents & applications

| Project                                                           | What it does                                                                                                                                                                                                    |
| ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[waggle](https://github.com/ali-cabukel/waggle)**               | Agentic web scraping platform — plan / execute / repair loop over Playwright, crawl4ai, and remote CDP engines, with scheduled runs, a Celery job backend, and a WebSocket chatbot over the scraped store |
| **[tradenet-chat](https://github.com/ali-cabukel/tradenet-chat)** | Conversational agent generating read-only Cypher against a Neo4j trade graph (FastAPI + React, OpenAI or local Ollama)                                                                                          |
| **[tissue-bot](https://github.com/ali-cabukel/tissue-bot)**       | Collects GitHub repo and issue data, then analyses and resolves issues with LangGraph agents (FastAPI + Next.js)                                                                                                |
| **[marti-io](https://github.com/ali-cabukel/marti-io)**           | Multi-agent personal assistant hub (FastAPI + LangGraph)                                                                                                                                                       |

## Data & analysis

| Project                                                         | What it does                                                                    |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **[tradenet](https://github.com/ali-cabukel/tradenet)**         | Bilateral trade data from UN Comtrade, modelled as a network for graph analysis |
| **[policritique](https://github.com/ali-cabukel/policritique)** | UK election results, MPs, and party policy data collected for analysis          |
| **[supamarkt](https://github.com/ali-cabukel/supamarkt)**       | Intraday market data and rule-based trading signals                             |

---

## Stack

**Languages** · Python · SQL · TypeScript

**Backend & data** · FastAPI · Flask · SQLAlchemy · PostgreSQL · SQLite · MongoDB · Neo4j · Redis · Kafka · Celery · Spark

**GenAI** · LangGraph · LangChain · Google ADK · agentic browser automation (Playwright, crawl4ai, CDP) · RAG · vector search (pgvector, Chroma) · LLM-as-judge evaluation · fine-tuning and serving (vLLM, Transformers)

**MLOps** · Kubeflow Pipelines · Airflow · dbt · Feast · MLflow · BentoML · Evidently (drift) · DVC

**Cloud & infra** · GCP (Vertex AI, BigQuery, Cloud Run, GKE, Dataflow, Cloud Data Fusion) · Azure · Docker · Terraform · GitHub Actions · Prometheus · Grafana

**Frontend** · React · Next.js

## Development workflow

**Agentic coding** · Claude Code · Cursor · OpenCode · OpenClaw · Hermes

**Local inference** · Ollama · LM Studio

**Rapid prototyping** · Lovable · v0 · Supabase

I use agents throughout the development lifecycle — scaffolding, refactoring, and test generation — and run models locally when working with data that shouldn't leave the machine.

---

## Elsewhere

[Blog & build notes](https://ali-cabukel.github.io) · [LinkedIn](https://www.linkedin.com/in/ali-cabukel)

Google Cloud Professional — Machine Learning Engineer · Data Engineer · Cloud Architect (held 2023–2025, renewal in progress).
