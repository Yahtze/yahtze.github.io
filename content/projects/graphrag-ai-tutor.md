---
title: GraphRAG-Based AI Tutor
---

> **Disclaimer:** The content on this page was AI-generated from the GitHub repository README and may not be fully accurate. Please refer to the [source repository](https://github.com/Yahtze/Erica-AI-Project) for the most up-to-date information.

**Nano-GraphRAG, Qdrant, Chainlit, Ollama, Docker** | *Dec 2025* | [GitHub](https://github.com/Yahtze/Erica-AI-Project)

## Overview

Erica is an AI tutor that helps students engage with course material by combining vector-based retrieval with knowledge graph traversal. Rather than relying on flat document search, Erica builds a structured Knowledge Graph from course content and uses it to answer questions with multi-hop reasoning — connecting ideas across documents that a standard RAG pipeline would miss.

## The Problem

Traditional RAG systems retrieve documents based on semantic similarity, but they struggle with questions that require connecting information from multiple sources. For a course tutor, this is a significant limitation — students often ask questions that span multiple topics or require synthesizing related concepts.

## How It Works

The system is built as a pipeline with four stages:

**Data Ingestion.** Articles are scraped from URLs listed in `kg_urls.json` and stored in MongoDB. The raw content is cleaned, preprocessed, and stored in a separate collection ready for graph construction.

**Knowledge Graph Construction.** The graph is built using Qwen3-32B via the OpenRouter API. OpenRouter was chosen for its efficient routing and model flexibility, which matters because GraphRAG involves a large number of parallel LLM calls. The automated graph construction workflow reduced manual schema design effort by 80% while processing 255+ documents.

**Query-Time Retrieval.** When a student asks a question, the subgraph retriever performs semantic search and structural graph reasoning to find the relevant portion of the knowledge graph. This hybrid approach enables multi-hop reasoning — traversing edges in the graph to connect related concepts.

**Answer Generation.** The retrieved subgraph is formatted into context and sent to the LLM with a system prompt. The LLM returns an answer grounded in the knowledge graph, with citations back to the source material.

## Why GraphRAG?

Standard vector-similarity RAG retrieves documents independently — it finds the most similar chunks but doesn't understand how they relate. By building a knowledge graph, Erica captures explicit relationships between concepts: prerequisites, definitions, examples, and causal links. This structural understanding increased answer relevance by 30% over baseline vector-only retrieval.

The system is also highly scalable. Expanding coverage to additional course material only requires adding more article URLs to the input list — the graph construction and retrieval pipelines handle the rest automatically.

## Tech Stack

- **RAG:** Nano-GraphRAG, Qdrant, Chainlit
- **LLM:** Ollama, Qwen3-32B (via OpenRouter)
- **Database:** MongoDB
- **Infrastructure:** Docker
