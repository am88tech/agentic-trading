# Agentic AI — Backtest & Signal Notebook
## Overview

This project performs end-to-end algorithmic backtesting and signal generation for a single stock (e.g., **ADANIPOWER.NS**) using Python.
It automates data fetching, indicator computation, strategy evaluation, and daily signal generation through an agentic, rule-based framework.

---

## What the Code Does

* Fetches historical OHLCV data using **yfinance**.
* Calculates technical indicators across four key domains:

  * **Trend** – SMA, EMA, MACD
  * **Momentum** – RSI, ROC, Stochastic, MACD Histogram
  * **Volume** – OBV, CMF, VROC, MFI
  * **Volatility** – ATR, Bollinger Bands, Historical Volatility
* Generates combinations of indicator-based trading rules and backtests each.
* Uses a **fixed-allocation backtesting model** that limits per-trade risk and avoids compounding.
* Incorporates brokerage fees, GST, and slippage assumptions.
* Ranks indicator combinations using a composite score (`Sharpe - 2*|Max Drawdown|`).
* Outputs a final ranked list of best-performing combinations and their performance metrics.
* Produces a **“Today Suggestion”** — a daily recommendation file that shows whether to buy today, how many shares to buy, and expected risk based on ATR.

---

## Key Outputs

After running the notebook, three primary output files are generated:

1. **`trades_<TICKER>_<TIMESTAMP>.csv`**

   * Contains each simulated trade with entry/exit dates, prices, position size, and profit/loss.

2. **`backtest_summary_<TICKER>_<TIMESTAMP>.txt`**

   * Human-readable summary of the backtest showing Sharpe Ratio, CAGR, Drawdown, Win Rate, and profitability.

3. **`today_suggestion_<TICKER>_<YYYYMMDD>.txt`**

   * Current-day recommendation file showing whether a buy signal is active, suggested quantity, estimated position size, and expected loss (with fees included).

---

## Practical Interpretation

* **Signal = 1:** The strategy recommends **buying today**.
* **Signal = 0:** No active signal — **do not buy**.
* The suggestion file gives share quantity, stop-loss (based on ATR), and maximum loss estimate.
* Always confirm signals with real-time market context before trading.

---

## Configuration Parameters

| Parameter             | Description                          | Example           |
| --------------------- | ------------------------------------ | ----------------- |
| `TICKER`              | Stock symbol                         | `"ADANIPOWER.NS"` |
| `INIT_CAPITAL`        | Starting capital (INR)               | `100000.0`        |
| `RISK_PER_TRADE`      | Fraction of capital risked per trade | `0.01`            |
| `BROKERAGE_PER_ORDER` | Flat brokerage per order             | `₹20`             |
| `GST_RATE`            | GST applied on brokerage             | `0.18`            |
| `SLIPPAGE_PCT`        | Slippage on entry/exit               | `0.0005` (0.05%)  |
| `ANNUAL_FACTOR`       | Trading days used for annualization  | `252`             |

---

## How to Run

1. Open this notebook in **Google Colab** or **Jupyter Notebook**.
2. Ensure dependencies are installed:

   ```bash
   pip install yfinance pandas matplotlib
   ```
3. Run all cells sequentially.
4. Review printed results and charts.
5. Download output files from the working directory (Colab: use `files.download()`).

---

## Core Functions

* **`backtest_rules()`** — Runs the backtest simulation with fixed allocation and realistic trade execution.
* **`performance_stats_from_equity()`** — Calculates Sharpe Ratio, CAGR, and Max Drawdown.
* **`trade_aggregates()`** — Aggregates and summarizes trade results.
* **`signal_from_indicators()`** — Generates buy signals for selected indicator combinations.
* **`today_suggestion`** — Evaluates the last trading day and creates a plain-text recommendation file.

---

## Limitations

* Long-only logic (no short selling).
* Simplified fee structure (brokerage + GST only).
* No train/test split — strategies are evaluated on full history.
* Execution is simulated at open price ± slippage.
* Backtest does not guarantee future performance.

---

## Suggested Next Steps

* Add **walk-forward validation** or **train/test split** to check robustness.
* Include full Indian fee model (STT, exchange, and stamp duty).
* Extend to multiple tickers with parallel backtests.
* Automate **email alerts** for the daily suggestion file.
* Integrate into an **Agentic AI orchestration** workflow (e.g., LangChain or LangGraph) for multi-stock automation.

---

## Example Interpretation

If the output shows:

```
"signal": 1,
"shares_suggested": 120,
"est_max_loss_net_incl_fees": 850.0
```

It means the model suggests buying approximately **120 shares** today, and if the stop-loss (based on ATR) hits, the maximum expected loss including brokerage + GST is around **₹850**.

---

## Summary

This notebook serves as a **research and evaluation framework** for rule-based trading strategy optimization.
It bridges financial analytics with agentic AI principles — automating indicator selection, rule testing, and daily signal generation — all without manual tweaking.

---