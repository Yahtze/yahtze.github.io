---
title: Solar Power Forecasting with LSTM & XAI
---

> **Disclaimer:** The content on this page was AI-generated from the GitHub repository READMEs and may not be fully accurate. Please refer to the source repositories for the most up-to-date information: [PV-Forecast-LSTM-Genetic-Algorithm-XAI](https://github.com/Yahtze/PV-Forecast-LSTM-Genetic-Algorithm-XAI), [Photovoltaic-Cell-Power-Forecasting-Using-LSTM-With-XAI-Integration](https://github.com/Yahtze/Photovoltaic-Cell-Power-Forecasting-Using-LSTM-With-XAI-Integration).

**PyTorch, LSTM, XAI, Genetic Algorithms** | *Aug 2024 – May 2025*

## Overview

A two-phase research project on photovoltaic power forecasting. Phase 1 established LSTM-based GHI forecasting with SHAP interpretability, resulting in a published paper. Phase 2 extended the approach with Genetic Algorithm hyperparameter optimization, pushing RMSE further across multiple forecast horizons.

## Published Paper

Duvuru, Y.R., Mahadev, S., Saranya, P. (2026). *Photovoltaic Cell Power Forecasting Using LSTM with XAI Integration*. In: Senjyu, T., So-In, C., Joshi, A. (eds) **Smart Trends in Computing and Communications**. SmartCom 2025. Lecture Notes in Networks and Systems, vol 1463. Springer, Singapore. [https://doi.org/10.1007/978-981-96-7514-2_22](https://doi.org/10.1007/978-981-96-7514-2_22)

## Phase 1: LSTM + XAI

The first phase focused on forecasting Global Horizontal Irradiance (GHI) at four operational horizons: 15, 30, 45, and 60 minutes. The preprocessing pipeline handles missing-value removal, outlier detection, clear-sky GHI calculations, and nighttime filtering — producing clean time-series data suitable for sequence modeling.

LSTM networks were chosen for their ability to capture temporal dependencies in solar irradiance patterns. A persistence model (predicting the current value as the forecast) served as the baseline. SHAP was integrated for interpretability, enabling feature impact analysis and visual explanations of model behavior. This XAI component achieved a 40% reduction in training time by guiding downstream feature selection — identifying which input features actually matter for each forecast horizon.

The project was awarded the Department Silver Medal at SRM Research Day 2025.

## Phase 2: GA-Optimized LSTM

The second phase introduced Genetic Algorithm optimization to search for better LSTM configurations. Rather than relying on manual hyperparameter tuning, GA encodes candidate configurations (sequence length, model size, training settings) and iteratively improves them through selection, crossover, and mutation — optimizing directly for RMSE.

This approach explores a broader hyperparameter space than grid or random search and found configurations that reduced RMSE by an additional 4% beyond the manually-tuned Phase 1 models. The GA-optimized pipeline was evaluated across five forecast horizons: 10, 20, 40, 60, and 120 minutes.

XAI analysis was applied to the GA-optimized models as well, providing transparency into which input factors most influence forecasts at each horizon — critical for operational trust in grid scheduling and storage control applications.

## Tech Stack

- **ML:** PyTorch, LSTM, XAI (SHAP, Captum)
- **Optimization:** Genetic Algorithms
- **Data:** NSRDB (NREL), solar irradiance time-series
- **Domain:** Photovoltaic Power Forecasting, Grid Scheduling
