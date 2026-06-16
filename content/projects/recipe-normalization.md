---
title: Automated Recipe Normalization Engine
---

**Triton, Kubernetes, Ansible, MLflow, HF Accelerate** | *May 2026* | [GitHub](https://github.com/Yahtze/recipe-scraper-mlops)

## Overview

Fine-tuned T5 (Small/Base/Large) variants on a 150K recipe dataset, achieving a 0.70 ROUGE-L score in correcting text errors generated via a custom synthetic data corruption pipeline.

## Key Contributions

- Accelerated multi-GPU training by approx. 2x utilizing FP8 precision over BF16 via the Hugging Face Accelerate library, tracking comprehensive metrics and artifacts seamlessly within MLflow.
- Provisioned a 5-node Kubernetes cluster on Chameleon Cloud via Ansible, reducing infrastructure setup time by 90% (to under 12 minutes) through end-to-end configuration playbooks.
- Deployed models to production via Triton Inference Server to guarantee sub-3s inference latency, monitoring cluster health and performance with Prometheus and Grafana.

## Tech Stack

- **ML:** T5, Hugging Face Accelerate, MLflow
- **Infrastructure:** Kubernetes, Ansible, Triton Inference Server
- **Monitoring:** Prometheus, Grafana
