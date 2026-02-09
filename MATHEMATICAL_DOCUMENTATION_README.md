# Mathematical Documentation for PRECOG Quant Task

This directory contains comprehensive mathematical documentation for all formulations and strategies used in this quantitative trading research platform.

## Files

### Main Documentation
- **`mathematical_documentation.tex`** - LaTeX source file with all mathematical formulations
- **`mathematical_documentation.pdf`** - Compiled PDF documentation (21 pages)

## What's Included

The LaTeX document provides complete mathematical documentation for:

### 1. **Data Cleaning and Feature Engineering**
- Relative Strength Index (RSI) formula with 14-day window
- Rogers-Satchell volatility estimator using OHLC data
- Kalman filter equations for signal smoothing (Q=0.01, R=1.0)
- Complete engineered feature set with formulas
- Risk-adjusted momentum calculations

### 2. **Similarity Analysis and Cointegration**
- Pearson correlation coefficient for asset screening
- Log-price transformation formulas
- Hedge ratio estimation via OLS regression
- Cointegrated spread construction
- Augmented Dickey-Fuller (ADF) test equations
- Johansen cointegration test methodology
- Distance metrics for clustering

### 3. **Machine Learning Models**
- Walk-forward cross-validation framework
- Target variable construction (14-day forward returns and volatility)
- XGBoost model configuration and hyperparameters
- Loss function (L2 regularization)
- Signal engineering: raw score, smoothing, and inverse volatility weighting
- Complete feature engineering pipeline

### 4. **Backtesting Framework**
- Portfolio construction methodology
- Position sizing formulas (inverse volatility weighting)
- Transaction cost modeling (10 bps)
- Return calculations (daily, cumulative, annualized)
- Risk metrics:
  - Sharpe Ratio
  - Sortino Ratio
  - Maximum Drawdown
  - Average Drawdown
- Performance attribution and benchmark comparison
- Information Ratio

### 5. **Statistical Tests**
- Augmented Dickey-Fuller (ADF) test
- Johansen cointegration test
- Correlation hypothesis testing
- Sharpe ratio significance testing

### 6. **Trading Strategies**
- Main strategy: Long-only quantitative equity strategy
- Alternative: Pairs trading with mean-reversion signals
- Entry/exit conditions with mathematical formulas
- Complete strategy parameters and configuration

### 7. **Performance Metrics**
- Expected performance ranges
- Strategy viability criteria
- Transaction cost impact analysis

## Document Structure

1. **Executive Summary** - Overview of the entire framework
2. **Data Cleaning and Feature Engineering** - All technical indicators
3. **Similarity Analysis and Cointegration** - Statistical arbitrage methods
4. **Machine Learning Models** - XGBoost configuration and signal generation
5. **Backtesting Framework** - Portfolio metrics and performance
6. **Statistical Tests** - Hypothesis testing methodology
7. **Strategy Summary** - Complete trading strategy documentation
8. **Model Configuration** - All parameters in one place
9. **Performance Metrics** - Expected results and criteria
10. **Mathematical Notation** - Reference guide for all symbols
11. **References** - Academic papers and software documentation

## How to Use

### Viewing the PDF
Simply open `mathematical_documentation.pdf` with any PDF reader. The document includes:
- Table of contents with clickable links
- Properly formatted mathematical equations
- Tables summarizing all parameters
- Clear section organization

### Compiling the LaTeX Source

If you need to modify the LaTeX source:

```bash
# Install LaTeX (if not already installed)
sudo apt-get install texlive-latex-base texlive-latex-extra texlive-fonts-recommended

# Compile the document (run twice for TOC)
pdflatex mathematical_documentation.tex
pdflatex mathematical_documentation.tex
```

### Cleaning LaTeX Auxiliary Files

After compilation, you can remove auxiliary files:

```bash
rm -f *.aux *.log *.out *.toc *.gz
```

## Key Formulas Quick Reference

### RSI Calculation
```
RSI_t = 100 - (100 / (1 + RS_t))
where RS_t = AvgGain_t / AvgLoss_t
```

### Kalman Filter
```
K_t = P_{t|t-1} / (P_{t|t-1} + R)  [Kalman Gain]
x̂_{t|t} = x̂_{t|t-1} + K_t(z_t - x̂_{t|t-1})
```

### Trading Score
```
TradeScore_t = MA_3(pred_return_t / (pred_risk_t + ε))
```

### Sharpe Ratio
```
Sharpe = (μ_r / σ_r) × √252
```

### Maximum Drawdown
```
MDD = min_t (PV_t / max_{s≤t} PV_s - 1)
```

## Mathematical Notation

- `t` = Time index
- `μ` = Mean
- `σ` = Standard deviation
- `ρ` = Correlation coefficient
- `β` = Regression coefficient/hedge ratio
- `K_t` = Kalman gain
- `PV_t` = Portfolio value at time t
- `ε` = Small constant (epsilon)

## References Included

The document includes comprehensive references to:
- Original papers (Wilder, Rogers-Satchell, Engle-Granger, etc.)
- Kalman filtering literature
- Machine learning resources (XGBoost, De Prado)
- Portfolio management (Sharpe, Sortino)
- Software documentation (Statsmodels, PyKalman, etc.)

## Educational Use

This documentation is designed for:
- Understanding the mathematical foundations of the trading system
- Academic review and validation
- Implementation reference
- Research and development
- Educational purposes

## Updates and Maintenance

The LaTeX source can be updated to reflect:
- New features or indicators
- Modified parameters
- Additional strategies
- Enhanced risk models
- New performance metrics

Simply edit `mathematical_documentation.tex` and recompile.

## License

This documentation is for educational and research purposes only. See main repository README for full disclaimer.

---

**Created:** February 2026  
**Format:** LaTeX/PDF  
**Pages:** 21  
**Author:** PRECOG Quant Task Project
