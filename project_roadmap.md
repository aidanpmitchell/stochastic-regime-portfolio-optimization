# Project Roadmap
## Stochastic Regime-Switching Portfolio Optimization

## Project Goal

This project explores how **stochastic processes and portfolio optimization** can be combined to create a **dynamic asset allocation strategy** that adapts to changing market conditions.

Traditional portfolio optimization assumes that expected returns and covariance structures are constant over time. In reality, financial markets exhibit **different regimes**, such as bull markets, bear markets, and high-volatility periods.

This project will model market regimes using **stochastic processes (Markov chains)** and compute **regime-dependent optimal portfolios** using **mean-variance optimization**.

The goal is to evaluate whether a **regime-switching portfolio strategy** can outperform static portfolio allocation strategies.

---

# Mathematical Framework

## Portfolio Returns

Let

$$
w = (w_1, \dots, w_n)
$$

be portfolio weights and

$$
R = (R_1, \dots, R_n)
$$

be asset returns.

Portfolio return:

$$
R_p = w^T R
$$

Expected return:

$$
E[R_p] = w^T \mu
$$

where \( \mu \) is the vector of expected asset returns.

---

## Portfolio Risk

Portfolio variance:

$$
Var(R_p) = w^T \Sigma w
$$

where \( \Sigma \) is the covariance matrix of asset returns.

---

## Mean-Variance Optimization

The portfolio weights are chosen by solving:

$$
\max_w \; w^T \mu - \lambda w^T \Sigma w
$$

subject to:

$$
\sum_i w_i = 1
$$

$$
w_i \ge 0
$$

This produces the optimal allocation given estimated returns and covariance.

---

## Market Regime Model

Market regimes will be modeled as a **Markov chain**.

Let

$$
S_t \in \{1,2,3\}
$$

represent the market regime at time \(t\).

Example regimes:

1. Bull market  
2. Neutral market  
3. Bear market  

The regime transition probabilities are given by:

$$
P_{ij} = P(S_{t+1} = j \mid S_t = i)
$$

These probabilities form the **transition matrix**.

---

## Regime-Dependent Portfolio Optimization

For each regime \(k\) we estimate:

$$
\mu_k
$$

$$
\Sigma_k
$$

Then solve:

$$
\max_w \; w_k^T \mu_k - \lambda w_k^T \Sigma_k w_k
$$

This produces regime-specific portfolios:

- \( w_{bull} \)
- \( w_{neutral} \)
- \( w_{bear} \)

---

# Implementation Plan

## Phase 1 — Data Collection

**Goal:** obtain historical market data.

Tasks:

- download ETF price data
- compute daily log returns
- visualize return distributions

Assets to include:

- **SPY** — US equities  
- **EFA** — international equities  
- **TLT** — long-term bonds  
- **GLD** — gold  
- **VNQ** — real estate  

Output:

Clean dataset of asset returns.

---

## Phase 2 — Covariance Estimation

**Goal:** estimate asset return covariance.

Tasks:

- compute sample covariance matrix
- visualize covariance structure
- examine correlations between assets
- compare rolling covariance estimates

Concepts used:

- covariance matrix  
- correlation matrix  
- matrix algebra  

---

## Phase 3 — Portfolio Optimization

**Goal:** compute optimal portfolios using mean-variance optimization.

Tasks:

- implement optimization problem
- compute optimal weights
- visualize efficient frontier
- compare portfolios under different risk levels

Concepts used:

- quadratic optimization  
- matrix operations  
- convex optimization  

---

## Phase 4 — Market Regime Detection

**Goal:** classify historical periods into market regimes.

Possible approaches:

- volatility regimes
- return-based regimes
- clustering methods

Output:

Dataset with regime label for each time period.

---

## Phase 5 — Markov Regime Model

**Goal:** model transitions between regimes.

Tasks:

- compute regime transition counts
- estimate transition matrix
- analyze regime persistence
- compute stationary distribution

Concepts used:

- Markov chains  
- transition matrices  
- stochastic processes  

---

## Phase 6 — Regime-Dependent Portfolio Optimization

**Goal:** compute optimal portfolios for each regime.

Tasks:

- estimate \( \mu_k \) and \( \Sigma_k \) for each regime
- solve optimization problem per regime
- analyze how allocations differ across regimes

---

## Phase 7 — Backtesting Strategy

**Goal:** simulate a regime-switching portfolio strategy.

Algorithm:

For each period:

1. detect current regime  
2. use corresponding optimal portfolio  
3. rebalance portfolio  
4. compute portfolio return  

Strategies to compare:

- equal-weight portfolio  
- static mean-variance portfolio  
- regime-switching portfolio  

---

## Phase 8 — Performance Evaluation

Metrics:

- cumulative returns
- Sharpe ratio
- volatility
- maximum drawdown

Visualizations:

- cumulative performance curves
- drawdown plots
- regime timeline

---

# Final Deliverables

## 1. GitHub Repository

Contains:

- data pipeline
- analysis notebooks
- reusable Python modules
- figures and results

---

## 2. Quarto Project Website

Includes:

- motivation
- mathematical framework
- model implementation
- portfolio optimization results
- strategy evaluation

---

## 3. Research-Style Documentation

The final project will present:

- mathematical explanations
- implementation details
- empirical results
- discussion of limitations and extensions

---

# Future Extensions

Possible improvements:

- hidden Markov models
- shrinkage covariance estimators
- transaction cost modeling
- machine learning regime detection
