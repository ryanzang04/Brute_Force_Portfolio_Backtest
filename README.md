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

## Performance Analysis

1. **Overall Return and Growth**
- Date Range: May 23, 2013 – February 28, 2025 (about 11.77 years).
- Final Portfolio Value: 14.85, which corresponds to a total return of +1,384.58%.
- Benchmark (SPY) Value: 4.43, a total return of +343.02% over the same period.
![daily_r](https://github.com/user-attachments/assets/b3445b64-58c0-4b08-9e60-fd805faa5e81)

2. **Volatility and Sharpe Ratio**
- Annualized Volatility: 20.77% (Portfolio) vs. 16.83% (SPY).
- Sharpe Ratio: 1.21 (Portfolio) vs. 0.84 (SPY).
Volatility (standard deviation of returns) is higher in the portfolio—meaning that day-to‐day/month‐to‐month fluctuations are somewhat bigger than the market average.
But the risk‐adjusted performance, captured by the Sharpe ratio, is still higher (1.21 vs. 0.84).

3. **Drawdowns (Peak‐to‐Trough Losses)**
- Max Drawdown: –25.84% (Portfolio) vs. –33.70% (SPY).
![daily_d](https://github.com/user-attachments/assets/7b29ad91-6eb8-40eb-9d88-8898b76313f9)


4. **Changing Allocations Over Time**
- Owing to the selection of only 8 out of 14 possible tickers, the portfolio can become heavily concentrated in strong performers. This cuts both ways—return potential is higher if correct, but sector or individual‐name risk could increase. However, the 30% weight cap helps limit risk from any single name.
![daily_a](https://github.com/user-attachments/assets/ff8c4fb9-a550-4f06-a43b-02c070132e59)



