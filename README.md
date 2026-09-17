# Hi, I'm Ali

**Lead AI Engineer — production LLM systems, agents, and ML platforms.**

I build and ship GenAI systems at scale: agentic pipelines, RAG applications, and the MLOps tooling that keeps them running in production. Currently at Vodafone Group in London, where I lead cross-functional delivery teams across customer experience, contact centre, pricing, and sales AI products.

The repos here are where I work through the problems that show up when models meet production — features that must match between training and serving, prompts that have to keep behaving after a model change, and models that fail quietly.

[Build log & notes →](https://ali-cabukel.github.io)

---

## Featured

**[ecommerce-conversion-pipeline](https://github.com/ali-cabukel/ecommerce-conversion-pipeline)** — real-time purchase-conversion scoring, built as a full production stack rather than a notebook.

Kafka events land session features in Redis via Feast `push`, joined at serve time with batch user/product/seller features materialised from a dbt warehouse. Airflow runs the daily path — dbt → Feast → drift check → train → validate — and blocks promotion when test ROC-AUC falls below threshold. Evidently compares live feature distributions (PSI) against the snapshot written at training time. BentoML serves `/predict`; Prometheus and Grafana track latency, error class, and score distribution.

Point-in-time correct features, offline/online parity, a promotion gate, drift detection, and pre-commit security scanning (bandit, gitleaks, sqlfluff).

`Kafka` · `dbt` · `Airflow` · `Feast` · `Redis` · `MLflow` · `BentoML` · `Evidently` · `Prometheus` · `Grafana` · `DVC`

**[crashquery](https://github.com/ali-cabukel/crashquery)** — an agentic text-to-SQL system over the UK STATS19 road casualty database, built around the ways text-to-SQL fails in production.

STATS19 stores coded integers with the dictionary in a separate spreadsheet, so the agent has to retrieve metadata rather than pattern-match column names. Schema is exposed as tools instead of pasted into the prompt, so context is paid for on demand. Execution runs as a read-only Postgres role behind a statement timeout, a `sqlglot` parse guard, and an EXPLAIN cost gate — four layers, with the role as the actual boundary and the parser only there for good error messages.

Evaluation scores by execution match against reference SQL, and asserts on behaviour: a case answered correctly without consulting the code dictionary was answered by luck. The gold set targets specific traps — wrong table grain, missing-value codes stored as numbers, a methodology break mid-series.

`LangChain` · `Postgres` · `sqlglot` · `Docker` · `Poetry`

**[hailstorm](https://github.com/ali-cabukel/hailstorm)** — distributed hyperparameter search with Ray, Optuna and XGBoost, demonstrated on NYC taxi tip-percentage prediction.

Ray Data handles Parquet ingest and feature engineering so nothing lands on the driver; Optuna proposes configs by TPE and ASHA prunes bad trials at ~50 boosting rounds instead of 2000, which is where the wall-clock saving comes from. Trials share one `ray.put` copy of the data zero-copy.

The modelling decisions are the point: `total_amount` is excluded because it contains the target (asserted in code, not in a comment), the split is temporal with validation strictly between train and test so early stopping can't leak, and categorical vocabularies are pinned rather than inferred per shard — otherwise integer codes stop meaning the same thing across Ray workers. Two silent distributed failures are documented: the Ray Train / Ray Data CPU deadlock, and ASHA no-op'ing when trials report only once.

`Ray Tune` · `Ray Train` · `Ray Data` · `Optuna` · `ASHA` · `XGBoost`

---

## Libraries

Small, focused tools for production LLM and ML work.

| Project | What it does |
| --- | --- |
| **[jsonguard](https://github.com/ali-cabukel/jsonguard)** | Extract, repair, and validate JSON from LLM responses |
| **[promptreg](https://github.com/ali-cabukel/promptreg)** | Pytest-style regression tests for prompts |
| **[modeldebug](https://github.com/ali-cabukel/modeldebug)** | Diagnostic checks that explain why an ML model is failing |

## Agents & applications

| Project | What it does |
| --- | --- |
| **[threadneedle](https://github.com/ali-cabukel/threadneedle)** | UK macro policy RAG over Bank of England, ONS and HM Treasury sources — Docling parsing, header-aware chunking into Chroma, and a LangGraph tool-calling agent that filters by document edition and pulls live ONS figures rather than quoting stale PDF numbers |
| **[waggle](https://github.com/ali-cabukel/waggle)** | Agentic web scraping platform — plan / execute / repair loop over Playwright, crawl4ai, and remote CDP engines, with scheduled runs, a Celery job backend, and a WebSocket chatbot over the scraped store |
| **[klaxon](https://github.com/ali-cabukel/klaxon)** | Incident management desk — SQLite issue store behind a FastAPI REST surface, the same tools exposed in-process or over MCP stdio, and a LangGraph agent driven by Slack slash commands with HMAC verification and 3-second ack |
| **[tradenet-chat](https://github.com/ali-cabukel/tradenet-chat)** | Conversational agent generating read-only Cypher against a Neo4j trade graph (FastAPI + React, OpenAI or local Ollama) |
| **[tissue-bot](https://github.com/ali-cabukel/tissue-bot)** | Collects GitHub repo and issue data, then analyses and resolves issues with LangGraph agents (FastAPI + Next.js) |
| **[marti-io](https://github.com/ali-cabukel/marti-io)** | Multi-agent personal assistant hub (FastAPI + LangGraph) |

## ML platform & training

| Project | What it does |
| --- | --- |
| **[warp](https://github.com/ali-cabukel/warp)** | Copier template for Vertex AI Pipelines (KFP v2) classification projects on BigQuery ML — generates the pipelines, Terraform for GCS, Artifact Registry, Pub/Sub, Cloud Run, Scheduler and IAM, and Cloud Build triggers for PR checks, apply and release. `copier update` propagates template changes into projects already generated from it |
| **[spider-lora](https://github.com/ali-cabukel/spider-lora)** | LoRA fine-tuning for text-to-SQL on Spider, graded by execution accuracy — result-set comparison rather than string match, with timeouts, value normalisation, and unscorable items quarantined out of the denominator. One config runs on Apple Silicon (MPS) or CUDA |

## Data & analysis

| Project | What it does |
| --- | --- |
| **[tickhouse](https://github.com/ali-cabukel/tickhouse)** | Tick-to-dashboard market data pipeline — Kafka trades consumed by a ClickHouse Kafka engine table, persisted through a materialized view, and rolled into 1-minute OHLCV candles by an AggregatingMergeTree that maintains aggregate states incrementally rather than recomputing. A Strawberry GraphQL API exposes purpose-built fields (candles, VWAP, order-flow imbalance) instead of generic table access, with server-side parameter binding, capped windows and row limits, and a `priceTicks` WebSocket subscription driving a React/urql dashboard. Decimal prices stay strings end to end; volumes use a 64-bit scalar because GraphQL `Int` overflows. Synthetic GBM feed, swappable for a real one |
| **[tradenet](https://github.com/ali-cabukel/tradenet)** | Bilateral trade data from UN Comtrade, modelled as a network for graph analysis |
| **[policritique](https://github.com/ali-cabukel/policritique)** | UK election results, MPs, and party policy data collected for analysis |
| **[supamarkt](https://github.com/ali-cabukel/supamarkt)** | Intraday market data and rule-based trading signals |

---

## Stack

**Languages** · Python · SQL · TypeScript

**Backend & data** · FastAPI · Flask · SQLAlchemy · PostgreSQL · SQLite · MongoDB · Neo4j · ClickHouse · Redis · Kafka · Celery · Spark · GraphQL (Strawberry)

**GenAI** · LangGraph · LangChain · Google ADK · MCP (FastMCP) · text-to-SQL and text-to-Cypher agents · agentic browser automation (Playwright, crawl4ai, CDP) · RAG (Docling, header-aware chunking, incremental indexing) · agent evaluation harnesses · vector search (pgvector, Chroma) · LLM-as-judge evaluation · fine-tuning and serving (vLLM, Transformers)

**ML & distributed training** · XGBoost · scikit-learn · Ray (Tune, Train, Data) · Optuna · ASHA · distributed HPO · LoRA/PEFT fine-tuning (MPS and CUDA)

**MLOps** · Kubeflow Pipelines (KFP v2) · Vertex AI Pipelines · Airflow · dbt · Feast · MLflow · BentoML · Evidently (drift) · DVC · project scaffolding with Copier

**Cloud & infra** · GCP (Vertex AI, BigQuery, BigQuery ML, Cloud Run, GKE, Dataflow, Cloud Data Fusion, Pub/Sub, Cloud Scheduler, Artifact Registry) · Azure · Docker · Terraform · Cloud Build · GitHub Actions · Prometheus · Grafana

**Frontend** · React · Next.js · urql

## Development workflow

**Agentic coding** · Claude Code · Cursor · OpenCode · OpenClaw · Hermes

**Local inference** · Ollama · LM Studio

**Rapid prototyping** · Lovable · v0 · Supabase

I use agents throughout the development lifecycle — scaffolding, refactoring, and test generation — and run models locally when working with data that shouldn't leave the machine.

---

## Elsewhere

[Blog & build notes](https://ali-cabukel.github.io) · [LinkedIn](https://www.linkedin.com/in/ali-cabukel)

Google Cloud Professional — Machine Learning Engineer · Data Engineer · Cloud Architect (held 2023–2025, renewal in progress).
