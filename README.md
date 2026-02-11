# Nikkei-TOPICS-Margin
# Nikkei–TOPIX Flow-Based Spread Strategy
*Exploratory alpha research using public Japanese market data*

---

## 1. Project Overview

This project studies whether **participant flow information from TOPIX futures** can predict **relative performance (leadership)** between the **Nikkei 225** and **TOPIX** indices over short horizons (1–5 days).

Rather than forecasting absolute market direction, the strategy focuses on **spread returns**:

> Which index outperforms the other, conditional on observable flow pressure and macro context.

The core idea is that **institutional flows enter the market before prices fully adjust**, creating temporary and exploitable relative mispricings.

---

## 2. Core Hypotheses

### H1 — Flow–Price Divergence
When **foreign investors accumulate TOPIX futures**, but **TOPIX does not outperform Nikkei on the same day**, TOPIX tends to outperform Nikkei over the following days.

This reflects:
- timing mismatch between flow execution and price discovery
- structural differences in index composition and investor usage

### H2 — Macro Coherence (Filter, not Alpha)
The predictive power of flow signals is stronger when the **macro narrative is internally consistent**, as reflected in daily sell-side outlooks (Resona Market Flash Daily).

Sell-side reports are **not used as a standalone signal**, but as a **regime filter**.

---

## 3. Instruments & Targets

### Instruments
- **Nikkei 225** — price-weighted, exporter / tech heavy
- **TOPIX** — market-cap weighted, broad domestic exposure

### Target Variable
```text
spread_return = r_TOPIX − r_Nikkei
