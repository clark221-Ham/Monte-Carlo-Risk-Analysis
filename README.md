# Monte-Carlo-Risk-Analysis
Monte Carlo Risk Envelope for Rate-Sensitive Assets
Overview

This project implements a Monte Carlo–based risk framework for evaluating the distribution of future price outcomes for financial assets over a fixed horizon.
The model is designed for Capital Markets and risk analysis use cases, with a focus on rate-sensitive securities such as Treasuries and MBS ETFs.

Rather than producing point forecasts, the model constructs probabilistic price envelopes that allow users to assess:

Expected price dispersion

Downside and upside risk bands

Frequency and magnitude of realized price breaches

Volatility-driven risk versus price-level risk

All evaluations are performed strictly out-of-sample to avoid data leakage.

Methodology
1. Data Collection

Historical price data is sourced from Yahoo Finance via yfinance.

The user specifies a training window and a forward evaluation window

Adjusted Close prices are used when available (otherwise Close)

Data is cleaned to remove missing values and ensure continuity

2. Return Estimation

Log returns are computed over the training window:

𝑟
𝑡
=
log
⁡
(
𝑃
𝑡
)
−
log
⁡
(
𝑃
𝑡
−
1
)
r
t
	​

=log(P
t
	​

)−log(P
t−1
	​

)

From these returns, the model estimates:

Mean daily return (μ)

Daily volatility (σ)

These parameters form the stochastic inputs to the Monte Carlo simulation.

3. Monte Carlo Simulation

Using a geometric Brownian motion framework, the model generates simulated price paths:

𝑃
𝑡
+
1
=
𝑃
𝑡
⋅
𝑒
𝜇
+
𝜎
𝜖
𝑡
P
t+1
	​

=P
t
	​

⋅e
μ+σϵ
t
	​


Where:

𝜖
𝑡
∼
𝑁
(
0
,
1
)
ϵ
t
	​

∼N(0,1)

The number of simulations and horizon length are user-defined

The output is a matrix of simulated price paths representing a forward risk distribution, not a forecast.

4. Risk Envelope Construction

From the simulated paths, the model computes:

Median price path

Lower and upper confidence bounds (default: 5th–95th percentile)

These bounds define the Monte Carlo risk envelope.

5. Out-of-Sample Evaluation

The realized forward prices are compared against the simulated envelope to produce diagnostics:

Price RMSE vs median path

Directional hit rate

Breach rate (percentage of days outside the confidence band)

Days outside risk envelope

Average band width

Annualized volatility

Maximum downside breach

This allows users to evaluate how well the estimated risk distribution captures realized outcomes.

Output Example
Monte Carlo Risk Diagnostics
======================================
Asset:               MBB
Evaluation Window:   2020-01-01 → 2021-01-01
Confidence Band:     5–95%
Trading Days:        253
--------------------------------------
Price RMSE:          $1.80
Directional Hit:     79.8%
Breach Rate:         20.2%
Days Outside Band:   51
--------------------------------------
Avg Band Width:      $4.91
Annualized Vol:      2.4%
Max Downside Miss:   $0.77
======================================

Visualization

The model produces a single out-of-sample chart showing:

Realized price path

Median Monte Carlo projection

Shaded confidence band

This visual representation allows for quick interpretation of:

Risk asymmetry

Volatility regime shifts

Structural deviations from historical dynamics

How to Use
1. Install Dependencies
pip install numpy pandas yfinance matplotlib scikit-learn

2. Configure Parameters

In main.py, adjust:

ticker = "MBB"
train_start = "2015-01-01"
train_end   = "2020-01-01"
test_end    = "2021-01-01"
simulations = 300


The model works with any equity or ETF ticker, but is designed for rate-sensitive assets.

3. Run the Model
python main.py


This will:

Download data

Estimate return parameters

Run Monte Carlo simulations

Print risk diagnostics

Generate the risk envelope chart

Intended Use Cases

Capital Markets risk analysis

Scenario-based asset evaluation

Distribution-based risk framing

Understanding volatility vs price-level risk

Supplementary decision support (not trading signals)

Design Philosophy

Risk > prediction

Out-of-sample validation

Interpretability over complexity

Desk-relevant metrics

No optimization or curve-fitting

This framework is intentionally conservative and avoids over-engineering.

Limitations

Assumes normally distributed log returns

Does not model regime switching or jumps

Does not incorporate macro or rate inputs directly

Not intended for alpha generation

Author Notes

This project was developed to demonstrate how Monte Carlo techniques can be used to support Capital Markets risk assessment, particularly for rate-sensitive instruments, by translating historical volatility into interpretable forward risk distributions.

Final Assessment (Hiring Manager Lens)

As a standalone project: Strong
Combined with a yield-curve or rates-sensitivity project: Very strong

This project signals:

Quantitative literacy

Risk-first thinking

CM-relevant intuition

Professional communication
