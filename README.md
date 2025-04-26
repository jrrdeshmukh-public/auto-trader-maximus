# 🔢 AutoTrader - Application Flow

This document outlines the **logical flow** and **user-driven interactions** inside the **AutoTrader Trading Bot**.

---

## 🌐 System Startup

When the application first loads:

- **Initialize Ably Realtime Client**: 
  - Connects to Ably for streaming price updates, actions, and position changes.

- **Fetch Historical Market Data**:
  - Pulls 7 days of historical price data from external sources.
  - Used to compute trading indicators and thresholds.

- **Generate Initial Trading Rules**:
  - Randomly creates an initial rule set (5 rules) based on trading variables, thresholds, and z-scores.

---

## 📈 User Clicks "Start Trading"

When the user initiates live trading:

- Every **5 seconds**:
  - **Fetch Live Price**: Pull current asset price.
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

- **Fetch Latest Historical Data** for asset prices.
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

# 👁️ Case-Based Reasoning (CBR) in AutoTrader

AutoTrader employs a **case-based reasoning** (CBR) framework to evolve its trading strategies over time. 
Rather than predefining a fixed model, the system **generates rules**, **tests them**, and **learns by comparing outcomes**, similar to how a human trader would analyze historical trades and adjust.

### How CBR Works in This System:

- **Historical Data as Cases**:
  - Uses 7 days of historical price data as "past experiences."

- **Dynamic Rule Generation**:
  - Randomly creates hypotheses about trading conditions using indicators, thresholds, and z-scores.

- **Simulate and Evaluate**:
  - Applies each rule set over historical cases to simulate trades.

- **Score and Select**:
  - Evaluates rules based on portfolio performance, Sharpe ratio, drawdown, and cross-entropy.

- **Learn and Adapt**:
  - Selects the best rule set and updates the live trading logic.

---

# 📈 Generate-and-Test Cycle

The **Generate and Test** process is AutoTrader's core evolutionary engine:

| Phase         | What Happens |
|:--------------|:-------------|
| **Generate**  | Randomly create new candidate rules. |
| **Simulate**  | Apply rule sets to historical data and simulate trades. |
| **Score**     | Calculate performance metrics. |
| **Select**    | Choose the best-performing rule set. |
| **Save**      | Store the best rule set and apply it in live trading. |

✅ This cycle repeats **whenever "Generate & Test" is clicked**, helping AutoTrader continuously adapt and optimize.

---

> This flow ensures that AutoTrader continuously adapts, learns, and simulates intelligent trading in real-time with dynamic rule generation and live market execution.

---

💡 **Tip**: This structure also supports future extension into multi-asset trading, broker API integration, and reinforcement learning for smarter rule evolution!

