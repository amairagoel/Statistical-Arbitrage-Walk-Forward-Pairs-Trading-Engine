# Statistical-Arbitrage-Walk-Forward-Pairs-Trading-Engine
An algorithmic pairs trading research engine featuring walk-forward optimization, algorithmic cointegration screening, and dynamic rolling hedge ratios to adapt to market regime changes.
Table of Contents

Overview

Mathematical Rationale

Key Features

Installation & Usage

Backtest Performance

Future Improvements

Disclaimer

Overview

This project implements a complete quantitative research pipeline for statistical arbitrage. It moves beyond basic static-beta models by introducing a robust Walk-Forward Optimizer that algorithmically screens a basket of equities, identifies the most cointegrated pair for a given market regime, and dynamically updates the hedge ratio using rolling covariance.

By utilizing a rolling-window approach, the strategy actively defends against strategy decay and structural market breaks (such as corporate mergers or macroeconomic shifts).

Mathematical Rationale

The core logic relies on identifying non-stationary time series (stock prices) that, when linearly combined, create a stationary process (the spread).

Log-Price Transformation: Calculates spreads using the natural logarithm of prices ($\ln(P)$) to stabilize variance and prevent absolute-price scaling issues.

Dynamic OLS Beta ($\beta$): Instead of static $Spread = Y - X$, we use rolling covariance/variance to calculate the hedge ratio: $\beta = \frac{Cov(Y, X)}{Var(X)}$. Therefore, the spread is defined as $Spread = \ln(Y) - (\beta \times \ln(X))$.

Z-Score Normalization: The spread is normalized using a shifted 30-day rolling mean and standard deviation to prevent lookahead bias.

Key Features

Algorithmic Pair Selection: Uses the Augmented Dickey-Fuller (ADF) test to automatically screen combinations of equities and select the pair with the strongest mean-reverting properties (lowest p-value).

Walk-Forward Optimization: Prevents lookahead bias and overfitting by rotating pairs annually. (e.g., Train on 2019-2020 $\rightarrow$ Trade on 2021).

Dynamic Hedge Ratios: Maintains true market neutrality as asset betas drift over time.

Vectorized Backtesting: Replaces slow Pandas iterative logic with high-performance NumPy arrays for path-dependent signal generation and stop-loss execution.

Installation & Usage

Prerequisites

Ensure you have Python 3.8+ installed. You will need the following libraries:

pip install pandas numpy statsmodels yfinance matplotlib


Running the Backtest

Simply clone the repository and run the main Python script. The script will automatically download the required historical data via the yfinance API, screen the basket, run the walk-forward simulation, and plot the equity curve.

git clone https://github.com/YourUsername/Walk-Forward-Pairs-Trading.git
cd Walk-Forward-Pairs-Trading
python pairs_trading.py


Backtest Performance

Tested Out-of-Sample on the top 6 Indian Banking Equities (2021-2023)

Metric

Result

Annualised Return

27.25%

Annualised Volatility

24.54%

Sharpe Ratio

1.11

Regime Stability

High (Algorithm successfully rotated out of decaying pairs, such as HDFC/KOTAK in 2022)

(Note: These figures represent gross returns. In a production environment, a friction penalty of ~5-10 bps per trade should be applied to simulate bid-ask spread and exchange fees).

Future Improvements

Execution Friction: Implement order execution penalties to simulate realistic slippage.

Kalman Filters: Upgrade the rolling covariance hedge ratio to a Kalman Filter for faster, noise-resistant beta adaptation.

Ornstein-Uhlenbeck Half-Life Targeting: Dynamically adjust the Z-score lookback windows based on the calculated OU half-life of the active pair.

Disclaimer

This project is for educational and research purposes only. It does not constitute financial advice. Real-world trading involves significant execution risk, liquidity constraints, and transaction costs not fully modeled in this backtest.
