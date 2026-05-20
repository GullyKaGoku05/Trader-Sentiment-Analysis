# Trader Performance vs Market Sentiment
**Primetrade.ai — Data Science Intern Round-0 Assignment**  
**Candidate:** Nikhil Kshirsagar | **Date:** 20th May 2025

---

## Methodology

**Data Sources**  
Two datasets were used: 211,224 Hyperliquid on-chain trade records (32 accounts, May 2023–May 2025) and 2,644 daily Bitcoin Fear/Greed Index readings. Both were clean, with no missing values, no duplicates.

**Preprocessing**  
Timestamps were parsed and normalized to the date level for alignment. The five Fear/Greed labels were collapsed into three regimes Fear, Neutral, Greed to ensure statistically meaningful group sizes. Only exit trades (Close Long, Close Short, Sell, Buy) were retained, since realized PnL only crystallizes at position close. Each exit received a `net_pnl` column (Closed PnL minus fees) and a binary `win` flag. A leverage proxy was computed as position size relative to starting position, capped at 50× to contain outliers.

**Feature Engineering**  
Analysis operated at two granularities:

- *Trader-day level:* daily PnL, win rate, trade count, long/short ratio, average leverage, average position size merged with the day's sentiment regime.
- *Account level:* total PnL, win rate, trade frequency, leverage distribution, PnL stability (Sharpe-like ratio), and fear exposure (% of volume on Fear days) used for segmentation.

A Greed-streak counter was added to study the effect of sustained sentiment on behavior. A lag variable (`prev_regime`) enabled next-day reaction analysis.

**Segmentation**  
K-means clustering (k=4) on five standardized account-level features produced four archetypes: Consistent Winner, Aggressive Scalper, Emotion-Reactive, and Low-Risk Passive.

---

## Key Insights

**1. Greed maximizes PnL magnitude; Fear maximizes win rate**  
Greed days produce the largest wins (median $587/trader-day, 69.3% win rate). Fear days yield smaller but more frequent wins (median $460, 71.4% win rate). Neutral is weakest on both dimensions. The practical implication: these are not equivalent regimes. A Fear environment rewards precision, a Greed environment rewards conviction.

**2. Fear triggers hyperactivity; Greed triggers larger bets**  
The day after a Fear regime, traders place 40%+ more trades than after a Greed regime. On Greed days, average position size is measurably larger than traders' scale-up size, not necessarily frequency. Directional bias (long/short ratio ~0.5) is flat across all regimes, meaning sentiment does not change *which way* traders bet, only *how much* and *how often*.

**3. Staying active during Fear is the highest-conviction signal**  
The day following a Fear regime produces an average PnL of $6,042 and a 71.7% win rate, 28% higher PnL, and 2.5 percentage points more accurate than after Greed days. Traders who exit the market during Fear miss the recovery window and self-select out of the best-returning sequence.

**4. Greed streaks produce overtrading without proportional gain**  
When Greed persists for 6+ consecutive days, trade frequency reaches its highest level but PnL growth flattens. This is a textbook overtrading signature: more cost and noise without more edge.

**5. Low leverage is the dominant strategy on Greed days**  
Low-leverage traders earned a median of $1,441 on Greed days vs $458 for high-leverage traders, 3.1× gap. Counterintuitively, high leverage does not amplify Greed-day gains; it suppresses them. On Fear days, the gap narrows, suggesting that high leverage's highest cost is paid when markets are trending up, and over-positioned traders get shaken out.

**6. No revenge trading detected — discipline is selective**  
After a losing Fear day, traders cut next-day activity by 44% (77.6 vs 136.8 trades). Across both regimes, activity always drops after a loss. The cohort shows no revenge overtrading signal — but the pullback is sharper on Fear days, suggesting emotional amplification when sentiment is already negative.

---

## Strategy Recommendations

**Rule 1: "On Greed days, increase size, not leverage" (for all segments)**  
Directly from Insight 5: low-leverage traders earn 3× more on Greed days. When the Fear/Greed index enters Greed, the correct response is to increase *notional position size* while keeping the leverage multiplier low or flat. High leverage on Greed days introduces liquidation risk that nets against the directional tailwind.

**Rule 2: "During Fear, stay active with tighter position sizes; don't exit the market"**  
Fear days carry the highest win rate (71.4%), and the post-Fear day is the single best-performing trading day in the dataset ($6,042 avg PnL). For Consistent Winners and Aggressive Scalpers, the optimal Fear playbook is: maintain activity, tighten individual trade size to manage downside, and hold through into the recovery. Emotion-Reactive traders — who already concentrate disproportionate volume in Fear — should specifically tighten size (not frequency) to avoid the erratic PnL profile seen in Chart 8.

**Rule 3: "After 5 consecutive Greed days, cap daily trade count"**  
Prolonged Greed (5+ days) is a hard threshold after which additional trades generate fees and noise without proportional PnL uplift (Insight 4). A practical implementation: flag the streak counter in the trading system and enforce a daily trade cap (e.g., 80% of the trailing 5-day average) once the streak exceeds 5. This rule is especially relevant for Aggressive Scalpers, whose high base frequency makes them most exposed to overtrading in extended Greed.

---
