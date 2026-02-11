# Nikkei–TOPIX Flow-Based Spread Strategy
Exploratory alpha research using public Japanese market data

---

## 1. Project Overview

This project studies whether participant flow information from TOPIX futures can predict relative performance (leadership) between the Nikkei 225 and TOPIX indices over short horizons (1–5 days).

Rather than forecasting absolute market direction, the strategy focuses on spread returns:

Which index outperforms the other, conditional on observable flow pressure and macro context.

The core idea is that institutional flows enter the market before prices fully adjust, creating temporary and exploitable relative mispricings.

---

## 2. Core Hypotheses

### H1 — Flow–Price Divergence

When foreign investors accumulate TOPIX futures, but TOPIX does not outperform Nikkei on the same day, TOPIX tends to outperform Nikkei over the following days.

This reflects:
- timing mismatch between flow execution and price discovery
- structural differences in index composition and investor usage

### H2 — Macro Coherence (Filter, not Alpha)

The predictive power of flow signals is stronger when the macro narrative is internally consistent, as reflected in daily sell-side outlooks (Resona Market Flash Daily).

Sell-side reports are not used as a standalone signal, but as a regime filter.

---

## 3. Instruments & Targets

### Instruments

- Nikkei 225 — price-weighted, exporter / tech heavy
- TOPIX — market-cap weighted, broad domestic exposure

### Target Variable

spread_return = r_TOPIX - r_Nikkei

Evaluated over:
- T+1
- T+3
- T+5 trading days

This isolates relative leadership while removing broad market beta.

---

## 4. Data Sources

### 4.1 JPX TOPIX Futures Participant Flow (Core)

- Daily participant flow data for TOPIX futures
- Focus on foreign investor net flow
- Publicly released by JPX (CSV / Excel; no public API)

Used to construct:
- flow z-scores
- flow acceleration
- flow–price divergence signals

---

### 4.2 Index Price Data

- Daily close prices for Nikkei 225 and TOPIX
- Used to compute:
  - daily returns
  - cumulative spread returns

---

### 4.3 Resona Market Flash Daily (Secondary)

- Daily PDF reports
- Only two structured sections are used:
  - 本日の注目ポイント
  - 予想レンジ・方向性

Extracted features:
- direction indicators (up / flat / down)
- predicted range width
- macro coherence flags

No black-box sentiment models are used.

---

## 5. Signal Construction (Baseline)

### Flow–Price Divergence Signal

At end of day t:

flow_z_60d > +1.0  
AND  
spread_return_t < 0  (Nikkei outperformed on day t)

Action:
- Enter long TOPIX / short Nikkei at t+1

Signals are evaluated without parameter tuning in the initial phase.

---

## 6. Backtest Assumptions

- Frequency: Daily
- Signal timing: End of day t
- Execution: Next trading day (fixed assumption)
- Holding periods: 1, 3, 5 days
- Positioning: Dollar-neutral index spread
- Transaction costs: Simple fixed assumptions documented in code

---

## 7. Evaluation Metrics

- Mean spread return
- Hit rate
- Information ratio / Sharpe
- Maximum drawdown
- Trade count and turnover
- Subperiod stability (pre/post regime changes)

Signals are discarded early if:
- performance is unstable
- results are driven by a single period
- transaction costs eliminate returns

---

## 8. Project Structure

project/
├── data/
│   ├── raw/        # Original JPX files and PDFs
│   └── clean/      # Parsed and aligned datasets
├── src/
│   ├── data.py     # Data loading and cleaning
│   ├── signals.py  # Signal definitions
│   └── backtest.py # Backtest logic
├── notebooks/      # Exploratory analysis
└── README.md

---

## 9. Non-Goals

This project intentionally does not:
- use intraday data
- trade options (options are expression, not alpha)
- apply machine learning or black-box NLP
- aggressively optimize parameters

The objective is mechanism validation, not curve fitting.

---

## 10. Intended Outcome

The goal is to determine whether:
- public, low-attention flow data contains real, explainable alpha
- relative-value signals can be improved with simple macro conditioning

If successful, the framework may later be extended to:
- alternative flow definitions
- volatility-aware expressions
- options-based implementations

---

## 11. Disclaimer

This project is for research and educational purposes only.
No investment advice is provided.
