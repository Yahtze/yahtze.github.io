---
title: Projects
order: 2
---

A collection of things I've built.

## [[projects/rag-document-qa|Full-Stack RAG Document QA System]]

**FastAPI, React, Qdrant, Redis, Celery, PostgreSQL** | *Jun 2026* | [GitHub](https://github.com/Yahtze/QA_RAG)

- Engineered a hybrid RAG pipeline using Reciprocal Rank Fusion (RRF) to fuse BM25 and semantic vector search, dynamically filtering candidate documents down to the top-k optimal context chunks for SSE answer streaming.
- Developed an asynchronous distributed ingestion engine using Celery to handle parallel document processing, achieving a throughput of 100+ pages/minute across text extraction, chunking, and 1,536-dim embedding generation.
- Designed a Redis HNSW semantic cache utilizing a two-pass hydration strategy, cutting downstream LLM API costs by serving repetitive queries with a sub-75ms response latency.
- Implemented a stateful multi-turn conversation engine with strict per-session document scoping, ensuring isolated user context windows and zero cross-tenant data leakage during vector retrieval.

---

## [[projects/recipe-normalization|Automated Recipe Normalization Engine]]

**Triton, Kubernetes, Ansible, MLflow, HF Accelerate** | *May 2026* | [GitHub](https://github.com/Yahtze/recipe-scraper-mlops)

- Fine-tuned T5 (Small/Base/Large) variants on a 150K recipe dataset, achieving a 0.70 ROUGE-L score in correcting text errors generated via a custom synthetic data corruption pipeline.
- Accelerated multi-GPU training by approx. 2x utilizing FP8 precision over BF16 via the Hugging Face Accelerate library, tracking comprehensive metrics and artifacts seamlessly within MLflow.
- Provisioned a 5-node Kubernetes cluster on Chameleon Cloud via Ansible, reducing infrastructure setup time by 90% (to under 12 minutes) through end-to-end configuration playbooks.
- Deployed models to production via Triton Inference Server to guarantee sub-3s inference latency, monitoring cluster health and performance with Prometheus and Grafana.

---

## [[projects/graphrag-ai-tutor|GraphRAG-Based AI Tutor]]

**Nano-GraphRAG, Qdrant, Chainlit, Ollama, Docker** | *Dec 2025* | [GitHub](https://github.com/Yahtze/Erica-AI-Project)

- Implemented an end-to-end Generative AI pipeline for scraping, chunking, and indexing high-volume datasets using as feature embeddings in a Qdrant vector store.
- Built a hybrid RAG pipeline integrating vector similarity search with knowledge graph traversal (Nano-GraphRAG), enabling multi-hop reasoning and increasing answer relevance by 30% over baseline.
- Developed an automated graph construction workflow using Qwen3-32B, reducing manual schema design effort by 80% while processing 255+ documents for downstream retrieval.

---

## [[projects/basketball-analytics|Basketball Video Analytics Pipeline]]

**Python, PyTorch, OpenCV, Roboflow, SAM2** | *Nov 2025*

- Engineered a Computer Vision pipeline processing 100K+ frames, utilizing PyTorch and OpenCV for player and ball detection with Roboflow models.
- Improved pipeline performance by replacing SAM2-based segmentation with ByteTrack tracking, achieving a 27x speedup (for a 20-minute clip) to resolve compute bottlenecks.
- Added a local RAG-based QA system over 10K+ tokens of game commentary using Ollama and Qdrant for context-aware gameplay insights.
