# Brute-Force Portfolio Construction

## Introduction
This repository contains a **brute-force portfolio construction and backtesting** framework. It selects an optimal subset of stocks from a given universe to maximize the portfolio’s Sharpe ratio under specific constraints. The results are benchmarked against the S&P 500 (using SPY), with key metrics like **CAGR, Volatility, Sharpe Ratio, and Max Drawdown**.

## What Problem Does This Solve?
1. **Portfolio Selection**: Systematically identifies which stocks from a defined universe could improve risk-adjusted returns.  
2. **Weight Optimization**: Uses a constrained optimization method to allocate capital to chosen stocks.  
3. **Automated Rebalancing & Tracking**: Periodically rebalances, tracks daily performance, and compares results to a market benchmark.

This approach answers:
- Which assets to include?
- How much to allocate to each position?
- How often to rebalance?

By exhaustively searching through combinations of potential stocks, it aims to find a historically optimal configuration.

---

## Project Overview

1. **Configuration & Parameters**  
   - **API Key**: Insert your [Alpha Vantage](https://www.alphavantage.co/) API key.  
   - **Tickers**: Define your universe of stocks in `universe_tickers`.  
   - **Benchmark**: Typically `SPY` for the S&P 500.  
   - **Lookback**: Rolling window size (default 252 days).  
   - **Rebalance Frequency**: Rebalance interval (default every 5 days).  
   - **Portfolio Size**: Number of stocks to select (default 8).  
   - **Max Weight**: Maximum position size per stock (30% in this example).  
   - **Risk-Free Rate**: Annual rate used for Sharpe calculation (converted to daily).

2. **Data Fetching**  
   - Leverages Alpha Vantage to retrieve daily adjusted prices for each ticker.  
   - Cleans the data (forward fill, drops rows with incomplete data).

3. **Core Functions**  
   - **`compute_sharpe`**: Calculates the daily Sharpe ratio.  
   - **`optimize_weights`**: Constrained optimization to maximize Sharpe ratio (long-only, sum of weights = 1, weights ≤ max weight).  
   - **`brute_force_best_portfolio`**: Tests every combination of the chosen `portfolio_size` within the universe to find the highest Sharpe combination.

4. **Backtesting Flow**  
   - **Initial Portfolio**: Determines a valid initial date, selects a portfolio using the most recent `lookback_period` of returns.  
   - **Main Loop**:  
     1. Moves day-by-day.  
     2. Rebalances on schedule (every N days), re-runs brute-force on the latest window.  
     3. Tracks daily returns and portfolio value.

5. **Performance Metrics**  
   - **Total Return, CAGR, Annual Volatility, Sharpe Ratio, and Max Drawdown**: Compares the final results to the benchmark (SPY).

6. **Visualization**
   - **Portfolio vs. Benchmark**: Assesses whether or not the strategy outperforms the market over time.
   - **Daily Allocation**: Reveals the internal composition of the portfolio across the entire backtest.
   - **Drawdown Comparison**: Demonstrates the risk characteristics, especially critical during market downturns.

---

