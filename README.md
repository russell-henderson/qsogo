# qsogo
Daily ZIP-level signals on permits, filings, and local buzz before anyone else sees them
# QSO GO

---

> **Daily signal intelligence for every ZIP code** — permits, filings, foot-traffic clues, and local buzz distilled into actionable JSON, CSV, and webhook events.

---

## 1 · What problem are we solving?

Hyper-local data is scattered across city clerks, county assessor FTP dumps, agenda PDFs, Craigslist posts, and a dozen other brittle sources. Professionals (realtors, investors, marketers, civic analysts) either miss these weak signals or burn hours babysitting websites.

**QSO GO** turns that chaos into a structured, near-real-time feed you can drop straight into dashboards or ML pipelines.

---

## 2 · How it works (high-level)

```
┌──────────┐  Scrapers   ┌────────────┐  Streaming   ┌─────────────┐  Summaries
│  Source  │────────────▶│  Extract & │─────────────▶│  Kafka /    │───────────▶ API / Webhooks / Airtable
│  Sites   │             │  Normalize │              │  Postgres   │
└──────────┘             └────────────┘              └─────────────┘
             ⤷ PDF -> text       ⤷ geocoding           ⤷ GPT-4 synthesis
```

* **Distributed scrapers**: Python + Scrapy cloud workers (proxy-rotated).
* **Normalization layer**: named-entity extraction, address hygiene, geohash indexing.
* **Event bus**: Kafka → Postgres logical replication for historical replay.
* **LLM summarizer**: GPT-4o prompt-chains convert raw rows into plain-English “daily signal” blurbs.
* **Delivery**: REST, GraphQL, OpenAPI client SDKs, plus Mailgun templated digests.

Everything is built to be **stateless, queue-driven, and horizontally chunkable** so a solo founder can operate it without 3 a.m. pager duty.

---

## 3 · Why it matters

* **First-to-know advantage** – catch zoning changes or teardown permits days before MLS comps update.
* **Democratized civic data** – public info deserves a usable API, not a PDF behind a captcha.
* **Composable** – feed the same stream into a BI tool, a LangChain agent, or a simple SMS alert.

---

## 4 · Tech stack snapshot

| Layer          | Primary tools                                     |
| -------------- | ------------------------------------------------- |
| Scraping & ETL | Python 3.13, Scrapy, Playwright, pdfminer-six     |
| Data store     | Postgres 15 + PostGIS, S3-compatible object store |
| Pipelines      | Airflow on Docker Swarm, Debezium for CDC         |
| Messaging      | Kafka + Redpanda (dev mode)                       |
| API            | FastAPI, asyncpg, GraphQL via Strawberry          |
| AI             | OpenAI GPT-4o, embeddings cached in pgvector      |
| Infra-as-Code  | Terraform, GitHub Actions, Renovate               |

We keep dependencies pragmatic—no vendor lock-in, everything reproducible with a `docker compose up` once the private ops repo is public.

---

## 5 · Roadmap (public milestones)

* **Phase 0 (✅)**  Brand & static wait-list site live.
* **Phase 1**  MVP pipeline for DFW permits + property transfers (ZIP → JSON feed).
* **Phase 2**  Realtime webhooks, GPT daily digest, Stripe billing.
* **Phase 3**  Multi-county expansion, pluggable source SDK, self-serve dashboards.

---

## 6 · Contributing

1. **Open an issue** describing a data source or pipeline improvement.
2. We’ll tag it `good first issue`, `sourcing`, `infra`, or `LLM-prompt` so you can self-select.
3. Fork → feature branch → PR. Include a test harness (`pytest -k yourmodule`).

All discussions happen in GitHub Discussions; architecture proposals in `/docs/adr/` using the MADR template.

---

## License & ownership

This repository is MIT-licensed. Scraped raw datasets are public domain; value-added aggregates remain © QSO GO until open-sourced.

*Build signals, not noise.*
