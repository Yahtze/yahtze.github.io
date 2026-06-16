---
title: Automated Recipe Normalization Engine
---

> **Disclaimer:** The content on this page was AI-generated from the GitHub repository README and may not be fully accurate. Please refer to the [source repository](https://github.com/Yahtze/recipe-scraper-mlops) for the most up-to-date information.

**Triton, Kubernetes, Ansible, MLflow, HF Accelerate** | *May 2026* | [GitHub](https://github.com/Yahtze/recipe-scraper-mlops)

## Overview

This project implements an end-to-end ML pipeline built around the open-source recipe manager [Mealie](https://github.com/mealie-recipes/mealie). The system takes scraped recipes from the internet that may contain formatting issues or mistakes and produces neatly formatted, corrected recipes for users to save in their Mealie app. It was developed as a course project for *Machine Learning Systems Engineering & Operations (ECE-GY 9183, Spring 2026, NYU Tandon)*.

## The Problem

Recipes scraped from the web often arrive with inconsistent formatting, encoding errors, or structural mistakes. Manual cleanup is tedious and doesn't scale. This pipeline automates the correction process using fine-tuned language models, turning messy scraped data into clean, structured recipes.

## How It Works

The pipeline centers on T5 models (Small, Base, and Large variants) fine-tuned on a 150K recipe dataset. Training data was generated through a custom synthetic corruption pipeline that introduces realistic text errors, allowing the models to learn correction patterns. The best-performing variant achieved a 0.70 ROUGE-L score on the correction task.

Multi-GPU training was accelerated roughly 2x by leveraging FP8 precision over BF16 via the Hugging Face Accelerate library. All training runs, metrics, and artifacts were tracked through MLflow for reproducibility and comparison.

## Infrastructure

The platform runs on a 5-node Kubernetes cluster provisioned on Chameleon Cloud using Ansible playbooks. This approach reduced infrastructure setup time by 90% — bringing cluster provisioning down to under 12 minutes from scratch. ArgoCD manages GitOps-based deployments, while Argo Workflows orchestrate the training pipeline.

Serving is handled by Triton Inference Server, which guarantees sub-3s inference latency. Cluster health and model performance are monitored through Prometheus and Grafana dashboards.

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
