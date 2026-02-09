# PRECOG project : Quantitative Trading Research Platform

A comprehensive quantitative trading research framework that combines machine learning, cointegration analysis, and backtesting to develop and validate systematic trading strategies.

> **📖 [Complete Mathematical Documentation (PDF)](mathematical_documentation.pdf)** - See [Mathematical Documentation README](MATHEMATICAL_DOCUMENTATION_README.md) for details on all formulas and strategies.

## Project Overview

Project implements a full algorithmic trading pipeline:

1. **Data Acquisition & Cleaning** - Load market data and engineer technical indicators using Kalman filters
2. **Pair Discovery** - Identify cointegrated asset pairs for statistical arbitrage opportunities
3. **Predictive Modeling** - Build XGBoost models to forecast 14-day returns and volatility
4. **Strategy Backtesting** - Simulate trading strategy with realistic transaction costs and rebalancing logic

## Repository Structure

```
<repo>/
├── anonymized_data/                    # 100 anonymized asset CSV files (Asset_001.csv - Asset_100.csv)
├── models/                              # Trained model outputs and predictions
├── outputs/                             # Strategy results and analysis artifacts
│   └── alpha_risk_predictions.csv
├── data-cleaning.ipynb                  # Step 1: Data processing & feature engineering
├── similarity-checking.ipynb            # Step 2: Cointegration pair analysis
├── model-building.ipynb                 # Step 3: XGBoost model training
├── backtesting.ipynb                    # Step 4: Strategy simulation & metrics
├── backtesting_analysis.txt             # Detailed performance report
├── mathematical_documentation.pdf       # 📖 Complete mathematical documentation (21 pages)
├── mathematical_documentation.tex       # LaTeX source for documentation
├── MATHEMATICAL_DOCUMENTATION_README.md # Guide to using the documentation
└── README.md                            # This file
```

## Key Features

### 1. Data Cleaning (`data-cleaning.ipynb`)
- Downloads data from Kaggle PRECOG dataset
- Engineers 8+ technical indicators:
  - RSI with Kalman smoothing
  - Rogers-Satchell volatility
  - 14-day returns (Kalman filtered)
  - Volume z-scores
  - Risk-adjusted momentum

### 2. Similarity Analysis (`similarity-checking.ipynb`)
- Discovers hidden sector structures via correlation clustering
- Identifies cointegrated pairs using Engle-Granger test
- Filters pairs by correlation threshold (0.90+) for efficiency
- Visualizes mean-reverting spread signals

### 3. Model Building (`model-building.ipynb`)
- Trains separate XGBoost models for:
  - **Alpha prediction**: 14-day forward returns
  - **Risk prediction**: 14-day forward volatility
- Walk-forward validation to prevent look-ahead bias
- Signal smoothing (3-day rolling average)
- Inverse volatility weighting for position sizing

### 4. Backtesting (`backtesting.ipynb`)
- Simulates live trading with:
  - Daily rebalancing logic
  - 10 bps transaction costs per trade
  - Sticky rank to minimize turnover
  - Top 10 asset selection by signal strength
- Performance metrics:
  - Sharpe ratio, max drawdown, cumulative returns
  - Transaction cost impact analysis
  - Comparison vs. equal-weight benchmark

## Getting Started

### Prerequisites

- Python 3.8+
- Jupyter Notebook or JupyterLab
- ~2GB disk space for data

### Installation

1. **Clone or download the repository**

2. **Set up a virtual environment** (recommended)
```bash
python -m venv env
source env/bin/activate  # On Windows: env\Scripts\activate
```

3. **Install dependencies**
```bash
uv pip install pyarrow kagglehub numpy pandas scipy scikit-learn pykalman \
    matplotlib seaborn pandas_ta statsmodels xgboost joblib
```

Or with pip:
```bash
pip install pyarrow kagglehub numpy pandas scipy scikit-learn pykalman \
    matplotlib seaborn pandas_ta statsmodels xgboost joblib
```

### Running the Pipeline

**Execute notebooks in this order:**

#### Step 1: Data Preparation
```bash
jupyter notebook data-cleaning.ipynb
```
- Downloads 100 assets from Kaggle
- Generates `features_all_assets.parquet`
- Outputs: Essential feature matrix for modeling

#### Step 2: Pair Discovery (Optional)
```bash
jupyter notebook similarity-checking.ipynb
```
- Explores sector structure and cointegration
- Identifies trading pair opportunities
- Output: Educational insights, no required files for next steps

#### Step 3: Model Training
```bash
jupyter notebook model-building.ipynb
```
- Trains XGBoost models on engineered features
- Generates predictions with smoothing & weighting
- Outputs: `models/alpha_risk_predictions.parquet` and `outputs/alpha_risk_predictions.csv`

#### Step 4: Strategy Backtesting
```bash
jupyter notebook backtesting.ipynb
```
- Simulates trading with realistic costs
- Compares strategy vs. benchmark
- Generates reports and visualizations:
  - `backtesting_results.png` - 4-panel performance dashboard
  - `transaction_cost_impact.png` - Cost sensitivity analysis
  - `backtesting_analysis.txt` - Detailed metrics and insights

## Configuration & Parameters

### Model Parameters (model-building.ipynb)
```python
LOOKAHEAD = 14              # Prediction horizon (days)
params = {
    'n_estimators': 500,    # XGBoost trees
    'learning_rate': 0.01,  # Regularization
    'max_depth': 3,         # Tree depth (prevent overfitting)
    'subsample': 0.7,       # Row sampling
    'colsample_bytree': 0.7 # Feature sampling
}
```

### Strategy Parameters (backtesting.ipynb)
```python
INITIAL_CAPITAL = 1_000_000        # Starting investment
TRANSACTION_COST_BPS = 0.0010      # 10 basis points per trade
TEST_YEARS = 2                     # Backtest duration
TOP_N = 10                         # Number of assets to hold
THRESHOLD = 1.0                    # Minimum score filter
```

Modify these to test sensitivity to different market conditions or trading costs.

## Output Files

| File | Description |
|------|-------------|
| `features_all_assets.parquet` | Clean market data + 8 engineered features |
| `alpha_risk_predictions.parquet` | Model predictions with smoothing & weights |
| `backtesting_results.png` | 4-panel strategy performance dashboard |
| `transaction_cost_impact.png` | Cost sensitivity visualization |
| `backtesting_analysis.txt` | Comprehensive performance summary |

## Key Results Example

Expected performance on test data (2 years):
- **Total Return**: 15-25% (varies by market period)
- **Sharpe Ratio**: 0.8-1.2
- **Max Drawdown**: -8% to -15%
- **Transaction Costs**: ~1-2% of returns

Strategy should outperform equal-weight benchmark on a risk-adjusted basis.

## Technical Details

### Feature Engineering
- **Kalman Filtering**: Smooths noisy indicators while preserving signal
- **Rogers-Satchell Volatility**: High-low estimator less biased than close-based volatility
- **Normalized Features**: Z-scored to handle different asset scales

### Model Architecture
- Separate alpha/risk models for independent optimization
- Walk-forward validation prevents look-ahead bias
- Subsample/colsample prevent overfitting on 100 assets

### Backtesting Logic
- **Sticky Rank**: Keep existing holdings if still in top 15 → reduces churn
- **Inverse Vol Weighting**: Higher weight to lower-volatility assets
- **Rebalancing Buffer**: 0.5% position threshold to avoid micro-trades

## Troubleshooting

**"FileNotFoundError: features_all_assets.parquet"**
→ Run data-cleaning.ipynb first

**"Kaggle authentication error"**
→ Set up Kaggle API key: https://github.com/Kagglehub/kagglehub-client#authentication

**"Models not found" in backtesting**
→ Run model-building.ipynb before backtesting.ipynb

**Memory issues with 100 assets**
→ Reduce to subset: `data[data['Asset'].isin(data['Asset'].unique()[:50])]`

## Performance Considerations

- Full pipeline runtime: ~10-15 minutes (100 assets, 2-year backtest)
- Most time spent in model training (XGBoost walk-forward CV)
- Backtesting is fast (<1 minute)

Tip: Run similarity-checking.ipynb in parallel—it doesn't depend on other notebooks.

## Mathematical Documentation

A comprehensive 21-page LaTeX document is provided that covers all mathematical formulations and strategies:

📖 **[mathematical_documentation.pdf](mathematical_documentation.pdf)** - Complete mathematical reference

### What's Included

- **Data Cleaning & Feature Engineering**: RSI, Rogers-Satchell volatility, Kalman filter equations
- **Cointegration Analysis**: ADF test, Johansen test, hedge ratio estimation, spread construction
- **Machine Learning**: XGBoost configuration, walk-forward CV, signal engineering formulas
- **Backtesting Framework**: Position sizing, transaction costs, Sharpe ratio, maximum drawdown
- **Trading Strategies**: Long-only quant strategy, pairs trading with mean-reversion
- **Complete Parameter Tables**: All hyperparameters and configuration in one place
- **Academic References**: Original papers and software documentation

See [MATHEMATICAL_DOCUMENTATION_README.md](MATHEMATICAL_DOCUMENTATION_README.md) for usage guide and quick formula reference.

## Disclaimer

This is research code for educational purposes. Past performance does not guarantee future results. Trading involves substantial risk of loss. This framework requires extensive validation, optimization, and risk management before any live deployment. Always consult a financial advisor.

## References

- Engle-Granger Cointegration: Statsmodels documentation
- XGBoost: https://xgboost.readthedocs.io/
- Kalman Filtering: https://github.com/rlabbe/filterpy
- Walk-Forward Analysis: De Prado, *Machine Learning for Asset Managers*

---

**Author**: Ayush Subham Moharana
**Last Updated**: February 2026  
**License**: Research/Educational Use
