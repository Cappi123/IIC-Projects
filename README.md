# Investment Portfolio Analysis & Construction

Multi-asset portfolio optimization using Modern Portfolio Theory.  
NTU Investment Interactive Club — Invest Academy AY24/25

---

## Overview

This project constructs an optimal investment portfolio across 8 ETFs spanning US large-cap, global developed markets, emerging markets, commodities, tech, and small-cap equities. The analysis builds cumulatively — each section produces findings that the next section validates or extends.

**Assets**: VOO, URTH, EWM, EWJ, GLD, QQQ, IWM, MCHI  
**Data period**: January 2019 – December 2024  
**Risk-free rate**: 4% (US 10-Year Treasury)  
**Out-of-sample test**: January 2025 – present

## Repository Structure

```
InvestmentPortfolio/
├── Portfolio.ipynb                              # Main notebook (all analysis)
├── PortfolioReport.pdf                          # PDF report
├── Investment Portfolio Assignment 1.pptx       # Original assignment brief
├── README.md
└── out/                                         # Generated charts
    ├── risk_return_profile.png
    ├── correlation_matrix.png
    ├── beta_regression.png
    ├── efficient_frontier.png
    ├── portfolio_allocation.png
    ├── sharpe_vs_beta_tradeoff.png
    ├── portfolio_backtest.png
    └── oos_backtest.png
```

## Methodology

The notebook is structured so that each section's output feeds directly into the next:

| Section | Purpose | Key Output |
|---------|---------|------------|
| **§1** Return & Risk | Annualized return, volatility, Sharpe for each ETF | Best standalone asset (QQQ) as benchmark Sharpe to beat |
| **§2** Correlation | Pairwise correlation matrix across all 8 ETFs | Identifies GLD as the best diversifier for QQQ (near-zero correlation) |
| **§3** Beta / CAPM | OLS regression of excess returns vs market (VOO) | Beta, alpha, R² for each asset; GLD has β ≈ 0 with positive alpha |
| **§4** Optimization | Markowitz mean-variance optimization (15,000 MC + SLSQP) | Max Sharpe and Min Variance portfolios; validates §1–3 predictions |
| **§5** Beta Constraint | Sweep beta caps to map the Sharpe–beta tradeoff | Shows beta constraints are free above the unconstrained optimum |
| **§6** In-Sample Backtest | Replay 2019–2024 with frozen §4 weights | Optimized portfolio beats equal-weight and VOO on Sharpe and drawdown |
| **§7** Out-of-Sample Backtest | Apply same frozen weights to 2025–2026 (unseen data) | Tests whether §1–3 patterns persist beyond the training window |

## Key Findings

- The optimizer independently selected **QQQ + GLD** as the dominant pair — confirming the qualitative prediction from §1 (QQQ has highest return), §2 (near-zero QQQ–GLD correlation), and §3 (GLD has β ≈ 0 with positive alpha).
- The optimized portfolio's **Sharpe ratio exceeds the best individual asset** (QQQ), justifying multi-asset construction.
- **Beta constraints are effectively free**: the Sharpe–beta tradeoff curve is flat above the unconstrained portfolio beta, because GLD's near-zero beta already provides natural crash protection.
- The out-of-sample backtest (§7) applies frozen weights to unseen 2025–2026 data to test whether results generalize.

## Techniques Used

- Annualized return, volatility, and Sharpe ratio computation
- Pearson correlation matrix and heatmap visualization
- Beta estimation via OLS regression (CAPM: `R_asset − Rf = α + β(R_market − Rf) + ε`)
- Markowitz mean-variance optimization with long-only constraints
- Monte Carlo simulation (15,000 random portfolios) for efficient frontier visualization
- Capital Market Line derivation
- Beta-constrained portfolio optimization
- In-sample and out-of-sample backtesting with rolling beta tracking

## Requirements

```
numpy
pandas
yfinance
matplotlib
statsmodels
scipy
```

Install with:

```bash
pip install numpy pandas yfinance matplotlib statsmodels scipy
```
