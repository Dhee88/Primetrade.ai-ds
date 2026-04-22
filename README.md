# Trader Performance vs Market Sentiment Analysis

**Primetrade.ai Data Science Intern – Round 0 Assignment**

---

## Overview

This project analyzes the relationship between Bitcoin market sentiment (measured by the Fear & Greed Index) and the trading behavior and performance of Hyperliquid traders. The goal is to uncover actionable patterns that can inform smarter, sentiment-aware trading strategies.

The analysis addresses three core questions:
1. Does trader performance (PnL, win rate) differ between Fear and Greed market regimes?
2. Do traders change their behavior (trade frequency, position size, long/short bias) based on sentiment?
3. Can we segment traders to identify who performs best in which market conditions?

---

## Repository Structure

```
.
├── trader_sentiment_analysis3.ipynb  
├── data/                              
│   ├── historical_data.csv
│   └── fear_greed_index.csv
├── output/                          
│   ├── 01_daily_pnl_by_sentiment_bucket.png
│   ├── 02_behavior_by_sentiment_bucket.png
│   ├── 03_profitability_and_loss_rates.png
│   ├── 04_frequency_segment_median_pnl.png
│   ├── 05_size_segment_median_pnl.png
│   ├── 06_winner_segment_median_pnl.png
│   ├── data_quality_summary.csv
│   ├── overall_sentiment_summary.csv
│   ├── fear_greed_comparison.csv
│   ├── account_segment_summary.csv
│   ├── frequency_segment_summary.csv
│   ├── size_segment_summary.csv
│   ├── winner_segment_summary.csv
│   └── assignment_summary.md
├── README.md                                                  
```

---

## Datasets

### 1. `historical_data.csv` (Hyperliquid Trader History)

| Column | Description |
|--------|-------------|
| `Account` | Trader wallet address |
| `Coin` | Trading pair symbol (e.g., BTC, ETH, HYPE) |
| `Execution Price` | Trade execution price |
| `Size Tokens` | Quantity in token units |
| `Size USD` | Notional value in USD |
| `Side` | BUY or SELL |
| `Timestamp IST` | Trade timestamp (India Standard Time) |
| `Start Position` | Position size before trade (0 = new position) |
| `Direction` | Buy / Sell |
| `Closed PnL` | Realized profit/loss for closing trades (0 for opens) |
| `Fee` | Trading fee paid |
| `Transaction Hash` | Unique transaction identifier |
| `Order ID` | Order identifier |
| `Trade ID` | Trade identifier |

### 2. `fear_greed_index.csv` (Bitcoin Fear & Greed Index)

| Column | Description |
|--------|-------------|
| `timestamp` | Unix epoch timestamp |
| `value` | Index value (0 = Extreme Fear, 100 = Extreme Greed) |
| `classification` | Categorical label: `Extreme Fear`, `Fear`, `Neutral`, `Greed`, `Extreme Greed` |
| `date` | Date (YYYY-MM-DD) |

---

## Setup and How to Run

### Prerequisites

- Python 3.8 or higher
- Jupyter Notebook or JupyterLab
- Required Python packages:
  ```bash
  pip install pandas numpy matplotlib seaborn
  ```

### Step-by-Step Instructions

1. **Clone or download** this repository to your local machine.
2. **Create a `data/` folder** in the project root directory.
3. **Download the datasets** and place them inside the `data/` folder:
   - `historical_data.csv`
   - `fear_greed_index.csv`
4. **Launch Jupyter Notebook**:
   ```bash
   jupyter notebook
   ```
5. **Open** `trader_sentiment_analysis3.ipynb`.
6. **Verify file paths** in the first code cell (update if your paths differ):
   ```python
   TRADES_PATH = Path("data/historical_data.csv")
   SENTIMENT_PATH = Path("data/fear_greed_index.csv")
   ```
7. **Run all cells** sequentially (`Cell` → `Run All`).

The notebook will automatically:
- Clean and align the data
- Generate daily performance metrics per trader
- Merge with sentiment data
- Create visualizations and summary tables
- Save all outputs to the `output/` directory

---

## Output Description

After a successful run, the `output/` folder will contain:

### Charts (`.png`)

| File | Description |
|------|-------------|
| `01_daily_pnl_by_sentiment_bucket.png` | Boxplot of daily PnL across Fear, Neutral, and Greed buckets |
| `02_behavior_by_sentiment_bucket.png` | Three-panel chart: trade count, average trade size, and long ratio by sentiment |
| `03_profitability_and_loss_rates.png` | Bar chart showing profitable-day and loss-day rates per sentiment bucket |
| `04_frequency_segment_median_pnl.png` | Median PnL by sentiment for High vs. Low Frequency traders |
| `05_size_segment_median_pnl.png` | Median PnL by sentiment for Large vs. Small Size traders |
| `06_winner_segment_median_pnl.png` | Median PnL by sentiment for Winners vs. Losers |

### Tables (`.csv`)

| File | Description |
|------|-------------|
| `data_quality_summary.csv` | Row counts, duplicates, and missing values for each dataset |
| `overall_sentiment_summary.csv` | Aggregate statistics by sentiment bucket |
| `fear_greed_comparison.csv` | Direct Fear vs. Greed comparison of key metrics |
| `account_segment_summary.csv` | Per-account summary with segment labels |
| `frequency_segment_summary.csv` | Metrics by frequency segment and sentiment |
| `size_segment_summary.csv` | Metrics by size segment and sentiment |
| `winner_segment_summary.csv` | Metrics by winner segment and sentiment |
| `assignment_summary.md` | Auto-generated markdown report of findings |

---

## Methodology Summary

### 1. Data Preparation
- Loaded both raw datasets and documented their dimensions, duplicates, and missing values.
- Converted `Timestamp IST` to daily dates; aligned with the Fear & Greed Index by date.
- Removed duplicate trade records to avoid inflating trade counts.

### 2. Metric Engineering
For each trader-day combination:
- **Daily Realized PnL**: Sum of `Closed PnL`
- **Win Rate**: Proportion of realized trades with positive PnL
- **Trade Count**: Total number of trades
- **Average Trade Size**: Mean of `Size USD`
- **Long/Short Ratio**: Proportion of BUY trades among directional trades

### 3. Sentiment Grouping
Sentiment classifications were grouped into three buckets for robust comparison:
- **Fear**: `Extreme Fear` + `Fear`
- **Neutral**: `Neutral`
- **Greed**: `Greed` + `Extreme Greed`

### 4. Trader Segmentation
Three segment pairs were created using median splits:
- **Frequent vs. Infrequent**: Based on median daily trade count
- **Large Size vs. Small Size**: Based on median trade size (USD)
- **Winners vs. Losers**: Based on median total account PnL

### 5. Analysis
- Compared performance and behavior metrics across sentiment buckets.
- Analyzed segment-specific performance to identify which trader types thrive in which regimes.
- Generated visualizations and exported summary tables.

**Note on Leverage:** The original dataset does not contain a leverage column. Average trade size is used as the closest observable proxy for risk appetite.

---

## Key Insights

### Insight 1: High-frequency traders capture the Greed advantage
Median daily PnL for high-frequency traders rises significantly from Fear to Greed, while low-frequency traders see minimal improvement. The ability to trade quickly appears to be a key differentiator in momentum-driven Greed markets.

### Insight 2: Large-size traders lose edge in Greed markets
Despite overall positive sentiment in Greed regimes, large-size traders experience a decline in median PnL compared to Fear days. In contrast, small-size traders improve. This suggests execution drag or crowding effects disproportionately impact larger positions during trending moves.

### Insight 3: Winners and losers represent different trading processes
Winners maintain positive median PnL even in Greed markets, while losers struggle even in Fear conditions. The performance gap is not explained by market direction alone—it reflects differences in discipline, strategy fit, and risk management.

---

## Strategy Recommendations

| Market Regime | Rule | Action | Rationale |
|---------------|------|--------|-----------|
| **Fear** | Reduce risk for low-frequency and losing traders | Cap position sizes and require stricter entry criteria for these segments | These groups do not generate sufficient PnL in Fear to justify high exposure |
| **Greed** | Keep fast traders active, but trim size for large notional traders | Allow high-frequency traders to participate fully; reduce position limits for large-size accounts | Greed benefits fast execution, but larger traders lose edge as crowding increases |

These rules can be implemented as sentiment-based risk overlays in a live trading system.

---

## Future Work (Optional Extensions)

- **Predictive Modeling**: Use sentiment and behavioral features to predict next-day profitability or PnL volatility.
- **Clustering Analysis**: Apply unsupervised learning (e.g., K-means) to identify natural trader archetypes beyond simple median splits.
- **Interactive Dashboard**: Build a Streamlit app to explore segment performance dynamically.
- **Leverage Estimation**: If position size and account equity data become available, estimate implied leverage for more precise risk analysis.

---
