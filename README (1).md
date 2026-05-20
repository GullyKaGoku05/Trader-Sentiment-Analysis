# Trader Performance vs Market Sentiment
**Candidate:** Nikhil Kshirsagar 
**Date:** May 2025

---

## Overview

This project analyzes how Bitcoin market sentiment (Fear/Greed Index) relates to trader behavior and realized performance on **Hyperliquid**, a decentralized perpetuals exchange. The analysis covers 211,224 on-chain trade records across 32 trader accounts from May 2023 to May 2025, merged with 2,644 daily sentiment readings to surface actionable, evidence-backed strategy rules.

## Datasets

| Dataset | Source | Description |
|---|---|---|
| `historical_data.csv` | [Google Drive](https://drive.google.com/file/d/1IAfLZwu6rJzyWKgBToqwSmmVYU6VbjVs/view) | Hyperliquid on-chain trades (211,224 rows) |
| `fear_greed_index.csv` | [Google Drive](https://drive.google.com/file/d/1PgQC0tO8XN-wqkNyghWc_-mnrYv_nhSf/view) | Bitcoin Fear/Greed daily index (2,644 rows) |

> **Download both files and place them in the same directory as the notebook before running.**

---

## Setup & How to Run

### 1. Clone / Download

```bash
git clone https://github.com/<your-username>/trader-sentiment-analysis.git
cd trader-sentiment-analysis
```

### 2. Install Dependencies

Python 3.9+ recommended.

```bash
pip install pandas numpy matplotlib scikit-learn jupyter
```

Or with a virtual environment:

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install pandas numpy matplotlib scikit-learn jupyter
```

### 3. Add the Data Files

Download `historical_data.csv` and `fear_greed_index.csv` from the Google Drive links above and place them in the project root:

```
trader-sentiment-analysis/
├── historical_data.csv       ← place here
├── fear_greed_index.csv      ← place here
├── TraderPerformance.ipynb
...
```

### 4. Run the Notebook

```bash
jupyter notebook TraderPerformance.ipynb
```

Run all cells top-to-bottom (`Kernel → Restart & Run All`). Charts will be saved to the `outputs/` folder automatically.

---


### Key Insights

**Insight 1: Greed generates larger wins; Fear generates more consistent wins**  
Greed days: median PnL **$587**, win rate **69.3%**.  
Fear days: median PnL **$460**, win rate **71.4%**.  
Neutral days are weakest on both dimensions.

**Insight 2: Fear triggers hyperactivity; Greed triggers bigger bets**  
Traders fire 40%+ more trades the day after a Fear regime. On Greed days, average position size grows, and traders bet larger when confident. Directional bias (long/short ratio) stays flat across regimes.

**Insight 3: Fear-day survivors outperform the following day**  
Day after Fear: avg PnL **$6,042**, win rate **71.7%**.  
Day after Greed: avg PnL **$4,729**, win rate **69.2%**.  
Staying active during Fear selectively filters skilled traders and the rebound is real.

**Insight 4: Greed streaks produce overtrading without proportional returns**  
Trade count rises monotonically with streak length. PnL growth does not keep pace. Sustained Greed (6+ days) shows elevated activity but diminishing per-trade returns.

**Insight 5: Low leverage dominates on Greed days**  
Low-leverage traders: Greed-day median PnL **$1,441**.  
High-leverage traders: Greed-day median PnL **$458** (3.1× gap).  
On Fear days, the gap narrows; high leverage is penalized more on Greed days than Fear.

**Insight 6: No revenge trading; survivors are disciplined**  
After a loss on a Fear day, traders average only 77.6 next-day trades vs 136.8 after a win — a 44% pullback. The pattern holds across regimes, showing the cohort practices loss of discipline, not revenge overtrading.


## Dependencies

```
pandas >= 1.5
numpy >= 1.23
matplotlib >= 3.6
scikit-learn >= 1.1
jupyter >= 1.0
```

---

## Contact

**Nikhil Kshirsagar**  
[nikhilkshirsagar.iitb@gmail.com]  
[www.linkedin.com/in/nikhil-kshirsagar-0220712a6]  
