# Optimizing ESG Portfolios

**Jorge Hernandez and Enrique Villamor**
*The Journal of Impact and ESG Investing*, Spring 2026

---

## Overview

This paper develops a rolling-window portfolio optimization framework that incorporates ESG constraints into a unified Black-Litterman / MGARCH / quadratic-programming pipeline. Black-Litterman expected returns are formed from 64-day rolling average return differentials (tau = 0.025) combined with a market-cap-weighted equilibrium prior. A DCC-MGARCH(1,1) model provides the one-step-ahead covariance matrix at each rebalancing date. Three nested quadratic programs are then solved: a baseline model with no ESG filter (unconstrained Lagrangian), an ESG long-short model that enforces a mean deficiency ceiling via a closed-form two-multiplier Lagrangian, and an ESG long-only model solved numerically with sign and bound constraints. The framework is backtested over 16 out-of-sample trades (64-trading-day rebalancing intervals, July 2010 to May 2023) and shows monotonically increasing mean one-period returns as ESG constraints are tightened.

---

## Repository contents

### Notebooks

| File | Model | Description | Paper exhibits |
|------|-------|-------------|----------------|
| `Model 1.ipynb` | Baseline (unconstrained) | Black-Litterman + MGARCH, no ESG constraint; closed-form Lagrangian weights | Exhibits 1–2 |
| `Model 2.ipynb` | ESG long-short | Adds ESG deficiency ceiling; closed-form two-multiplier Lagrangian; short positions allowed | Exhibits 3–6 |
| `Model 3.ipynb` | ESG long-only | Same ESG constraint; QP solved with `qpsolvers`/`quadprog`; weights bounded to [0.0001, 1] | Exhibits 7–9 |

### Data files

| File | Role |
|------|------|
| `Stock Prices.xlsx` | Daily adjusted closing prices (Bloomberg PX_LAST) for the 60-stock universe |
| `ESG Scores.xlsx` | S&P Global ESG Rank (RobecoSAM Total Sustainability Rank) for each stock; deficiency = 100 minus raw rank |
| `Market Caps.xlsx` | Daily market capitalisations used to construct the Black-Litterman equilibrium prior |

---

## Data

- **Universe:** 60 US large-cap stocks
- **Date range:** July 2010 to May 2023
- **ESG source:** S&P Global ESG Rank (RobecoSAM Total Sustainability Rank); higher rank = higher ESG quality; the notebooks work with *deficiency* = 100 minus the raw rank so that lower values are better
- **Price and market-cap source:** Bloomberg Terminal

---

## Reproducing the results

**Requirements**

```bash
pip install -r requirements.txt
```

**Execution**

Open each notebook in Jupyter and run all cells top to bottom. Each notebook is self-contained. Runtime is dominated by the MGARCH fitting step (16 fits per notebook on a 60-asset covariance model).

**Headline results**

| Notebook | Mean one-period return |
|----------|----------------------|
| `Model 1.ipynb` (baseline) | 0.0024 |
| `Model 2.ipynb` (ESG long-short) | 0.0036 |
| `Model 3.ipynb` (ESG long-only) | 0.0066 |

Rolling 64-trading-day retraining window; 16 out-of-sample trades.

---

## Citation

```bibtex
@article{hernandez_villamor_2026_esg,
  author  = {Hernandez, Jorge and Villamor, Enrique},
  title   = {Optimizing {ESG} Portfolios},
  journal = {The Journal of Impact and {ESG} Investing},
  year    = {2026},
  season  = {Spring},
}
```

---

## Acknowledgements

The authors thank **Daniel Bilsker** for assistance with the Python implementation. This research was partially funded by the US Department of Defense under grant **W911NF-25-1-0138**. This is contribution **#2000** from the Institute of Environment at Florida International University (FIU).
