![image](https://github.com/user-attachments/assets/680def7a-2fcf-4f16-9818-c1afc8f28a85)



# Serenity Index Strategy for ProRealTime

This repository provides a **Serenity Index Strategy** implementation for ProRealTime, a popular trading platform. The Serenity Index combines volatility (Average True Range), momentum (Relative Strength Index), and trend-following elements (Moving Average) into a single indicator to aid traders in identifying potential buy and sell signals.

## Table of Contents
- [Overview](#overview)
- [Parameters](#parameters)
- [Calculation](#calculation)
- [Strategy Logic](#strategy-logic)
- [Installation](#installation)
- [Contributing](#contributing)


## Overview

The **Serenity Index Strategy** helps traders assess market conditions by integrating:
- **Volatility** through the **Average True Range (ATR)**.
- **Momentum** via the **Relative Strength Index (RSI)**.
- **Trend-following** using a **Moving Average (MA)**.

This strategy offers a risk-adjusted market view, enabling traders to make informed buy and sell decisions based on calculated "serenity" levels of the market.

## Parameters

The Serenity Index Strategy uses the following default parameters, which can be customized:

```prorealtime
ATRPeriod = 14   // Period for ATR calculation
MAPeriod = 50    // Period for Moving Average calculation
RSIPeriod = 14   // Period for RSI calculation
```

These parameters can be adjusted to fit the desired time frame or trading style.

## Calculation

The Serenity Index is calculated using the following steps:

1. **True Range (TR):**
   - The True Range (TR) is the maximum value between:
     - `high - low` (current candle)
     - Absolute value of `high - close[1]` (previous close)
     - Absolute value of `low - close[1]` (previous close)

   ```prorealtime
   TRValue = max(high - low, max(abs(high - close[1]), abs(low - close[1])))
   ```

2. **Average True Range (ATR):**
   - The ATR is the moving average of the True Range over the `ATRPeriod`.

   ```prorealtime
   ATRValue = average[ATRPeriod](TRValue)
   ```

3. **Moving Average (MA):**
   - The moving average of the closing price over the `MAPeriod`.

   ```prorealtime
   MALong = average[MAPeriod](close)
   ```

4. **Relative Strength Index (RSI):**
   - Calculate the RSI using the `RSIPeriod`.

   ```prorealtime
   RSIValue = RSI[RSIPeriod]
   ```

5. **Serenity Index Calculation:**
   - The Serenity Index is calculated as follows:

   ```prorealtime
   SerenityIndex = (100 - ATRValue) * (RSIValue / 100) * (close / MALong)
   ```

This formula combines volatility, momentum, and trend-following into a single value, reflecting the market’s condition.

## Strategy Logic

### Buy Conditions:
- Enter a long position when:
  - **Serenity Index** rises above a defined threshold (e.g., 30) from oversold conditions, indicating bullish momentum.
  - Price action shows confirmation of a bullish reversal (e.g., bullish engulfing candle, hammer).

### Sell Conditions:
- Enter a short position when:
  - **Serenity Index** falls below a defined threshold (e.g., 70) from overbought conditions, indicating bearish momentum.
  - Price action confirms a bearish reversal (e.g., bearish engulfing candle, shooting star).

### Exit Strategy:
- Close long positions when the Serenity Index crosses above a higher threshold (e.g., 70), indicating overbought conditions.
- Close short positions when the Serenity Index crosses below a lower threshold (e.g., 30), indicating oversold conditions.

### Stop Loss:
- For long positions: Place the stop loss below the most recent swing low.
- For short positions: Place the stop loss above the most recent swing high.

### Take Profit:
- Optionally, use trailing stops or set a risk-reward ratio of at least 1:2 to lock in profits.

## Installation

To use this strategy in **ProRealTime**:

1. Open ProRealTime and navigate to the **Code** section.
2. Create a new indicator or strategy.
3. Copy and paste the following code into the script editor:

```prorealtime
// Parameters for the Serenity Index
ATRPeriod = 14
MAPeriod = 50
RSIPeriod = 14

// Calculate the components
TRValue = max(high - low, max(abs(high - close[1]), abs(low - close[1])))
ATRValue = average[ATRPeriod](TRValue)
MALong = average[MAPeriod](close)
RSIValue = RSI[RSIPeriod]

// Serenity Index Calculation
SerenityIndex = (100 - ATRValue) * (RSIValue / 100) * (close / MALong)

// Plot the Serenity Index
return SerenityIndex
```

4. Apply the indicator or strategy to your chart.
5. Adjust parameters or thresholds to fine-tune the strategy based on your preferred asset or time frame.

## Contributing

Contributions are welcome! To contribute:
1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m 'Add new feature'`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a Pull Request.

