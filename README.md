# A-HLFDM (Adaptive VMD-based High–Low Frequency Differentiated Modeling)

This repository provides the implementation of **A-HLFDM**, a hybrid soft-computing framework for forecasting highly volatile and non-stationary time series with mixed-frequency dynamics.

A-HLFDM integrates causality-preserving preprocessing, adaptive VMD-based signal decomposition, and frequency-aligned modeling. It automatically selects key VMD hyperparameters (e.g., the number of modes and bandwidth penalty) through a data-driven optimization process, enabling robust extraction of intrinsic mode functions (IMFs). A joint energy–frequency analysis further partitions IMFs into low- and high-frequency components using a cumulative energy criterion.

For forecasting, A-HLFDM adopts a frequency-aligned modeling strategy: low-frequency components are modeled using regression-based approaches to capture long-term trends, while high-frequency components are predicted using dependency-aware models to capture short-term fluctuations. Final forecasts are obtained through linear superposition of component-wise predictions.

Some experimental results are omitted due to GitHub limitations; please contact the authors if needed.

---

## Method Overview

A-HLFDM is a frequency-aware forecasting framework designed for highly volatile and non-stationary time series. The framework follows five stages:

1. **Data Preprocessing**  
   The raw time series is processed using a causality-preserving pipeline. Temporal descriptors (e.g., trailing statistics and differences) are constructed based only on historical observations, and anomalous values are detected using an Isolation Forest. Detected anomalies are corrected using a strictly causal strategy (e.g., trailing median), ensuring robustness without introducing look-ahead bias.

2. **Adaptive VMD Optimization and Decomposition**  
   The purified series is decomposed into intrinsic mode functions (IMFs) using Variational Mode Decomposition (VMD). Key hyperparameters (e.g., the number of modes and bandwidth penalty) are automatically selected via a data-driven optimization process that jointly considers reconstruction accuracy and modal regularity.

3. **High–Low Frequency Partitioning**  
   The extracted IMFs are characterized by both energy and frequency features. After sorting by frequency, a cumulative energy criterion is applied to partition IMFs into low-frequency (trend-dominated) and high-frequency (fluctuation-dominated) components.

4. **Frequency-Aligned Forecasting**  
   Forecasting models are assigned according to frequency characteristics:  
   - Low-frequency components are modeled using regression-based approaches to capture smooth demand evolution.  
   - High-frequency components are modeled using dependency-aware models to capture short-term fluctuations.  
   Forecasting is performed in a rolling one-step-ahead manner with causality preserved.

5. **Forecast Reconstruction**  
   The final prediction is obtained by linearly aggregating the forecasts of all IMFs. This reconstruction preserves multi-scale information and integrates long-term trends with short-term variations.

This design enables effective modeling of mixed-frequency dynamics and provides an accurate, stable, and interpretable solution for forecasting volatile time series.

---

## Repository Structure

```text
A-HLFDM-code/
├── Baseline/                  # Baseline models on Fashion Retail Sales dataset
├── Baseline(Exchange)/        # Baseline models on Exchange Rate dataset
├── A-HLFDM/                       # A-HLFDM method on Fashion Retail Sales dataset
├── A-HLFDM(Exchange)/             # A-HLFDM method on Exchange Rate dataset
└── Preprocessing/             # Data preprocessing scripts
```

---

## Datasets

### 1) Fashion Retail Sales Dataset
Source (Kaggle):  
https://www.kaggle.com/datasets/atharvasoundankar/fashion-retail-sales

### 2) Exchange Rate Dataset
Source (HuggingFace):  
https://huggingface.co/datasets/thuml/Time-Series-Library/tree/main/exchange_rate

---

## Environment

Recommended:
- Python >= 3.8
- numpy
- pandas
- matplotlib
- scikit-learn
- scipy
- torch
- tensorflow
- vmdpy
