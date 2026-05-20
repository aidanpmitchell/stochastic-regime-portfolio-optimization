# Stochastic Regime Portfolio Optimization

This project studies whether market regimes and macroeconomic regime forecasts can improve portfolio allocation across a multi-asset ETF universe.

The workflow starts with standard mean-variance optimization, adds Hidden Markov Model market-regime detection, and then tests whether macroeconomic data from FRED can forecast next-month regimes well enough to improve a forward-looking portfolio strategy.

**Main finding:** macro regime probabilities create economically sensible defensive shifts, especially toward GLD and TLT when predicted Crisis risk rises. However, the forward-looking macro strategy does not outperform the reactive HMM regime strategy in the final backtest.

![Walk-forward cumulative performance](figures/macro_forward_cumulative_performance.png)

## Project Structure

| Stage | Notebook | Purpose |
|---|---|---|
| 1 | [01_market_data.ipynb](notebooks/01_market_data.ipynb) | Load and prepare asset prices and returns |
| 2 | [02_covariance_estimation.ipynb](notebooks/02_covariance_estimation.ipynb) | Estimate covariance matrices |
| 3 | [03_portfolio_optimization.ipynb](notebooks/03_portfolio_optimization.ipynb) | Build baseline mean-variance portfolios |
| 4 | [04_market_regime_detection.ipynb](notebooks/04_market_regime_detection.ipynb) | Detect Bull, Neutral, and Crisis regimes with an HMM |
| 5 | [05_regime_specific_portfolio.ipynb](notebooks/05_regime_specific_portfolio.ipynb) | Estimate regime-specific return and covariance inputs |
| 6 | [06_basic_regime_switching_backtest.ipynb](notebooks/06_basic_regime_switching_backtest.ipynb) | Backtest a reactive HMM regime-switching strategy |
| 7 | [07_macro_regime_prediction.ipynb](notebooks/07_macro_regime_prediction.ipynb) | Use FRED macro data to predict next-month HMM regimes |
| 8 | [08_macro_forward_regime_backtest.ipynb](notebooks/08_macro_forward_regime_backtest.ipynb) | Test a forward-looking macro-regime portfolio |

For the full written interpretation, see [reports/final_results_report.md](reports/final_results_report.md).

## Data

The asset universe is:

| Ticker | Role |
|---|---|
| SPY | US equities |
| EFA | Developed international equities |
| GLD | Gold |
| TLT | Long-term US Treasuries |
| VNQ | US real estate |

Notebook 7 adds FRED macro indicators covering labor markets, inflation, interest rates, industrial activity, credit spreads, housing, and consumer sentiment.

## Methodology

1. Compute daily asset returns and baseline mean-variance portfolios.
2. Fit an HMM to market features to label market regimes.
3. Estimate regime-specific expected returns and covariance matrices.
4. Backtest a reactive strategy that allocates based on the currently observed HMM regime.
5. Train macro classifiers to predict the next month's HMM regime.
6. Use predicted regime probabilities to blend regime-specific moments.
7. Backtest the forward-looking macro strategy against equal-weight, static mean-variance, and reactive HMM benchmarks.

## Macro Regime Prediction

Notebook 7 selected `Random_Forest` as the best probability model by log loss. Persistence had higher hard-label accuracy, but its one-hot probabilities were too overconfident for portfolio construction.

![Macro model log loss](figures/macro_regime_model_log_loss.png)

| Model | Accuracy | Log Loss | Brier Score |
|---|---:|---:|---:|
| Random_Forest | 0.718 | 0.709 | 0.432 |
| Logistic_Regression | 0.595 | 1.146 | 0.562 |
| Hist_Gradient_Boosting | 0.679 | 1.201 | 0.515 |
| Gaussian_NB | 0.649 | 2.203 | 0.608 |
| Persistence | 0.794 | 3.797 | 0.412 |

The most important macro features were payroll growth, inflation, consumer sentiment, credit spreads, sentiment momentum, and the term spread.

![Macro feature importance](figures/macro_feature_importance.png)

## Final Backtest Results

Notebook 8 compares four monthly-rebalanced strategies from March 2015 through February 2026:

| Strategy | Annual Return | Annual Volatility | Sharpe Ratio | Max Drawdown |
|---|---:|---:|---:|---:|
| Reactive_HMM | 9.75% | 10.61% | 0.919 | -29.66% |
| Static_MV | 8.44% | 10.47% | 0.806 | -26.66% |
| Macro_Forward | 8.49% | 10.54% | 0.805 | -29.96% |
| Equal_Weight | 8.42% | 10.89% | 0.773 | -23.98% |

![Performance summary](figures/macro_forward_performance_summary.png)

`Macro_Forward` is competitive with `Static_MV` and slightly better than `Equal_Weight`, but `Reactive_HMM` remains the strongest performer by return and Sharpe ratio.

![Drawdowns](figures/macro_forward_drawdowns.png)

## What The Macro Strategy Did

The macro strategy behaved as intended: when predicted Crisis risk rose, it shifted toward GLD and TLT and away from SPY.

![Predicted regime probabilities](figures/macro_regime_predicted_probabilities.png)

![Average portfolio weights](figures/average_portfolio_weights.png)

![Macro weights by crisis probability](figures/macro_weights_by_crisis_probability.png)

This is economically sensible, but it did not consistently improve performance. Several high-Crisis-probability months were still realized Bull months, so the more defensive macro allocation sometimes gave up equity upside.

## Conclusion

The project shows that macroeconomic data can produce plausible forward-looking regime probabilities and that those probabilities can be translated into meaningful portfolio shifts. The current macro strategy is not a clear performance improvement over the reactive HMM benchmark, but it is useful as a risk signal and a foundation for future refinements.

Promising next steps:

- Calibrate the Random Forest probabilities.
- Blend macro forecasts with current-regime persistence.
- Add transaction costs and turnover penalties.
- Test macro probabilities as risk-scaling signals instead of full expected-return and covariance inputs.
- Use real-time ALFRED vintages to better control macro publication lag risk.

## Reproducing

Install dependencies and run notebooks in order:

```bash
pip install -r requirements.txt
```

The final outputs are saved in `results/`, and report figures are saved in `figures/`.
