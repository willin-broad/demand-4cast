# Demand Forecasting System — Setup & Project Guide

> **Purpose:** End-to-end system for predicting future demand, planning inventory, and projecting revenue using Python-based time series analysis and machine learning.

---

## Tech Stack

| Layer | Tool |
|---|---|
| Language | Python 3.10+ |
| Data Manipulation | pandas, numpy |
| Statistical Modeling | statsmodels (ARIMA, decomposition) |
| ML / Forecasting | scikit-learn, Prophet, (optional) neuralforecast for TiDE/TTM |
| Visualization | matplotlib, seaborn, plotly |
| Notebook Environment | Jupyter Lab |
| Dependency Management | pip + virtualenv (or conda) |

---

## Prerequisites

### 1. System Requirements
- Python **3.10 or higher** installed
- pip (comes with Python 3.4+)
- Git (for version control)
- At least **4 GB RAM** (8 GB recommended for large datasets)

### 2. Install Python (if not installed)
Download from [python.org](https://python.org) or use a package manager:
```bash
# macOS (Homebrew)
brew install python@3.11

# Ubuntu/Debian
sudo apt update && sudo apt install python3.11 python3.11-venv python3-pip

# Windows — download installer from python.org
```

### 3. Verify Installation
```bash
python --version     # Should show 3.10+
pip --version
git --version
```

---

## Step 1 — Create the Project Root Folder

```bash
mkdir demand-forecasting-system
cd demand-forecasting-system
```

---

## Step 2 — Set Up a Virtual Environment

Always isolate project dependencies from your global Python installation.

```bash
# Create the virtual environment
python -m venv venv

# Activate it
# macOS / Linux:
source venv/bin/activate

# Windows (CMD):
venv\Scripts\activate.bat

# Windows (PowerShell):
venv\Scripts\Activate.ps1
```

You should see `(venv)` appear at the start of your terminal prompt.

---

## Step 3 — Install All Required Libraries

```bash
pip install --upgrade pip

pip install \
  pandas \
  numpy \
  scikit-learn \
  statsmodels \
  prophet \
  matplotlib \
  seaborn \
  plotly \
  jupyterlab \
  ipykernel \
  python-dotenv \
  openpyxl \
  pyarrow
```

### Optional (for advanced deep learning forecasting / TTM)
```bash
pip install neuralforecast torch
```

### Save dependencies
```bash
pip freeze > requirements.txt
```

---

## Step 4 — Create the Project Folder Structure

Run the following to generate the full directory tree in one command:

```bash
mkdir -p \
  data/raw \
  data/processed \
  data/external \
  notebooks \
  src/ingestion \
  src/preprocessing \
  src/analysis \
  src/models \
  src/evaluation \
  src/visualization \
  outputs/forecasts \
  outputs/plots \
  outputs/reports \
  tests \
  config \
  logs
```

---

## Step 5 — Folder Structure Overview

```
demand-forecasting-system/
│
├── data/
│   ├── raw/                  # Original, unmodified source data (CSV, Excel, DB exports)
│   ├── processed/            # Cleaned and feature-engineered data
│   └── external/             # Third-party data (weather, holidays, economic indicators)
│
├── notebooks/
│   ├── 01_eda.ipynb          # Exploratory Data Analysis
│   ├── 02_decomposition.ipynb # Trend & seasonality analysis
│   ├── 03_arima_model.ipynb  # ARIMA modeling
│   ├── 04_prophet_model.ipynb # Prophet modeling
│   └── 05_forecast_vs_actual.ipynb # Evaluation & comparison
│
├── src/
│   ├── ingestion/
│   │   └── load_data.py      # Data loading from CSV, DB, APIs
│   ├── preprocessing/
│   │   └── clean.py          # Missing values, outliers, date parsing
│   ├── analysis/
│   │   └── eda.py            # Historical demand analysis functions
│   │   └── decomposition.py  # Seasonality & trend decomposition
│   ├── models/
│   │   └── arima_model.py    # ARIMA / SARIMA fitting & forecasting
│   │   └── prophet_model.py  # Prophet model wrapper
│   │   └── baseline.py       # Naive / moving average baselines
│   ├── evaluation/
│   │   └── metrics.py        # MAE, RMSE, MAPE, SMAPE calculations
│   │   └── comparison.py     # Forecast vs actual comparison logic
│   └── visualization/
│       └── plots.py          # Reusable plotting functions
│
├── outputs/
│   ├── forecasts/            # Saved forecast CSVs
│   ├── plots/                # Exported charts (PNG, HTML)
│   └── reports/              # Summaries and business reports
│
├── tests/
│   └── test_models.py        # Unit tests for model outputs
│
├── config/
│   └── config.yaml           # Model params, date ranges, feature flags
│
├── logs/
│   └── pipeline.log          # Runtime logs
│
├── requirements.txt          # All Python dependencies
├── .env                      # Environment variables (API keys, DB strings)
├── .gitignore
└── README.md
```

---

## Step 6 — Initialize Git & Create Boilerplate Files

```bash
git init

# .gitignore
cat > .gitignore <<EOF
venv/
__pycache__/
*.pyc
.env
data/raw/
outputs/
logs/
.DS_Store
*.ipynb_checkpoints
EOF

# README
echo "# Demand Forecasting System" > README.md

# .env placeholder
echo "DB_URL=\nAPI_KEY=" > .env

# Initial commit
git add .
git commit -m "Initial project scaffold"
```

---

## Step 7 — Launch Jupyter Lab

```bash
jupyter lab
```

This opens in your browser at `http://localhost:8888`. Start with `notebooks/01_eda.ipynb`.

---

## Step 8 — System Module Roadmap

Each module below maps to a folder in `src/` and a corresponding notebook.

### Module 1 — Historical Demand Analysis (`src/analysis/eda.py`)
- Load and inspect sales/demand data
- Plot demand over time (daily, weekly, monthly aggregations)
- Detect anomalies, gaps, and outliers
- Compute rolling averages and growth rates

### Module 2 — Seasonality & Trend Decomposition (`src/analysis/decomposition.py`)
- Use `statsmodels.tsa.seasonal.seasonal_decompose` (additive & multiplicative)
- Identify trend, seasonal, and residual components
- Plot decomposition charts per SKU / region / category

### Module 3 — Time Series Forecasting (`src/models/`)
- **ARIMA / SARIMA** via `statsmodels` — good for univariate, stationary series
- **Prophet** via `prophet` — handles holidays, multiple seasonalities, missing data well
- **TiDE / TTM** via `neuralforecast` — for large-scale multivariate deep learning forecasts (optional)
- Train/test split using a rolling or expanding window approach

### Module 4 — Forecast vs Actual Comparison (`src/evaluation/`)
- Compare predicted vs actual demand on a held-out test period
- Compute MAE, RMSE, MAPE per model
- Generate visual overlays (forecast band + actuals)
- Rank models by accuracy for each product / segment

---

## Step 9 — Config File Setup

Create `config/config.yaml` with key parameters:

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
    changepoint_prior_scale: 0.05

evaluation:
  test_periods: 12         # periods to hold out for evaluation
  metrics: [MAE, RMSE, MAPE]

forecast:
  horizon: 26              # periods ahead to forecast
```

---

## Step 10 — Verify Everything Works

Run this quick sanity check from your project root:

```bash
python -c "
import pandas as pd
import numpy as np
import statsmodels.api as sm
from prophet import Prophet
import sklearn
print('pandas:', pd.__version__)
print('numpy:', np.__version__)
print('statsmodels:', sm.__version__)
print('sklearn:', sklearn.__version__)
print('Prophet: OK')
print('All dependencies verified!')
"
```

If all libraries print without errors, your environment is ready.

---

## Quick Reference — Key Library Docs

| Library | Documentation |
|---|---|
| pandas | https://pandas.pydata.org/docs |
| statsmodels | https://www.statsmodels.org/stable |
| Prophet | https://facebook.github.io/prophet |
| scikit-learn | https://scikit-learn.org/stable |
| plotly | https://plotly.com/python |

---

*Next step: Populate `data/raw/` with your historical sales data and begin with `notebooks/01_eda.ipynb`.*
