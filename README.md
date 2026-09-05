# Green vs. Grey: A Risk-Adjusted Growth Opportunity Analysis of India's Energy Sector

## Project Question
Does India's renewable energy sector currently represent a stronger risk-adjusted growth opportunity than conventional/legacy power generation — and does sector labeling alone (green vs. grey) reliably predict that opportunity?

## Data & Basket Definitions
Five years of daily closing price data (2020–2025) were pulled for six NSE-listed Indian energy companies using the `yfinance` Python library, split into two baskets:

**Green Basket (renewable-focused):**
- Adani Green Energy (ADANIGREEN.NS)
- Suzlon Energy (SUZLON.NS)
- Inox Wind (INOXWIND.NS)

**Grey Basket (conventional / legacy power):**
- NTPC (NTPC.NS)
- Coal India (COALINDIA.NS)
- Tata Power (TATAPOWER.NS)

The Nifty 50 index (^NSEI) was used as the broader market benchmark for beta/alpha calculations.

## Methodology
Daily returns were computed from closing prices. From these, the following metrics were calculated for each stock:

- **Annualized Return** — average yearly price growth.
- **Annualized Volatility** — how much a stock's returns swing up and down over a year; the standard measure of risk.
- **Sharpe Ratio** — return earned per unit of risk taken (higher = better risk-adjusted performance).
- **Value at Risk (VaR, 95%)** — the daily loss a stock is not expected to exceed on 95% of days; a measure of downside risk.
- **Maximum Drawdown** — the worst peak-to-trough decline over the full period; captures the most severe loss an investor could have experienced.
- **Beta** — a stock's sensitivity to overall market movements (Nifty 50); beta > 1 means more volatile than the market, < 1 means less.
- **Alpha** — annualized excess return after adjusting for market exposure (beta); a positive alpha means the stock outperformed what its market risk alone would predict.
- **Growth Opportunity Score** (custom metric) — Annualized Return ÷ Annualized Volatility, used here as a risk-adjusted growth ranking across all six companies.
- **ARIMA Forecast** — a time-series model (AutoRegressive Integrated Moving Average) used to project each basket's average price 90 trading days forward, based purely on the series' own historical pattern.

## Quantitative Results

### Individual Company Metrics

| Ticker | Annual Return % | Annual Volatility % | Sharpe Ratio | VaR 95% (daily) | Max Drawdown % | Beta | Alpha (annual) |
|---|---|---|---|---|---|---|---|
| ADANIGREEN.NS | 44.07 | 53.14 | 0.83 | -0.0498 | -84.44 | 0.843 | 0.316 |
| SUZLON.NS | 78.89 | 57.48 | 1.37 | -0.0479 | -52.34 | 0.662 | 0.686 |
| INOXWIND.NS | 66.49 | 57.53 | 1.16 | -0.0481 | -64.77 | 0.964 | 0.518 |
| NTPC.NS | 26.26 | 28.82 | 0.91 | -0.0254 | -38.31 | 0.810 | 0.147 |
| COALINDIA.NS | 25.98 | 31.67 | 0.82 | -0.0302 | -43.40 | 0.819 | 0.140 |
| TATAPOWER.NS | 42.51 | 38.18 | 1.11 | -0.0367 | -55.65 | 1.107 | 0.263 |

### Growth Opportunity Score — Ranked

| Rank | Ticker | Score | Basket |
|---|---|---|---|
| 1 | SUZLON.NS | 1.37 | Green |
| 2 | INOXWIND.NS | 1.16 | Green |
| 3 | TATAPOWER.NS | 1.11 | Grey |
| 4 | NTPC.NS | 0.91 | Grey |
| 5 | ADANIGREEN.NS | 0.83 | Green |
| 6 | COALINDIA.NS | 0.82 | Grey |

*See `Figure_3.png` — bars colored by basket, with the Adani Green / Tata Power anomaly annotated directly on the chart.*

### Basket-Level Comparison

| Basket | Annual Return | Annual Volatility | Sharpe Ratio |
|---|---|---|---|
| Green | 63.15% | 38.33% | 1.65 |
| Grey | 31.58% | 27.18% | 1.16 |

## Market Benchmarking (vs. Nifty 50)
All three green-basket stocks carry a beta below 1.0, meaning they moved *less* than the overall market on average — yet they generated substantially higher alpha (31–69% annually) than the grey basket (14–26%). This indicates the green basket's outperformance came from stock-specific factors rather than simply taking on more market-wide risk.

## Forecasting
A 90-day ARIMA forecast was generated for both baskets' average price:
- **Green Basket**: forecast flattens around ~377
- **Grey Basket**: forecast flattens around ~356, following a recent recovery off multi-month lows

Both forecasts converge to a near-flat trend beyond a few days out — consistent with the well-documented near-random-walk behavior of stock prices. This is an expected property of ARIMA on price data, not a modeling flaw; it illustrates methodology rather than predicting a strong directional move.

*See `Figure_1.png` and `Figure_2.png`.*

## Risk Summary
Adani Green stands out as the highest-risk name in the dataset by a wide margin: a maximum drawdown of -84.44%, far steeper than any other stock analyzed, despite posting a respectable +31.6% alpha. This shows that its low risk-adjusted score is driven by an extreme downside event rather than weak underlying returns — a distinction a Sharpe ratio or Growth Opportunity Score alone doesn't fully convey.

## Discussion: Beyond the Numbers
*The following section is qualitative and conceptual — it draws on economic theory to interpret the quantitative findings above, since abatement-cost and technology-cost-curve data were outside this project's scope.*

**Dynamic efficiency** refers to whether a sector or firm improves its cost structure and technology over time, rather than being judged only on today's snapshot. Global renewable technology (solar, wind components) has followed a well-documented cost-decline curve through learning-by-doing and R&D. This plausibly explains why equipment-focused renewable names (Suzlon, Inox Wind) scored highest — the market may be pricing in this sector-wide tailwind. Adani Green's underperformance, however, shows that a company operating in a dynamically efficient *sector* does not guarantee dynamically efficient or well-priced *execution* — firm-specific risks (leverage, governance, investor sentiment) can dominate sector-level advantages.

**Pollution abatement cost** — the cost a polluter bears to reduce emissions — represents a forward-looking risk for conventional generators like NTPC and Coal India that isn't visible in backward-looking volatility or return metrics. Tightening emission regulations and compliance costs could compress margins for legacy players going forward, suggesting the grey basket's historical risk-adjusted scores may overstate its future attractiveness relative to renewables.

## Conclusion
This analysis compared risk-adjusted growth opportunity across six Indian energy stocks split into a renewable-focused basket (Adani Green, Suzlon, Inox Wind) and a conventional basket (NTPC, Coal India, Tata Power). The green basket delivered a stronger risk-adjusted growth score (1.65 vs. 1.16 Sharpe) and higher alpha despite lower market beta, indicating genuine stock-specific outperformance rather than simply riskier market exposure. However, individual rankings revealed that sector labels are an unreliable standalone guide: Tata Power, a legacy player transitioning toward renewables, outscored pure-play Adani Green, whose -84.4% maximum drawdown — the worst in the dataset — explains its low ranking despite meaningful alpha generation. A 90-day ARIMA forecast for both baskets converged to a near-flat trend, consistent with typical stock price behavior. Interpreted through a dynamic-efficiency and pollution-abatement-cost lens, these results suggest renewable energy represents the stronger current growth opportunity in India's power sector overall, while underscoring that firm-level execution and risk — not sector labels — should drive individual investment or strategic decisions.

## Tools Used
Python, yfinance, pandas, numpy, matplotlib, statsmodels (ARIMA)

## Author
Sramantika Sen
