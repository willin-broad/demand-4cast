# Demand Forecasting System

End-to-end system for predicting future demand, planning inventory, and projecting revenue using Python-based time series analysis and machine learning.

---

## Table of Contents
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Setup & Installation](#setup--installation)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Key Modules](#key-modules)

---

## Tech Stack

| Layer | Tool |
|---|---|
| Language | Python 3.10+ |
| Data Manipulation | pandas, numpy |
| Statistical Modeling | statsmodels (ARIMA, decomposition) |
| ML / Forecasting | scikit-learn, Prophet |
| Visualization | matplotlib, seaborn, plotly |
| Notebook Environment | Jupyter Lab |
| Package Management | pip |

---

## Prerequisites

- **Python 3.10 or higher** installed
- **pip** (comes with Python)
- **Git** for version control
- At least **4 GB RAM** (8 GB recommended for large datasets)

### Verify Installation
```bash
python --version     # Should show 3.10+
pip --version
git --version
```

---

## Setup & Installation

### 1. Activate Virtual Environment
```bash
# macOS / Linux:
source venv/bin/activate

# Windows (CMD):
venv\Scripts\activate.bat

# Windows (PowerShell):
venv\Scripts\Activate.ps1
```

You should see `(venv)` at the start of your terminal prompt.

### 2. Install Dependencies
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### 3. Register Jupyter Kernel (One-time setup)
```bash
python -m ipykernel install --user --name demand-forecasting --display-name "Python (Demand Forecasting)"
```

### 4. Verify Installation
```bash
python -c "
import pandas as pd
import numpy as np
import statsmodels.api as sm
from prophet import Prophet
import sklearn
print('✓ All dependencies verified!')
"
```

---

## Project Structure

```
demand-4cast/
│
├── data/
│   ├── raw/                  # Original, unmodified source data
│   ├── processed/            # Cleaned and feature-engineered data
│   └── external/             # Third-party data (weather, holidays, etc.)
│
├── notebooks/                # Jupyter notebooks for analysis & modeling
│   ├── 01_eda.ipynb
│   ├── 02_decomposition.ipynb
│   ├── 03_arima_model.ipynb
│   ├── 04_prophet_model.ipynb
│   └── 05_forecast_vs_actual.ipynb
│
├── src/
│   ├── ingestion/            # Data loading
│   ├── preprocessing/        # Data cleaning
│   ├── analysis/             # EDA & decomposition
│   ├── models/               # ARIMA, Prophet, baseline models
│   ├── evaluation/           # Metrics & comparisons
│   └── visualization/        # Plotting utilities
│
├── outputs/
│   ├── forecasts/            # Saved forecast CSVs
│   ├── plots/                # Exported charts
│   └── reports/              # Business reports
│
├── config/
│   └── config.yaml           # Model parameters & settings
│
├── tests/                    # Unit tests
├── logs/                     # Runtime logs
├── requirements.txt          # Python dependencies
├── .env                      # Environment variables (API keys, DB strings)
└── .gitignore
```

---

## Getting Started

### 1. Launch Jupyter Lab
```bash
jupyter lab
```
Opens at `http://localhost:8888`

### 2. Start with EDA Notebook
Open `notebooks/01_eda.ipynb` to explore historical demand data.

### 3. Configure Parameters
Edit `config/config.yaml` with your data paths and model settings.

### 4. Run Analysis Pipeline
Follow the embedded notebook sequence:
- **01_eda.ipynb** — Exploratory Data Analysis
- **02_decomposition.ipynb** — Seasonality & Trend Analysis
- **03_arima_model.ipynb** — ARIMA Modeling
- **04_prophet_model.ipynb** — Prophet Modeling
- **05_forecast_vs_actual.ipynb** — Evaluation & Comparison

---

## Configuration

Edit `config/config.yaml` to customize model behavior:

```yaml
data:
  raw_path: "data/raw/sales_data.csv"
  date_column: "date"
  target_column: "demand"
  freq: "W"               # D=daily, W=weekly, M=monthly

model:
  arima:
    order: [1, 1, 1]
    seasonal_order: [1, 1, 1, 52]   # for weekly data with yearly seasonality
  prophet:
    yearly_seasonality: true
    weekly_seasonality: true

evaluation:
  test_periods: 12         # periods to hold out for evaluation
  metrics: [MAE, RMSE, MAPE]

forecast:
  horizon: 26              # periods ahead to forecast
```

---

## Key Modules

| Module | Purpose |
|---|---|
| `src/ingestion/` | Load data from CSV, databases, APIs |
| `src/preprocessing/` | Clean missing values, handle outliers |
| `src/analysis/` | Historical analysis & decomposition |
| `src/models/` | ARIMA, Prophet, baseline forecasting |
| `src/evaluation/` | Accuracy metrics & comparisons |
| `src/visualization/` | Reusable plotting functions |

---

## Documentation

- [Setup Guide](demand_forecasting_setup_guide.md) — Detailed setup instructions
- [statsmodels](https://www.statsmodels.org/stable)
- [Prophet](https://facebook.github.io/prophet)
- [pandas](https://pandas.pydata.org/docs)
- [scikit-learn](https://scikit-learn.org/stable)
