# market-neutral-mf-ls


# Market-Neutral Multi-Factor Long/Short Strategy (US Large-Cap)

A systematic equity strategy that constructs a weekly rebalanced, market-neutral portfolio of liquid US large-cap stocks using a blended multi-factor signal.

## Overview

This project implements a production-style research pipeline for cross-sectional equity trading:

* Universe filtering by liquidity and price
* Factor construction and normalization
* Composite signal generation
* Portfolio construction with exposure constraints
* Transaction cost modeling
* Walk-forward validation
* Robustness testing

The goal is to isolate true alpha independent of market direction.

---

## Strategy Summary

**Style:** Market-neutral long/short
**Universe:** Liquid US large-cap equities
**Rebalance:** Weekly
**Execution:** Monday close signal → Tuesday open trade
**Gross Exposure:** 150% (75% long / 75% short)
**Net Exposure:** ~0%
**Position Cap:** 1.5% per name
**Min Positions:** ≥ 50 per side

---

## Factors

All factors are cross-sectionally winsorized and z-scored.

### 1) Short-Term Reversal

Captures short-horizon overreaction.

* Signal: 5-day return (contrarian)
* Rationale: Temporary price dislocations revert

---

### 2) Medium-Term Momentum (12-1)

Classic momentum anomaly.

* Signal: 12-month return excluding most recent month
* Rationale: Underreaction to information

---

### 3) Low Volatility

Quality / defensive tilt.

* Signal: Negative 20-day realized volatility
* Rationale: Low-risk anomaly

---

### 4) Volume Shock Interaction

Capitulation detection.

* Signal: Volume surprise × sign(−1-day return)
* Interpretation: High volume sell-offs → potential bounce

---

## Portfolio Construction

1. Rank stocks by composite score
2. Long top quantile, short bottom quantile
3. Enforce constraints:

* Market neutrality (beta near zero)
* Sector balance
* Position caps
* Liquidity limits

---

## Backtesting Methodology

### Data

* Source: Yahoo Finance (`yfinance`)
* Prices: Adjusted close (total return)
* Execution proxy: Next-day open
* Liquidity: 20-day dollar ADV

### Costs

* 4 bps per trade one-way
* 50 bps/year borrow cost on shorts

### Validation

Walk-forward testing:

* 3-year training window
* 1-year out-of-sample test
* Rolling annually (2016–2024)

---

## Robustness Tests

* Factor ablation (each factor individually)
* Weight perturbation
* Cost sensitivity analysis
* Randomized signal placebo test
* Look-ahead leakage test
* Bootstrap confidence intervals

---

## Key Results (Illustrative)

* Moderate Sharpe ratio after costs
* Performance sensitive to momentum factor
* Drawdowns significant during stress regimes
* Statistical significance limited under free-data constraints

⚠️ Results should be interpreted cautiously due to survivorship bias and data limitations.

---

## Limitations

* Survivorship bias (current constituents only)
* No point-in-time sector classifications
* Simplified execution model
* Flat borrow cost assumption
* No market impact model
* Limited universe compared to institutional datasets

---

## Repository Structure

```
.
├── data_cache_v2.pkl      # Cached market data
├── strategy.py            # Main research/backtest script
├── output_v3/             # Results and charts
└── README.md
```

---

## Requirements

Python 3.9+

Install dependencies:

```bash
pip install pandas numpy yfinance scikit-learn statsmodels matplotlib
```

---

## How to Run

```bash
python strategy.py
```

Outputs:

* Performance report
* Robustness diagnostics
* Visualization dashboard

---

## Intended Use

Educational and research purposes only.

This project demonstrates a systematic approach to quantitative equity strategies and is not investment advice.

---

## Author

ZiYi Hong
Econometrics · Quantitative Finance · Systematic Trading
