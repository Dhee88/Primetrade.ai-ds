# Trader Performance vs Market Sentiment

## Methodology
- Loaded both raw datasets, checked shapes, missing values, and duplicate rows.
- Converted `Timestamp IST` to daily timestamps and aligned Hyperliquid trader-days with the Bitcoin Fear & Greed index on the calendar date.
- Built a trader-day panel across all 32 accounts instead of focusing on a single wallet.
- Created the core metrics required by the assignment: daily PnL, profitable-day rate, loss-day rate, trade count, average trade size, total volume, and long/short ratio.
- Added three shortlist-relevant account segments: Frequent vs Infrequent, Large Size vs Small Size, and Winners vs Losers using a median total-PnL split.
- Note: this CSV does not include a true leverage column. The notebook explicitly states that leverage cannot be measured directly and uses position size as the closest observable risk-taking proxy instead.

## Data Preparation Summary
- `historical_data.csv`: 211,224 rows, 16 columns, 0 duplicate rows, 0 missing values.
- `fear_greed_index.csv`: 2,644 rows, 4 columns, 0 duplicate rows, 0 missing values.
- Analysis coverage: 32 accounts, 2,340 trader-days, date range 2023-05-01 to 2025-05-01.

## Fear vs Greed Comparison
- Avg PnL: Fear $5185.15 vs Greed $4144.21
- Avg realized win rate: Fear 84.2% vs Greed 85.6%
- Avg trades/day: Fear 105.4 vs Greed 76.9
- Avg position size: Fear $8529.86 vs Greed $5954.63
- Avg long ratio: Fear 52.2% vs Greed 47.2%

## Stronger Insights
1. High-frequency traders capture almost all of the regime edge.
   Their median daily PnL jumps from $188.56 on Fear days to $587.63 on Greed days, while low-frequency traders stay near flat in Greed at $0.00.
   This suggests the positive Greed regime is monetized mainly by traders who can recycle risk quickly rather than by the broader trader base.

2. Bigger-size traders underperform when sentiment turns Greed, despite the market-level uplift.
   Large Size accounts fall from $459.15 in Fear to $129.66 in Greed, while Small Size accounts improve to $289.26.
   That pattern is consistent with execution drag or looser risk management when larger traders chase crowded Greed moves.

3. Winners and losers behave like different systems, not just better and worse versions of the same system.
   Winner accounts post $312.13 median daily PnL in Greed, while Loser accounts are already at $78.81 in Fear.
   The strongest consistency regime is `Consistent Winners` on `Greed` days with a profitable-day rate of 76.3%, which points to process discipline rather than sentiment timing alone.

## Actionable Strategy Ideas
1. During Fear, reduce risk for slower or less consistent traders.
   If a trader is in the Low Frequency or Loser segment, cap position size and require higher-quality setups in Fear because these groups do not show enough payoff to justify volatility exposure.

2. During Greed, lean into fast execution but cut size for large notional traders.
   Greed is favorable for High Frequency accounts, so trend-following or momentum-style deployment can stay active there, but Large Size accounts should reduce notional size or tighten risk filters because their edge deteriorates as crowding rises.

## Files
- Charts: `output/*.png`
- Tables: `output/*.csv`
- This summary: `output/assignment_summary.md`
