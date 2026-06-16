---
title: Basketball Video Analytics Pipeline
---

**Python, PyTorch, OpenCV, Roboflow, SAM2** | *Nov 2025*

## Overview

Engineered a Computer Vision pipeline processing 100K+ frames, utilizing PyTorch and OpenCV for player and ball detection with Roboflow models.

## Key Contributions

- Engineered a Computer Vision pipeline processing 100K+ frames, utilizing PyTorch and OpenCV for player and ball detection with Roboflow models.
- Improved pipeline performance by replacing SAM2-based segmentation with ByteTrack tracking, achieving a 27x speedup (for a 20-minute clip) to resolve compute bottlenecks.
- Added a local RAG-based QA system over 10K+ tokens of game commentary using Ollama and Qdrant for context-aware gameplay insights.

## Tech Stack

- **CV:** PyTorch, OpenCV, Roboflow, SAM2, ByteTrack
- **RAG:** Ollama, Qdrant
- **Language:** Python
