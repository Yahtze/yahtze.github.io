---
title: Basketball Video Analytics Pipeline
---

> **Disclaimer:** The content on this page was AI-generated and may not be fully accurate.

**Python, PyTorch, OpenCV, Roboflow, SAM2** | *Nov 2025*

## Overview

This project processes basketball game footage into structured analytics, combining computer vision for player/ball detection with a RAG-based question-answering system over game commentary.

## The Problem

Analyzing basketball game footage manually is time-consuming and subjective. Coaches and analysts need fast, automated ways to detect player positions, track ball movement, and extract insights from both the visual feed and broadcast commentary.

## How It Works

The pipeline processes over 100K video frames using PyTorch and OpenCV for player and ball detection through Roboflow models. The initial approach used SAM2-based segmentation, but this proved computationally expensive. By switching to ByteTrack for object tracking, the pipeline achieved a 27x speedup on a 20-minute clip — turning what was a compute bottleneck into a practical, real-time-capable system.

On top of the visual detection layer, a local RAG-based QA system was built over 10K+ tokens of game commentary. Using Ollama for local LLM inference and Qdrant for vector search, the system can answer natural-language questions about gameplay, drawing context from both the detected visual events and the broadcast commentary.

## Tech Stack

- **CV:** PyTorch, OpenCV, Roboflow, SAM2, ByteTrack
- **RAG:** Ollama, Qdrant
- **Language:** Python
