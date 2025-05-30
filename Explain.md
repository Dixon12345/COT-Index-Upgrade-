# 🧠 Indicator Logic: COT Index + Linear Regression Forecast

*A script by Dixon Chai*

This document provides an in-depth explanation of the logic, structure, and purpose behind the "COT Index + Linear Regression Forecast" indicator published privately on TradingView.

---

## 📌 Concept Overview

This indicator combines **Commitment of Traders (COT)** data with a **Linear Regression Forecast** to analyze sentiment trends from the "smart money" (Commercial traders).

The main goal is to identify potential market turning points and trend continuations by projecting forward sentiment based on historical Commercial positioning.

---

## 🔍 Understanding COT Data

COT data is published weekly by the CFTC and splits traders into:

* **Commercials** – Usually large institutions or producers (considered smart money)
* **Non-Commercials** – Speculators like hedge funds
* **Retail (Non-Reportables)** – Small traders, typically less informed

By tracking net positioning of these groups, we can estimate market sentiment.

---

## ⚙️ Core Components

### 1. **Net Position Calculation**

```pinescript
netCom = comLong - comShort
```

This is the net long position of Commercials. A high net long = bullish sentiment.

---

### 2. **Commercial Index (CI)**

```pinescript
CI = 100 * (net - lowestLow) / (highestHigh - lowestLow)
```

This formula scales net positions into a 0-100 range. Higher values = stronger bullish sentiment.

The script calculates CI over:

* **3 Years (156 weeks)**
* **12 Months (52 weeks)**
* **6 Months (26 weeks)** → Used for forecasting

---

### 3. **Linear Regression Forecast**

```pinescript
regLine = ta.linreg(index_6m, length, 0)
forecast = ta.linreg(index_6m, length, -offset)
```

We use a regression model to:

* Estimate the trend of the 6-month Commercial Index
* Project its **future value** with an offset

This helps us forecast how Commercial sentiment may evolve.

---

### 4. **Forecast Bias Detection**

```pinescript
forecast > regLine * (1 + threshold / 100)
```

* **Bullish** if the forecast rises above current regression line by threshold
* **Bearish** if below by the threshold
* **Neutral** if within range

This gives a clear signal about where sentiment is heading.

---

### 5. **Visual Features**

* Colored forecast line:
  🟢 Green = Bullish
  🔴 Red = Bearish
  ⚪ Gray = Neutral

* Optional Bias Table:
  Displays current forecast bias (Bullish/Bearish/Neutral)

* Background highlights:
  Forecast zone is colored for better visual clarity

---

## ✅ How to Use This Indicator

* Add to a **weekly chart** for futures or major indices
* Watch for **forecast changes**:

  * Green = time to prepare for long positions
  * Red = possible short opportunities or trend weakening
* Confirm with other technical tools or price structure

---

## 📈 Example Use Cases

* Forecast turning points in commodities like Gold or Oil
* Filter macro sentiment before entering trades
* Identify divergence between forecast bias and price movement

---

## ⚠️ Disclaimer

This tool is designed for **educational purposes** only. It does not constitute financial advice. Always conduct independent analysis before trading.

---


