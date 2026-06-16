---
title: Full-Stack RAG Document QA System
---

**FastAPI, React, Qdrant, Redis, Celery, PostgreSQL** | *Jun 2025* | [GitHub](https://github.com/Yahtze/QA_RAG)

## Overview

Engineered a hybrid RAG pipeline using Reciprocal Rank Fusion (RRF) to fuse BM25 and semantic vector search, dynamically filtering candidate documents down to the top-k optimal context chunks for SSE answer streaming.

## Key Contributions

- Engineered a hybrid RAG pipeline using Reciprocal Rank Fusion (RRF) to fuse BM25 and semantic vector search, dynamically filtering candidate documents down to the top-k optimal context chunks for SSE answer streaming.
- Developed an asynchronous distributed ingestion engine using Celery to handle parallel document processing, achieving a throughput of 100+ pages/minute across text extraction, chunking, and 1,536-dim embedding generation.
- Designed a Redis HNSW semantic cache utilizing a two-pass hydration strategy, cutting downstream LLM API costs by serving repetitive queries with a sub-75ms response latency.

## Tech Stack

- **Backend:** FastAPI, Celery, PostgreSQL
- **Frontend:** React
- **RAG:** Qdrant, Redis (HNSW cache)
- **Search:** BM25, Semantic Vector Search, RRF
