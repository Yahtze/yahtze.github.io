---
title: Full-Stack RAG Document QA System
---

> **Disclaimer:** The content on this page was AI-generated from the GitHub repository README and may not be fully accurate. Please refer to the [source repository](https://github.com/Yahtze/QA_RAG) for the most up-to-date information.

**FastAPI, React, Qdrant, Redis, Celery, PostgreSQL** | *Jun 2025* | [GitHub](https://github.com/Yahtze/QA_RAG)

## Overview

A full-stack RAG application where users upload documents, ask questions in natural language, and receive grounded answers with inline citations — all streamed in real time. The system combines lexical and semantic search, fuses them with Reciprocal Rank Fusion, and uses a Redis-based semantic cache to cut LLM costs for repeated queries.

## The Problem

Most document QA systems either rely on keyword matching (BM25) or semantic vector search — but each has blind spots. Keyword search misses semantically relevant passages that don't share exact terms. Vector search can miss precise terminology. And both approaches pay the full LLM cost on every query, even when the answer was already computed for a similar question minutes ago.

## How It Works

**Hybrid Retrieval.** When a question arrives, it's searched against both Postgres full-text (BM25) and Qdrant semantic vectors simultaneously. The results are fused using Reciprocal Rank Fusion (RRF), which combines the ranked lists into a single ordering that captures both lexical precision and semantic relevance. The top-k chunks after fusion become the context for the LLM.

**Semantic Cache with Two-Pass Hydration.** Redis stores cached answers, but instead of storing full citation objects, it stores only chunk IDs. On every cache hit, citations are hydrated live from Qdrant (verifying chunks still exist) and Postgres (fetching fresh filenames, pages, and ACLs). This guarantees citation freshness without sacrificing cache speed. If hydration fails, the system falls through to full RAG — no stale answers are ever served.

**Async Ingestion.** Document processing runs through Celery workers: text extraction → chunking → embedding → vector indexing. This distributed architecture achieves 100+ pages/minute throughput. A reconciliation CLI detects stale or missing ingestion states and applies recovery actions automatically.

**Streaming Answers.** The backend streams LLM responses via SSE with inline citation labels (`[1]`, `[2]`, etc.). Citations are clickable — activating source cards that show the exact passage, page number, and document name.

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

## Design Decisions

The architecture prioritizes modularity and testability. Service seams are mandatory — all async and API behavior lives behind typed interfaces so adapters can be swapped without rewriting UI or route handlers. Tests hit deep modules (service classes) directly, not HTTP routes, using fake embeddings and fake vector stores for deterministic CI.

Chunk text lives in Postgres as the source of truth; Qdrant holds vectors only. Retrieval joins back to Postgres for text and metadata. This separation keeps the vector store lean and ensures that document updates propagate correctly.

Multi-turn conversation history is fully chained with each LLM call — prior questions, context chunks, and answers are all passed so the model can reference earlier turns. Active document scope is managed per-conversation, allowing users to control which documents participate in RAG.

## Tech Stack

- **Backend:** FastAPI (async), Celery, SQLAlchemy, Alembic, PostgreSQL
- **Frontend:** React, TypeScript, Vite, Tailwind CSS
- **RAG:** Qdrant, Redis (HNSW cache), BM25, Reciprocal Rank Fusion
- **Embeddings:** OpenAI `text-embedding-3-small` (1536-dim)
- **Infrastructure:** Docker, Nginx
