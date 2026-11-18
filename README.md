# Blog Post 9 — Predicting Stock Returns with Gradient Boosting

This repository contains the code, data, and results for Blog Post 9:
**"Predicting Stock Returns with Gradient Boosting: An Honest Out-of-Sample Attempt"**
(Computational Methods for Economists).

The post revisits the in-class stock-prediction experiment:

1. **The experiment** — gradient boosting (`gbm`) on a monthly panel of 30 large
   U.S. stocks to predict next-month returns, evaluated with a walk-forward
   (expanding-window) protocol over the last 60 months.
2. **An improvement attempt** — richer predictors (momentum at four horizons,
   realized volatility, distance from the 52-week high) and heavier
   regularization (slower learning rate, bagging, larger nodes).
3. **Honest evaluation** — out-of-sample comparison against both the original
   model and a naive benchmark (trailing 12-month average), plus an economic
   long–short backtest against buy-and-hold S&P 500.

## Repository structure

```
blog9-repo/
├── README.md                     # this file
├── code/
│   └── blog9_analysis.R          # standalone, reproducible analysis script
├── data/
│   └── prices.csv                # cached daily adjusted closes (Yahoo Finance)
├── posts/
│   └── blog9.Rmd                 # the blog post source (renders to HTML)
```

## How to reproduce

Requirements: R (>= 4.0) with the packages `dplyr`, `tidyr`, `zoo`, `ggplot2`,
`httr`, `jsonlite`, and `gbm`.

From the repository root, run:

```r
Rscript code/blog9_analysis.R
```

This recreates every number, figure, and table in `results/`. The daily price
snapshot in `data/prices.csv` ships with the repository, so no internet access
is needed; delete that file to re-download the latest data from Yahoo Finance
(results will shift slightly as new data arrive). All random draws are fixed by
`set.seed()`, so results are fully reproducible from the snapshot.

Optionally, render the post itself to HTML with:

```r
rmarkdown::render("posts/blog9.Rmd", output_format = "html_document")
```

## Key results

- Panel: 7,000 stock-month rows, 31 stocks, 2008–2026 (monthly).
- Out-of-sample window: 2021-10 to 2026-09 (60 months, 1,860 predictions).
- Predictive (OOS): R² — naive benchmark −0.126, original GBM −0.032,
  improved GBM −0.030; sign accuracy 51.2% / 53.6% / 53.3%.
- Economic: long–short Sharpe — original 0.06, improved 0.38, versus
  buy-and-hold S&P 500 0.82 (12.8% annualized).
- Conclusion: a more sophisticated model still fails to predict monthly returns
  out of sample, and its long–short edge stays far below passive indexing
  before transaction costs.
