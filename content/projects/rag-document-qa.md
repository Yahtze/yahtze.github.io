---
title: Full-Stack RAG Document QA System
---

**FastAPI, React, Qdrant, Redis, Celery, PostgreSQL** | *Jun 2025* | [GitHub](https://github.com/Yahtze/QA_RAG)

## Overview

A full-stack RAG application. Upload documents, ask questions, get grounded answers with inline citations — all streamed in real time. Engineered a hybrid RAG pipeline using Reciprocal Rank Fusion (RRF) to fuse BM25 and semantic vector search, dynamically filtering candidate documents down to the top-k optimal context chunks for SSE answer streaming.

## Key Contributions

- Engineered a hybrid RAG pipeline using Reciprocal Rank Fusion (RRF) to fuse BM25 and semantic vector search, dynamically filtering candidate documents down to the top-k optimal context chunks for SSE answer streaming.
- Developed an asynchronous distributed ingestion engine using Celery to handle parallel document processing, achieving a throughput of 100+ pages/minute across text extraction, chunking, and 1,536-dim embedding generation.
- Designed a Redis HNSW semantic cache utilizing a two-pass hydration strategy, cutting downstream LLM API costs by serving repetitive queries with a sub-75ms response latency.

## Architecture

```
┌──────────────┐     SSE stream      ┌──────────────────────┐
│   Frontend    │◄────────────────────│      Backend         │
│  React + TS   │────────────────────►│    FastAPI (async)   │
│  Vite / nginx │     REST + JWT      │                      │
└──────────────┘                      ├──────────────────────┤
                                      │  Answer Pipeline     │
                                      │  ├─ Semantic Cache ◄─┼── Redis vector lookup
                                      │  ├─ Lexical (BM25)   │
                                      │  ├─ Semantic (Qdrant)│
                                      │  ├─ RRF Fusion       │
                                      │  ├─ Context Packer   │
                                      │  ├─ Prompt Builder   │
                                      │  ├─ LLM Stream       │
                                      │  └─ Citation Mapper  │
                                      └─────┬────┬────┬─────┘
                                            │    │    │
                              ┌─────────────┘    │    └──────────────┐
                              ▼                  ▼                   ▼
                        ┌──────────┐      ┌───────────┐       ┌──────────┐
                        │ Postgres │      │   Redis   │       │  Qdrant  │
                        │ (data +  │      │  (cache   │       │ (vectors)│
                        │  chunks) │      │  + Celery │       │          │
                        └──────────┘      │  broker)  │       └──────────┘
                                          └─────┬─────┘
                                                │
                                          ┌─────▼─────┐
                                          │   Celery   │
                                          │   Worker   │
                                          │(ingestion) │
                                          └───────────┘
```

## Key Features

- **Upload** PDF, plain text, and Markdown documents (single or batch)
- **Hybrid Retrieval** — BM25 full-text (Postgres `tsvector`) + semantic similarity (Qdrant vectors), fused with Reciprocal Rank Fusion (RRF)
- **Streaming Answers** — SSE with inline citation labels (`[1]`, `[2]`, etc.)
- **Semantic Cache** — Redis vector similarity matching; cached answers use Two-Pass Hydration to guarantee fresh, live citations
- **Multi-turn Chat** — full conversation history chained with each LLM call
- **Active Document Scope** — per-conversation document selection controls RAG participation
- **Reconciliation CLI** — detects stale/missing ingestion states and applies recovery

## Ingestion Pipeline

```
PDF/Text/Markdown Parser
    │
    ▼
Chunker (deterministic character-based)
    │
    ▼
Embedder (text-embedding-3-small, 1536-dim)
    │
    ▼
Store Chunks (Postgres)
    │
    ▼
Store Vectors (Qdrant)
```

## Answer Pipeline

```
Semantic Cache (Redis)
    │
    ├─ hit → hydrate chunk IDs (Two-Pass Hydration)
    │         ├─ Qdrant fetch-by-ID (verify chunks exist)
    │         ├─ Postgres join (fresh filename, page, ACLs)
    │         ├─ success → return cached answer + fresh citations
    │         └─ failure → fall through to full RAG
    │
    ▼ miss
BM25 (Postgres) + Semantic (Qdrant)
    │
    ▼
RRF Fusion → Context Pack → Prompt Build → LLM Stream → Citation Map
    │
    ▼
Persist & Cache (store chunk_ids only) → Stream to Client
```

## Design Decisions

- **Module interfaces are the test boundary** — tests hit service classes directly, not HTTP routes
- **Service seams are mandatory** — all async/API behavior lives behind typed interfaces for swappable adapters
- **Chunk text is source of truth in Postgres** — Qdrant holds vectors only; retrieval joins back for text + metadata
- **Two-Pass Hydration** — semantic cache stores only chunk IDs; on cache hit, citations are hydrated live from Qdrant + Postgres to guarantee freshness
- **Streaming persistence** — user message persisted before stream starts; assistant message + citations persisted after stream completes

## Tech Stack

- **Backend:** FastAPI (async), Celery, SQLAlchemy, Alembic, PostgreSQL
- **Frontend:** React, TypeScript, Vite, Tailwind CSS
- **RAG:** Qdrant, Redis (HNSW cache), BM25, Reciprocal Rank Fusion
- **Embeddings:** OpenAI `text-embedding-3-small` (1536-dim)
- **Infrastructure:** Docker, Nginx
