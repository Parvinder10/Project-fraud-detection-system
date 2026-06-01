# 🛡️ Explainable Hybrid Fraud Detection System

> **Production-ready fraud detection platform** combining XGBoost, Isolation Forest, and SHAP for explainable, hybrid fraud prediction on financial transactions.

[![Python](https://img.shields.io/badge/Python-3.11-blue)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.111-green)](https://fastapi.tiangolo.com)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.35-red)](https://streamlit.io)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.0-orange)](https://xgboost.readthedocs.io)
[![MLflow](https://img.shields.io/badge/MLflow-2.13-blue)](https://mlflow.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

---

## 📋 Table of Contents

- [Architecture](#architecture)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Quick Start](#quick-start)
- [Dataset Setup](#dataset-setup)
- [Training](#training)
- [API Reference](#api-reference)
- [Dashboard](#dashboard)
- [Docker Deployment](#docker-deployment)
- [Research Experiments](#research-experiments)
- [Resume Description](#resume-description)

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    FRAUD DETECTION SYSTEM                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────────┐  │
│  │  Data Layer  │───▶│  ML Engine   │───▶│  Hybrid Engine   │  │
│  │              │    │              │    │                  │  │
│  │ • Ingest     │    │ • LogReg     │    │ XGBoost (65%)    │  │
│  │ • EDA        │    │ • RandomForest│   │ + IsoForest(35%) │  │
│  │ • SMOTE      │    │ • XGBoost ✓  │    │ = Risk Score     │  │
│  │ • Split      │    │              │    │   [0-100]        │  │
│  └──────────────┘    └──────────────┘    └──────────────────┘  │
│         │                   │                     │            │
│         ▼                   ▼                     ▼            │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────────┐  │
│  │  Feature     │    │  SHAP        │    │  Risk Categories │  │
│  │  Engineering │    │  Explainer   │    │                  │  │
│  │              │    │              │    │ 🟢 Low  (0-25)   │  │
│  │ • Deviation  │    │ • Global     │    │ 🟡 Med  (25-50)  │  │
│  │ • Risk Score │    │ • Local      │    │ 🟠 High (50-75)  │  │
│  │ • Time Feats │    │ • Waterfall  │    │ 🔴 Crit (75-100) │  │
│  └──────────────┘    └──────────────┘    └──────────────────┘  │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                    SERVING LAYER                                │
│                                                                 │
│  ┌──────────────────────┐    ┌──────────────────────────────┐  │
│  │   FastAPI Backend    │    │    Streamlit Dashboard       │  │
│  │   :8000              │    │    :8501                     │  │
│  │                      │    │                              │  │
│  │  POST /predict       │    │  📊 Dashboard Overview       │  │
│  │  POST /explain       │    │  🚨 Fraud Detection          │  │
│  │  POST /risk-score    │    │  📈 SHAP Explainability      │  │
│  │  GET  /health        │    │  🧪 Research Experiments     │  │
│  └──────────────────────┘    │  📉 Analytics                │  │
│                              └──────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✨ Features

| Phase | Feature | Status |
|-------|---------|--------|
| 1 | Data Ingestion, EDA, SMOTE | ✅ |
| 2 | Feature Engineering Pipeline | ✅ |
| 3 | Logistic Regression, Random Forest, XGBoost | ✅ |
| 4 | Isolation Forest Anomaly Detection | ✅ |
| 5 | Hybrid Risk Engine (0-100 score) | ✅ |
| 6 | SHAP Global + Local Explanations | ✅ |
| 7 | Research Experiments + MLflow | ✅ |
| 8 | Streamlit Dashboard (5 pages) | ✅ |
| 9 | FastAPI REST Backend | ✅ |
| 10 | Docker + CI/CD | ✅ |

---

## 🛠️ Tech Stack

| Category | Technology |
|----------|------------|
| **ML** | XGBoost, Scikit-learn, Isolation Forest |
| **Explainability** | SHAP |
| **Imbalance** | SMOTE, RandomUnderSampler (imbalanced-learn) |
| **API** | FastAPI, Uvicorn, Pydantic |
| **Dashboard** | Streamlit, Plotly |
| **Experiment Tracking** | MLflow |
| **Data** | Pandas, NumPy |
| **Deployment** | Docker, Docker Compose |
| **Testing** | Pytest |

---

## 📁 Project Structure

```
fraud-detection-system/
│
├── data/
│   ├── ingest.py              # Phase 1: Data ingestion, EDA, cleaning, split
│   ├── creditcard.csv         # (download from Kaggle)
│   └── PS_...log.csv          # (download from Kaggle)
│
├── pipelines/
│   └── feature_pipeline.py    # Phase 2: Feature engineering transformers
│
├── models/
│   ├── trainer.py             # Phase 3: ML training & comparison
│   ├── anomaly_detector.py    # Phase 4: Isolation Forest
│   ├── hybrid_engine.py       # Phase 5: Hybrid risk engine
│   └── explainer.py           # Phase 6: SHAP explainability
│
├── experiments/
│   └── runner.py              # Phase 7: Research experiments
│
├── dashboard/
│   ├── app.py                 # Phase 8: Streamlit main app
│   └── pages/
│       ├── overview.py        # Dashboard KPIs
│       ├── detection.py       # Live fraud detection
│       ├── shap_page.py       # SHAP visualizations
│       ├── experiments.py     # Experiment results
│       └── analytics.py       # Analytics charts
│
├── api/
│   └── main.py                # Phase 9: FastAPI backend
│
├── utils/
│   ├── config.py              # Centralized configuration
│   ├── logger.py              # Structured logging
│   └── metrics.py             # Evaluation metrics
│
├── tests/
│   └── test_system.py         # Unit & integration tests
│
├── docker/
│   ├── Dockerfile             # Multi-stage Docker build
│   └── docker-compose.yml     # Full stack orchestration
│
├── reports/                   # Auto-generated reports
├── notebooks/                 # Jupyter notebooks
├── train.py                   # Main training entrypoint
├── requirements.txt
├── pyproject.toml
├── .gitlab-ci.yml
└── README.md
```

---

## 🚀 Quick Start

### 1. Clone & Setup

```bash
git clone https://gitlab.com/vishesh-tomar-group/vishesh-tomar-project.git
cd vishesh-tomar-project

# Create virtual environment
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Copy environment config
cp .env.example .env
```

### 2. Dataset Setup

Download datasets from Kaggle and place them in `data/`:

```bash
# Option A: Kaggle CLI
pip install kaggle
kaggle datasets download -d mlg-ulb/creditcardfraud -p data/ --unzip
kaggle datasets download -d ealaxi/paysim1 -p data/ --unzip

# Option B: Manual download
# https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud  → data/creditcard.csv
# https://www.kaggle.com/datasets/ealaxi/paysim1           → data/PS_20174392719_1491204439457_log.csv
```

> **Note:** If datasets are not found, the system automatically generates synthetic data for demo purposes.

### 3. Train Models

```bash
# Full training pipeline (Credit Card dataset)
python train.py --dataset creditcard --imbalance smote

# PaySim dataset
python train.py --dataset paysim --imbalance combined

# Fast run (skip SHAP + experiments)
python train.py --dataset creditcard --skip-shap --skip-experiments
```

### 4. Start API

```bash
uvicorn api.main:app --reload --host 0.0.0.0 --port 8000
# API docs: http://localhost:8000/docs
```

### 5. Start Dashboard

```bash
streamlit run dashboard/app.py
# Dashboard: http://localhost:8501
```

---

## 📊 Dataset Setup

### Credit Card Fraud Dataset
- **Source:** [Kaggle - mlg-ulb/creditcardfraud](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)
- **Size:** 284,807 transactions, 492 frauds (0.172%)
- **Features:** 28 PCA components (V1-V28) + Amount + Time
- **File:** `data/creditcard.csv`

### PaySim Synthetic Dataset
- **Source:** [Kaggle - ealaxi/paysim1](https://www.kaggle.com/datasets/ealaxi/paysim1)
- **Size:** 6.3M transactions, 8,213 frauds (0.13%)
- **Features:** Transaction type, amount, balances
- **File:** `data/PS_20174392719_1491204439457_log.csv`

---

## 🤖 Training Pipeline

```
Phase 1: Data Ingestion
  ├── Load CSV (or synthetic fallback)
  ├── EDA report → reports/eda_report.json
  ├── Remove duplicates & nulls
  └── SMOTE / Undersample / Combined

Phase 2: Feature Engineering
  ├── Amount deviation score
  ├── Log-transformed amount
  ├── Time-based features (hour, night flag)
  ├── Balance error features (PaySim)
  ├── Customer & merchant risk scores
  └── RobustScaler normalization

Phase 3: Model Training
  ├── Logistic Regression
  ├── Random Forest
  ├── XGBoost ← auto-selected as best
  └── Comparison → reports/model_comparison.json

Phase 4: Anomaly Detection
  └── Isolation Forest → models/isolation_forest.pkl

Phase 5: Hybrid Engine
  └── Risk Score = 0.65 × FraudProb + 0.35 × AnomalyScore

Phase 6: SHAP
  ├── Global importance → reports/global_feature_importance.json
  └── SHAP report → reports/shap_report.md

Phase 7: Experiments
  ├── Algorithm comparison
  ├── Imbalance strategy comparison
  ├── Anomaly threshold sweep
  └── Results → experiments/research_results.json
```

---

## 🌐 API Reference

Base URL: `http://localhost:8000`

### `GET /health`
```json
{
  "status": "ok",
  "supervised_model_loaded": true,
  "anomaly_model_loaded": true
}
```

### `POST /predict`
```json
// Request
{
  "features": [0.1, -1.2, 0.5, ...],  // 28 numeric features
  "feature_names": ["V1", "V2", ...]   // optional
}

// Response
{
  "fraud_probability": 0.89,
  "is_fraud": true,
  "risk_score": 82.5,
  "risk_category": "Critical",
  "anomaly_score": 0.74
}
```

### `POST /explain`
```json
// Response
{
  "fraud_probability": 0.89,
  "risk_level": "Critical",
  "top_reasons": [
    "Amount increases fraud risk (SHAP=0.4231)",
    "V14 increases fraud risk (SHAP=0.3812)",
    "V4 decreases fraud risk (SHAP=-0.2100)"
  ],
  "shap_values": [
    {"feature": "Amount", "shap_value": 0.4231},
    ...
  ]
}
```

### `POST /risk-score`
```json
// Response
{
  "risk_score": 82.5,
  "risk_category": "Critical",
  "fraud_probability": 0.89,
  "anomaly_score": 0.74
}
```

---

## 📱 Dashboard Pages

| Page | Description |
|------|-------------|
| **Dashboard** | KPIs, class distribution, model comparison, risk gauge |
| **Fraud Detection** | Live prediction with manual or random input |
| **SHAP Explainability** | Global feature importance, SHAP report |
| **Research Experiments** | Algorithm & strategy comparison charts |
| **Analytics** | Fraud trends, amount ranges, hourly patterns |

---

## 🐳 Docker Deployment

### Single Container

```bash
# Build
docker build -f docker/Dockerfile -t fraud-detection .

# Run API
docker run -p 8000:8000 -v $(pwd)/models:/app/models fraud-detection

# Run Dashboard
docker run -p 8501:8501 fraud-detection \
  streamlit run dashboard/app.py --server.port 8501 --server.address 0.0.0.0
```

### Full Stack with Docker Compose

```bash
cd docker
docker-compose up -d

# Services:
# API:       http://localhost:8000
# Dashboard: http://localhost:8501
# MLflow:    http://localhost:5000

# Stop
docker-compose down
```

---

## 🧪 Testing

```bash
# Run all tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=. --cov-report=html

# Run specific test class
pytest tests/test_system.py::TestHybridEngine -v
```

---

## 📈 Research Experiments

The system runs 3 experiment categories tracked in MLflow:

1. **Algorithm Comparison** - XGBoost (shallow/deep) vs Random Forest (100/300 trees)
2. **Imbalance Strategy** - SMOTE vs Random Under-sampling
3. **Anomaly Threshold Sweep** - Isolation Forest contamination: 0.005, 0.01, 0.02, 0.05

View results:
```bash
# MLflow UI
mlflow ui --port 5000
# Open: http://localhost:5000
```

---

## 🎯 Risk Score Interpretation

| Score | Category | Action |
|-------|----------|--------|
| 0-25 | 🟢 **Low** | Auto-approve |
| 25-50 | 🟡 **Medium** | Flag for review |
| 50-75 | 🟠 **High** | Manual review required |
| 75-100 | 🔴 **Critical** | Block & investigate |

---

## 📄 Resume Description

```
Explainable Hybrid Fraud Detection System | Python, XGBoost, SHAP, FastAPI, Streamlit

• Architected a production-grade fraud detection platform processing financial
  transactions using a hybrid ML approach combining supervised learning (XGBoost)
  with unsupervised anomaly detection (Isolation Forest), achieving ROC-AUC > 0.97
  on the Credit Card Fraud dataset (284K transactions, 0.17% fraud rate).

• Engineered a reusable feature pipeline with 10+ domain-specific features including
  amount deviation scores, customer/merchant risk scores, and time-based behavioral
  signals; handled severe class imbalance using SMOTE and combined sampling strategies.

• Implemented Explainable AI (XAI) using SHAP TreeExplainer to generate global
  feature importance rankings and per-prediction local explanations with human-readable
  fraud reasons (e.g., "Large transaction amount increases fraud risk").

• Built a unified Risk Score (0-100) by combining fraud probability (65% weight) and
  anomaly score (35% weight) into four actionable risk categories: Low, Medium, High,
  and Critical.

• Developed a FastAPI REST backend (/predict, /explain, /risk-score, /health) and a
  5-page Streamlit dashboard with Plotly visualizations for real-time fraud monitoring,
  SHAP explainability, and research experiment comparison.

• Tracked 10+ experiments across algorithm variants, imbalance strategies, and anomaly
  thresholds using MLflow; containerized the full stack (API + Dashboard + MLflow)
  with Docker Compose and implemented CI/CD via GitLab CI.
```

---

## 📜 License

MIT License - see [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- [Credit Card Fraud Dataset](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) by ULB Machine Learning Group
- [PaySim Dataset](https://www.kaggle.com/datasets/ealaxi/paysim1) by Edgar Lopez-Rojas
- [SHAP](https://github.com/slundberg/shap) by Scott Lundberg
