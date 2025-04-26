# 🔢 AutoTrader - Application Flow

This document outlines the **logical flow** and **user-driven interactions** inside the **AutoTrader Crypto Trading Bot**.

---

## 🌐 System Startup

When the application first loads:

- **Initialize Ably Realtime Client**:
  - Connects to Ably for streaming price updates, actions, and position changes.

- **Fetch Historical Crypto Data**:
  - Pulls 7 days of Bitcoin price history from CoinGecko.
  - Used to compute trading indicators and thresholds.

- **Generate Initial Trading Rules**:
  - Randomly creates an initial rule set (5 rules) based on trading variables, thresholds, and z-scores.

---

## 📈 User Clicks "Start Trading"

When the user initiates live trading:

- Every **5 seconds**:
  - **Fetch Live Price**: Pull current Bitcoin price.
  - **Update Trading State**: Update variables like price, fair price.
  - **Evaluate All Rules**:
    - For each rule, check if the condition is satisfied.
    - If a rule is triggered, execute **BUY** or **SELL** action accordingly.
  - **Update Cash, Position, and Wealth**:
    - Adjust the portfolio according to the executed trades.
  - **Publish Updates**:
    - Broadcast new price, action, and position through Ably channels for real-time streaming.

---

## 🔄 User Clicks "Generate & Test"

When the user requests to optimize strategies:

- **Fetch Latest Historical Data** for crypto prices.
- **Fetch Previous Rule Sets** from the database.
- **Generate New Candidate Rule Sets**:
  - Create multiple random rule sets.
- **Backtest All Rule Sets**:
  - Simulate trading using historical data.
  - Calculate performance metrics: portfolio value, Sharpe ratio, maximum drawdown, cross-entropy.
- **Select Best Rule Set**:
  - Choose the rule set with the best overall performance.
- **Save and Update Rules**:
  - Store the new best rule set in the database.
  - Update the active rule set in the app.

---

## 📲 Receiving Live Updates (Ably Streams)

The app stays synced with real-time changes:

- **Price-Stream Channel**:
  - Logs price updates from Ably.

- **Action-Stream Channel**:
  - Logs when trade actions are executed.

- **Pos-Stream Channel**:
  - Logs updates to trading positions (number of units held).

---

## 📆 Quick Summary

| Flow Stage          | Main Actions |
|:--------------------|:-------------|
| **Startup**          | Setup Ably, fetch history, generate rules |
| **Start Trading**    | Fetch live prices, trade based on rules, update wealth |
| **Generate & Test**  | Evolve and optimize trading rules |
| **Realtime Updates** | Stream updates from Ably channels |

---

> This flow ensures that AutoTrader continuously adapts, learns, and simulates intelligent crypto trading in real-time with dynamic rule generation and live market execution.

---

💡 **Tip**: This structure also supports future extension into multi-asset trading, broker API integration, and reinforcement learning for smarter rule evolution!

