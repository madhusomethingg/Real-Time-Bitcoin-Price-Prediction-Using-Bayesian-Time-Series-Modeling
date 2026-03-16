# 📈 Real-Time Bitcoin Price Analysis Using Bayesian Modeling
### Uncertainty-Aware Forecasting in Volatile Financial Markets

> Real-time Bitcoin price analysis powered by PyMC3 and MCMC inference — combining live data ingestion, probabilistic time series modeling, and interactive dashboards to forecast prices with quantified uncertainty.

---

## 📌 Overview 

Predicting cryptocurrency prices is hard. Predicting them *honestly* — with a clear sense of how uncertain those predictions are — is harder. Most forecasting systems give you a number. Bayesian models give you a distribution.

This project builds a real-time Bitcoin analysis pipeline that continuously fetches live price data, fits probabilistic models using PyMC3, and surfaces uncertainty-aware forecasts through interactive visualizations.

| Goal | Approach |
|---|---|
| How can we quantify uncertainty in volatile crypto markets? | Bayesian inference via PyMC3 + MCMC sampling |
| How do we model price dynamics in real time? | Live data ingestion + trend/seasonal decomposition |
| How do we make results interpretable? | Interactive dashboard with posterior predictive intervals |

---

## 🔧 Tech Stack

| Category | Libraries / Tools |
|----------|-----------|
| Data Acquisition | `requests`, `websocket`, CoinGecko API |
| Data Manipulation | `pandas`, `numpy` |
| Bayesian Modeling | `pymc3`, `arviz` |
| Visualization | `plotly`, `matplotlib` |
| Infrastructure | `Docker`, `SQLite` |
| Testing | `pytest` |

---

## 🗂️ Repository Structure

```
📦 bitcoin-price-analysis
 ┣ 📂 src
 ┃ ┣ 📂 data
 ┃ ┃ ┣ 📜 data_fetcher.py      # Live Bitcoin price acquisition
 ┃ ┃ ┣ 📜 data_processor.py    # Cleaning and preprocessing
 ┃ ┃ ┗ 📜 database.py          # Database operations
 ┃ ┣ 📂 models
 ┃ ┃ ┣ 📜 bayesian_model.py    # PyMC3 model definitions
 ┃ ┃ ┣ 📜 time_series.py       # Time series analysis
 ┃ ┃ ┗ 📜 predictive.py        # Price prediction models
 ┃ ┣ 📂 visualization
 ┃ ┃ ┣ 📜 dashboard.py         # Interactive dashboard
 ┃ ┃ ┗ 📜 plots.py             # Visualization utilities
 ┃ ┗ 📂 utils
 ┃   ┣ 📜 config.py            # Configuration settings
 ┃   ┗ 📜 helpers.py           # Utility functions
 ┣ 📂 notebooks
 ┃ ┣ 📜 exploration.ipynb
 ┃ ┗ 📜 analysis.ipynb
 ┣ 📂 tests
 ┃ ┣ 📜 test_data.py
 ┃ ┣ 📜 test_models.py
 ┃ ┗ 📜 test_utils.py
 ┣ 📜 requirements.txt
 ┣ 📜 .env.example
 ┣ 📜 .gitignore
 ┗ 📜 README.md
```

---

## 🔬 Methodology

### 1. Data Collection & Preprocessing
- Fetches live Bitcoin prices via CoinGecko API (REST polling + WebSocket streaming)
- Cleans and validates raw data; structures it into time series using Pandas
- Stores processed data in a local database for historical lookups

### 2. Bayesian Modeling with PyMC3

The core of the project is a probabilistic model that treats price returns as draws from a distribution with unknown mean and volatility — both estimated from data using MCMC sampling.

```python
import pymc3 as pm

def create_price_model(data):
    with pm.Model() as model:
        σ = pm.HalfNormal('σ', sd=1)          # Prior for volatility
        μ = pm.Normal('μ', mu=0, sd=1)         # Prior for mean return
        returns = pm.Normal('returns', mu=μ, sd=σ, observed=data)
        trace = pm.sample(2000, tune=1000, return_inferencedata=False)
    return model, trace
```

Rather than producing a single price forecast, this yields a **posterior distribution** — a range of plausible outcomes with associated probabilities. That's the key advantage over point-estimate models.

### 3. Time Series Decomposition

Price data is decomposed into three components before modeling:

| Component | What It Captures |
|-----------|-----------------|
| **Trend** | Long-term directional movement |
| **Seasonal** | Recurring patterns (daily/weekly cycles) |
| **Residual** | Noise and unexplained variation |

### 4. Model Diagnostics
- MCMC convergence checks (R-hat, effective sample size)
- Prior and posterior predictive checks
- Residual analysis and model comparison metrics

---

## 📊 Key Results

- Successfully ingests and processes live Bitcoin price data in real time
- PyMC3 posterior distributions provide calibrated uncertainty intervals around forecasts
- Time series decomposition isolates trend and seasonal components from noise
- Interactive dashboard displays real-time charts, Bayesian inference plots, and prediction intervals
- Automated price alert system triggers on threshold crossings

---

## ⚠️ Limitations

- Cryptocurrency markets are highly sensitive to external shocks (regulatory news, macro events) that no historical model can anticipate
- MCMC sampling is computationally expensive — real-time inference requires tuning sample sizes carefully
- The current model assumes stationarity in return distributions, which may not hold during high-volatility regimes

---

## 🔮 Future Work

- **Regime detection** — use Hidden Markov Models to identify bull/bear market states and switch model parameters accordingly
- **Hierarchical Bayesian models** — pool information across multiple cryptocurrencies to improve estimates in low-data regimes
- **Online learning** — update posterior distributions incrementally as new data arrives, rather than resampling from scratch

---

## 🚀 Getting Started

```bash
# Clone the repository
git clone https://github.com/madhusomethingg/bitcoin-price-analysis.git

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

1. Copy `.env.example` to `.env` and add your API keys
2. Adjust model parameters in `src/utils/config.py`
3. Run the notebook or launch the dashboard via `src/visualization/dashboard.py`

---

## 👤 Author

Madhumitha Rajagopal
---

## 📄 License

This project is for educational and research purposes.
