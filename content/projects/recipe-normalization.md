---
title: Automated Recipe Normalization Engine
---

**Triton, Kubernetes, Ansible, MLflow, HF Accelerate** | *May 2026* | [GitHub](https://github.com/Yahtze/recipe-scraper-mlops)

## Overview

End-to-end ML pipeline built around the open-source recipe manager [Mealie](https://github.com/mealie-recipes/mealie). Takes scraped recipes from the internet that may contain formatting issues or mistakes and produces neatly formatted and corrected recipes. Fine-tuned T5 (Small/Base/Large) variants on a 150K recipe dataset, achieving a 0.70 ROUGE-L score in correcting text errors generated via a custom synthetic data corruption pipeline.

*Course project for Machine Learning Systems Engineering & Operations (ECE-GY 9183, Spring 2026, NYU Tandon).*

## Key Contributions

- Accelerated multi-GPU training by approx. 2x utilizing FP8 precision over BF16 via the Hugging Face Accelerate library, tracking comprehensive metrics and artifacts seamlessly within MLflow.
- Provisioned a 5-node Kubernetes cluster on Chameleon Cloud via Ansible, reducing infrastructure setup time by 90% (to under 12 minutes) through end-to-end configuration playbooks.
- Deployed models to production via Triton Inference Server to guarantee sub-3s inference latency, monitoring cluster health and performance with Prometheus and Grafana.

## Architecture

```
recipe-scraper-mlops/
├── devops/                  # Kubernetes, Helm charts, Ansible, ArgoCD, workflows
│   ├── ansible/             # Cluster provisioning
│   ├── argocd/              # ArgoCD application configs
│   ├── k8s/                 # Helm charts for platform, mealie, argo-workflows
│   ├── mealie-patches/      # Mealie source code modifications
│   └── workflows/           # Argo Workflows for MLflow model promotion
├── serving/                 # Serving layer — Triton, Mealie integration, monitoring
│   ├── mealie_cleaner.py    # Recipe polling and feedback collection
│   └── README.md            # Serving layer setup and operations guide
├── training/                # Model training pipeline
├── data/                    # Scraping and data processing scripts
├── shared/                  # Example inputs/outputs and shared schemas
└── README.md
```

## Argo Workflows

The platform uses Argo Workflows for orchestrating ML training jobs. The training workflow is launched from the Argo Workflows dashboard UI using the `recipe-model-training` template. The pipeline supports parameter overrides for training image, data paths, process count, and MLflow tracking URI.

## Team

| Role | Member |
|------|--------|
| **Training** | Yathin Reddy Duvuru |
| **Serving** | Grace McGrath |
| **Data** | Shruti Sridhar |
| **DevOps/Platform** | Sofia Papathanasiou |

## Tech Stack

- **ML:** T5, Hugging Face Accelerate, MLflow
- **Infrastructure:** Kubernetes, Ansible, Triton Inference Server, ArgoCD, Argo Workflows
- **Monitoring:** Prometheus, Grafana
