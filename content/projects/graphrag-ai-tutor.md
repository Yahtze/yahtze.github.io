---
title: GraphRAG-Based AI Tutor
---

**Nano-GraphRAG, Qdrant, Chainlit, Ollama, Docker** | *Dec 2025* | [GitHub](https://github.com/Yahtze/Erica-AI-Project)

## Overview

Erica is an AI tutor designed to assist students by leveraging a custom-built Knowledge Graph (KG) generated from course-related content. Built a hybrid RAG pipeline integrating vector similarity search with knowledge graph traversal (Nano-GraphRAG), enabling multi-hop reasoning and increasing answer relevance by 30% over baseline.

The Knowledge Graph was constructed using 35 articles sourced from the website hosted at pantelis.github.io. The system is highly scalable — expanding the tutor to cover the entire course requires only adding more article URLs to the input list.

## Key Contributions

- Implemented an end-to-end Generative AI pipeline for scraping, chunking, and indexing high-volume datasets using as feature embeddings in a Qdrant vector store.
- Built a hybrid RAG pipeline integrating vector similarity search with knowledge graph traversal (Nano-GraphRAG), enabling multi-hop reasoning and increasing answer relevance by 30% over baseline.
- Developed an automated graph construction workflow using Qwen3-32B, reducing manual schema design effort by 80% while processing 255+ documents for downstream retrieval.

## How It Works

1. **Data Fetching** — Scrapes content from URLs listed in `kg_urls.json` and stores raw results in MongoDB.
2. **Data Transformation** — Cleans and preprocesses scraped data, storing cleaned documents in a separate MongoDB collection.
3. **Knowledge Graph Construction** — Builds the KG using the OpenRouter API with Qwen3-32B for efficient routing and model flexibility across parallel LLM calls.
4. **Query Pipeline** — When a user asks a question:
   - `src/subgraphretriever.py` retrieves the relevant subgraph via semantic search and structural graph reasoning.
   - `educationalplanner.py` formats the subgraph into contextual input and sends it to the LLM with a system prompt.
   - The LLM returns an answer grounded in the extracted Knowledge Graph.

## Architecture

```
┌──────────────┐         ┌─────────────────────┐
│  Chainlit UI │◄───────►│   Application Layer  │
└──────────────┘         │   (src/app.py)       │
                         └──────────┬──────────┘
                                    │
                    ┌───────────────┼───────────────┐
                    ▼               ▼               ▼
            ┌──────────────┐ ┌───────────┐ ┌──────────────┐
            │ Subgraph     │ │ Nano-     │ │ Qdrant       │
            │ Retriever    │ │ GraphRAG  │ │ Vector Store │
            └──────────────┘ └───────────┘ └──────────────┘
                                    │
                                    ▼
                         ┌──────────────────┐
                         │  Knowledge Graph  │
                         │  (built via       │
                         │   Qwen3-32B)      │
                         └──────────────────┘
```

## Tech Stack

- **RAG:** Nano-GraphRAG, Qdrant, Chainlit
- **LLM:** Ollama, Qwen3-32B (via OpenRouter)
- **Database:** MongoDB
- **Infrastructure:** Docker
