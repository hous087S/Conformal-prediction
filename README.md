# Informative Selective Conformal Prediction

> ENSAE Paris · Statistical Modelling Seminar · 2024-2025  
> Authors: Houssem Bouabid · Guy Delloye · Supervised by Ulysse Gazin

![Python](https://img.shields.io/badge/Python-3.10+-blue) ![scikit-learn](https://img.shields.io/badge/scikit--learn-green) ![License](https://img.shields.io/badge/license-MIT-gray)

Implementation of **InfoSP** and **InfoSCOP** from [Gazin et al. (2024)](https://arxiv.org/abs/2410.00914) — methods that select only *informative* prediction intervals (length ≤ 2λ) while controlling the False Coverage Rate (FCR ≤ α).

---

## Overview

Classical conformal prediction outputs intervals for *all* test points — including ones where the model is very uncertain and the interval is too wide to be useful. InfoSP fixes this by only reporting intervals that are actually informative.

| Method | Selectivity | Key idea |
|--------|-------------|----------|
| **InfoSP** | ~75% | Direct BH selection on adjusted p-values |
| **InfoSCOP** | ~95% | Conditional variant — reduces InfoSP's conservatism |

Both methods guarantee **FCR ≤ α** on the selected set under exchangeability.

---

## How it works

1. **Nonconformity scores** on the calibration set: `s(X, Y) = |Y − f̂(X)| / σ̂(X)`
2. **Adjusted p-values** q_i — smallest α such that the conformal interval C^α_i ⊆ [−λ, λ]
3. **Benjamini–Hochberg** selection on {q_i} at level α → selected set S
4. **Adjusted intervals** with α′ = α|S|/m for all i ∈ S

```
FCR = E[ #{i∈S : Yᵢ∉Cᵢ} / |S| ] ≤ α
```

---

## Dataset

[Salary Prediction of Data Professions](https://www.kaggle.com/datasets/abdelrhmantarek37/salary-prediction-of-data-professions) (Kaggle) — 2,639 employees, target: annual salary (USD).

| Split | Size | Role |
|-------|------|------|
| Train | 50% | Fit the Random Forest |
| Calibration | 25% | Compute nonconformity scores |
| Test | 25% | Evaluate InfoSP / InfoSCOP |

---

## Quickstart

```bash
git clone https://github.com/your-username/infosp-salary.git
cd infosp-salary
pip install -r requirements.txt
```

Download the dataset from Kaggle, place the CSV at the project root, then open the notebook:

```bash
jupyter notebook InfoSP_InfoSCOP_notebook.ipynb
```

### Requirements

```
numpy
pandas
scikit-learn
matplotlib
jupyter
```

---

## Results

| Method | \|S\| / m | FCR | Median interval length | Time (s) |
|--------|-----------|-----|------------------------|----------|
| `InfoSP` | 75.3% | 0.087 | ~24,000 | 8.7 |
| `InfoSCOP` | 94.7% | 0.079 | ~22,500 | 10.3 |

Both methods satisfy FCR ≤ α = 0.10 and all intervals respect the 2λ = 31,000 bound.

---

## Project structure

```
infosp-salary/
├── InfoSP_InfoSCOP_notebook.ipynb   # main notebook
├── requirements.txt
├── README.md
└── Salary Prediction of Data Professions.csv
```

---

## Reference

Gazin, U., Durand, G., Blanchard, G., & Roquain, E. (2024). *Selecting Informative Conformal Prediction Sets with False Coverage Rate Control.* [arXiv:2410.00914](https://arxiv.org/abs/2410.00914)
