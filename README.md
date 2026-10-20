# Do hot stocks keep winning? Momentum outliers in US stocks, 1990–2026

This repo holds the code and summary results behind the [I'm A Doctor Not A Quant](https://www.youtube.com/@ImADoctorNotAQuant) video *Do Hot Stocks Keep Winning? I Tested 4,000 Stocks Since 1990*.

Each week I scored every liquid US stock against the rest of the market and flagged the ones that broke away from the pack. Then I asked what they did next.

## Short answer

- **Outliers are more likely to be next week's big movers, even against equally volatile stocks.** 35–40% of outliers land in next week's top or bottom 10%, against 20% by chance. Look-alike stocks, matched on 52-week volatility, liquidity and price, land there 30–34% of the time; outliers do 37–42% (2016–2026). So more than half of the raw gap is volatility, but 6–9 points remain, and every block-bootstrap interval sits above +5.
- **Over one week, extremes snap back.** Buying last week's bottom 10% and selling the top 10% earned +28% a year before costs since 1990. By decade that was about 60% a year in the 1990s and about 12% in the 2020s, which is no longer significant. Since 2016 it breaks even at about 7 bps per side, about a third of the estimated cost.
- **News decides much of the direction.** Last week's biggest winners with no earnings release lagged the market the next week, by 14% a year (t = −3.5). Winners with an earnings release held up (Chan 2003). This held in every decade, but it is weaker lately.
- **Over 12 months, winners and winning industries kept winning.**
  - **Stocks:** buying every 12-month outlier (skipping the latest month) beat the market by +12% a year before costs, positive in every decade.
  - **Industries:** in Ken French's industry portfolios, long minus short 12-month outliers earned +15% a year before costs (+13% after 5 bps per side), positive in every decade.
- **The real test fails.** I froze each of the 44 headline rules and its trade direction on data before 2016, then scored 2016–2026 after costs, with 100 bps a year to borrow. 15 of 44 made money; none passed a one-sided Benjamini–Hochberg test (best q = 0.16).

## Risk-adjusted scorecard

1990–2026, after estimated costs. Buy-only rules are measured against the market.

| Rule | Hold | Return after costs (% a year) | Sharpe | Max drawdown | 7-factor alpha after costs (t) | Break-even cost (bps per side) | After costs, 2016–2026 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Buy last week's crashes (1-week down-outliers) | 1 wk | −10.5 | 0.01 | −90% | −19.2% (−8.5) | 26 | −16.1 |
| Buy 12-month outliers, equal-weighted | 4 wk | +7.8 | 0.62 | −69% | +2.6% (1.3) | 126 | +3.2 |
| Buy 12-month outliers, liquidity-weighted | 4 wk | +16.3 | 0.72 | −70% | +11.9% (3.0) | 192 | +23.8 |
| 12-month winners minus losers (top vs bottom 10%) | 1 wk | −3.1 | −0.13 | −95% | −10.8% (−5.5) | 28 | +0.7 |
| Industries: 12-month outliers, up minus down | 1 wk | +12.8 | 0.49 | −55% | +7.1% (2.1), before costs | 30 | +11.4 |
| Market, buy and hold | — | — | 0.57 | −54% | — | — | Sharpe 0.76 |

The seven factors are Fama–French 5 plus momentum (UMD) and short-term reversal (STREV). The equal-weighted outlier rule's small alpha says it is mostly the standard momentum factor.

Since 2016, the liquidity-weighted version leaned on five stocks (NVDA, TSLA, MU, SNDK, AMD), which supplied 81% of its return. Without them it earns +12% a year (t = 1.4). Capped at 5% a stock, or equal-weighted across the most liquid third, it earns +17% to +19%, and +14% to +15% without their top 5. The top 5 were picked after the fact, so treat that as a stress test.

Sources: `results/tables/21a_verdicts_headline_full.csv`, `21a_verdicts_headline_recent.csv`, `17_industries_risk_adjusted.csv`, `18_drawdowns_and_crash_episodes.csv` and `14b_big_stock_concentration.csv`.

![The fixed-direction holdout](results/figures/camera/10_scoreboard.png)

## Robustness checks

| Check | Result | Table |
| --- | --- | --- |
| Volatility matching (13-week volatility, liquidity, price) | Tail excess of +5.6 to +9.6 points over the full sample (t 24–39) | `28_volatility_matched_tails.csv` |
| Longer matching (52-week volatility, beta, industry, liquidity, price) | +5.5 to +9.4 points in 2016–2026, block-bootstrap CIs all above zero | `35_longer_volatility_matching.csv` |
| Logistic regression, week-clustered | Odds ratio 1.5–1.9 for landing in either tail | `28b_logit_odds_ratios.csv` |
| Skip a day (enter at the next close) | The 1-week reversal keeps 87% of its return. The 12-1 long-short keeps 26% at a 1-week hold, because the loss sits in losers on the week's first day; a drop-last-day placebo keeps 93–97% | `15_skip_a_day_full.csv` |
| Ken French replication (monthly 12-1 deciles) | Equal-weighted spread gap −0.8% a year (t = −0.3); correlation with UMD 0.91 | `07b_ken_french_replication.csv` |
| Industry momentum, Moskowitz–Grinblatt design | 6-month formation, 6-month hold: +5.6% a year since 1990 (t = 2.6) | `17b_moskowitz_grinblatt_*.csv` |
| Four cost models and a flat cost grid | Break-even costs reported for every headline rule | `20_cost_model_robustness.csv` |
| Momentum crashes | 12-1 decile spread: −57% after costs in the Mar–May 2009 rebound; −20% in the Nov 2020 vaccine week | `18_drawdowns_and_crash_episodes.csv` |
| Holding period | Risk-adjusted payoff of 12-month winners peaks at 4–8 weeks | `19_holding_period_appraisal.csv` |
| Evergreen check by decade | Big movers held in every decade; the snap-back and the news effect weakened lately | `22_evergreen_by_decade.csv` |

## Limitations

- **Survivors only.** The universe is today's NYSE and Nasdaq listings, so companies that delisted are missing.
  - This flatters buy-only rules: the universe beats Ken French's market by about 3.4 points a year (t = 3.6).
  - The industry results use Ken French's portfolios, which include delisted companies.
  - Section 41 asks what it would take for survivorship to erase the big-movers result. The answer is 15–44% of outliers missing, *and* those missing stocks calmer next week than ordinary look-alikes. These are assumptions, not an estimate.
- **Costs are estimated from daily prices.** The pooled Abdi–Ranaldo estimate cannot resolve spreads of a few bps, so it overcharges the most liquid stocks. Break-even costs are the more reliable number.
- **Pseudo out-of-sample.** The pre-2016 freeze in section 30 is honest about direction, but I had already looked at the later years while building the rules.
- **Industry labels use current SEC codes.**
- **Ken French data end in Aug 2026.** After that, the market is extended with VTI and T-bill yields.

## Repository layout

```
momentum_outliers_v7.ipynb   the notebook, with its outputs (renders on GitHub)
results/README.md            index of every table and figure
results/tables/              75 summary tables (CSV)
results/snapshot_2026-10-16/ the dated "this week" screen (CSV + Markdown)
results/figures/camera/      16 video figures
results/figures/analysis/    18 working charts
results/consistency_checks.txt
requirements.txt
```

## Reproduce

1. Install Python 3.13 (tested), then run `pip install -r requirements.txt`.
2. Open the notebook and set `SEC_USER_AGENT` in the config cell to your own name and email; SEC EDGAR refuses anonymous requests.
3. Set `UPDATE_PRICES_TO` to the last completed Friday, or `None` to use only the first download.
4. Run All.
   - The first run downloads daily prices for about 4,500 tickers from Yahoo Finance and 8-K filing histories from EDGAR (about 30 minutes for EDGAR).
   - Everything is cached in `data_momentum/`, and outputs go to `outputs_v7/`. Git ignores both.

Your numbers will differ slightly from these. Yahoo restates history after splits and dividends, and today's listings change.

Sections 36–39 are optional. They need open prices, or an authorized, delisting-inclusive CRSP extract that you supply. Without those inputs they skip.

## Data

No raw market data is included. The tables are summary statistics, and the stock lists leave out price and volume levels.

- **Prices:** Yahoo Finance via `yfinance`.
- **Factors, deciles and industry portfolios:** the [Kenneth R. French Data Library](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html).
- **Tickers, exchanges, SIC codes, states of incorporation and 8-K filings:** SEC EDGAR.

## References

- Jegadeesh (1990), "Evidence of Predictable Behavior of Security Returns," *Journal of Finance*.
- Lehmann (1990), "Fads, Martingales, and Market Efficiency," *Quarterly Journal of Economics*.
- Jegadeesh & Titman (1993), "Returns to Buying Winners and Selling Losers," *Journal of Finance*.
- Moskowitz & Grinblatt (1999), "Do Industries Explain Momentum?," *Journal of Finance*.
- Chan (2003), "Stock Price Reaction to News and No-News: Drift and Reversal After Headlines," *Journal of Financial Economics*.
- Da, Gurun & Warachka (2014), "Frog in the Pan: Continuous Information and Momentum," *Review of Financial Studies*.
- Daniel & Moskowitz (2016), "Momentum Crashes," *Journal of Financial Economics*.
- Abdi & Ranaldo (2017), "A Simple Estimation of Bid-Ask Spreads from Daily Close, High, and Low Prices," *Review of Financial Studies*.
- Newey & West (1987); Fama & MacBeth (1973); Benjamini & Hochberg (1995).

## Disclaimer

For education only. This is not financial advice, and past results don't guarantee future returns.

## License

MIT. See `LICENSE`.
