VWAP Mean Reversion in Futures Markets
======================================

Statistical Validation, Data Engineering, and Systematic Strategy Research
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ZN 10-Year Treasury Note Futures  ·  6E Euro FX Futures
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

   **Bottom line**: ZN Treasury futures exhibit a statistically
   significant intraday mean-reversion signal around the daily VWAP —
   measurable, filterable, and profitable after realistic transaction
   costs. The same framework applied to Euro FX produces a consistent
   negative result. The project treats statistical significance and
   economic profitability as related but distinct questions, and
   presents both outcomes honestly.

**Research Workflow:** The complete analysis is documented in a single
Jupyter notebook — from raw Databento tick data through statistical
validation, backtesting, Monte Carlo analysis, and deployment testing.

Highlights
----------

ZN Treasury Futures
~~~~~~~~~~~~~~~~~~~

- **Identified and corrected** systematic contamination in commercial
  futures tick data — cross-validated against independently sourced
  Rithmic historical bars
- **Intraday AR(1) half-life ≈ 19 minutes** (R² = 0.932), confirming
  statistically significant mean reversion
- **Live z-score exits outperform fixed-price targets** — the project
  decomposes z-score reversion into price movement and VWAP drift, and
  demonstrates that exits must track both
- **Regime filters doubled trade expectancy** while reducing frequency
  by ~72%

+---------------+---------+------------+-------------+--------------+----------+----------+
| Configuration | Trades  | Expectancy | Total PnL   | Max DD       | Sharpe   | Sortino  |
+===============+=========+============+=============+==============+==========+==========+
| 2.0 / 1.0 /   | 864     | $37        | $31,607     | −$13,679     | 0.74     | 0.78     |
| 4.0           |         |            |             |              |          |          |
+---------------+---------+------------+-------------+--------------+----------+----------+
| **2.5 / 1.0 / | **484** | **$76**    | **$36,875** | **−$11,082** | **0.97** | **1.32** |
| 5.0**         |         |            |             |              |          |          |
+---------------+---------+------------+-------------+--------------+----------+----------+
| 3.0 / 1.0 /   | 264     | $122       | $32,145     | −$11,453     | 0.83     | 1.19     |
| 5.5           |         |            |             |              |          |          |
+---------------+---------+------------+-------------+--------------+----------+----------+

*10 contracts, full historical period. Entry σ / Exit σ / Stop σ.*

- **Best prop-firm configuration**: ~56% Monte Carlo evaluation pass
  rate under Apex 150K trailing drawdown rules
- **Live deployment** via Rithmic API — confirmed real fills, bracket
  order management, and fault-tolerant reconnect logic

6E Euro FX
~~~~~~~~~~

- Mean reversion is **statistically present** (half-life ≈ 38 min,
  reversion rate 79%)
- Strategy is **consistently unprofitable after realistic commissions**
  across all tested configurations
- Demonstrates the distinction between *statistical significance* and
  *economic profitability*: the lower tick value ($6.25 vs $15.625) and
  identical commission structure make the same signal unviable in 6E

Equity Curves
-------------

*Three filtered ZN configurations over the full historical period (10
contracts each). The 2.5/1.0/5.0 configuration produces the strongest
standalone risk-adjusted profile.* The figures below summarize the
progression from cleaned data to validated trading results.

.. figure:: images/zn_equity_curves.png
   :alt: ZN Equity Curves

   ZN Equity Curves

Data Quality
------------

*Before and after front-month contract filtering. Parent symbology
mixing introduces calendar spread prices (ZNH3-ZNM3 ≈ −0.63) as bar lows
in outright OHLCV data. After correction, bar ranges align with
independently sourced Rithmic historical bars.*

.. figure:: images/data_quality.png
   :alt: Data Quality Comparison

   Data Quality Comparison

Data Availability
-----------------

Futures market data used in this project was obtained from Databento
under a commercial data license and is therefore not included in this
repository. The notebook includes saved outputs from the completed
analysis so the reported results can be inspected without access to the
underlying proprietary data. Users with access to the required datasets
can rerun the analysis after configuring their local data paths and API
credentials.

Monte Carlo Results
-------------------

*10,000 Student’s-t simulations under Apex 150K EOD trailing drawdown
rules. Regime filtering materially improves pass rates and expected net
across all tested configurations.*

Monte Carlo Summary
~~~~~~~~~~~~~~~~~~~

+--------+------------+--------+------------+----------+--------+-------+--------+---------+------------+------+
| Config | Filters    | IS     | Expectancy | Positive | Trades | Pass  | Funded | Avg Net | Profitable | t df |
|        |            | Trades |            | Rate     | /      | Rate  | Bust   | /       | Attempts   |      |
|        |            |        |            |          | Month  |       | Rate   | Attempt |            |      |
+========+============+========+============+==========+========+=======+========+=========+============+======+
| 2.0 /  | Unfiltered | 2086   | $21        | 63.9%    | 80.2   | 36.6% | 100.0% | $1,650  | 21.4%      | 7.7  |
| 1.0 /  |            |        |            |          |        |       |        |         |            |      |
| 4.0    |            |        |            |          |        |       |        |         |            |      |
+--------+------------+--------+------------+----------+--------+-------+--------+---------+------------+------+
| 2.0 /  | Filtered   | 588    | $42        | 67.2%    | 24.5   | 55.5% | 99.4%  | $5,056  | 40.3%      | 8.1  |
| 1.0 /  |            |        |            |          |        |       |        |         |            |      |
| 4.0    |            |        |            |          |        |       |        |         |            |      |
+--------+------------+--------+------------+----------+--------+-------+--------+---------+------------+------+
| 2.5 /  | Unfiltered | 1385   | $14        | 50.8%    | 53.3   | 15.5% | 100.0% | $122    | 5.6%       | 25.1 |
| 1.0 /  |            |        |            |          |        |       |        |         |            |      |
| 4.0    |            |        |            |          |        |       |        |         |            |      |
+--------+------------+--------+------------+----------+--------+-------+--------+---------+------------+------+
| 2.5 /  | Filtered   | 375    | $41        | 54.1%    | 15.6   | 24.9% | 100.0% | $530    | 11.7%      | 23.8 |
| 1.0 /  |            |        |            |          |        |       |        |         |            |      |
| 4.0    |            |        |            |          |        |       |        |         |            |      |
+--------+------------+--------+------------+----------+--------+-------+--------+---------+------------+------+
| 3.0 /  | Unfiltered | 639    | $51        | 56.5%    | 24.6   | 23.6% | 100.0% | $461    | 10.9%      | 15.1 |
| 1.0 /  |            |        |            |          |        |       |        |         |            |      |
| 5.5    |            |        |            |          |        |       |        |         |            |      |
+--------+------------+--------+------------+----------+--------+-------+--------+---------+------------+------+
| 3.0 /  | Filtered   | 164    | $90        | 58.5%    | 6.8    | 31.1% | 98.7%  | $978    | 16.0%      | 18.1 |
| 1.0 /  |            |        |            |          |        |       |        |         |            |      |
| 5.5    |            |        |            |          |        |       |        |         |            |      |
+--------+------------+--------+------------+----------+--------+-------+--------+---------+------------+------+
| 2.5 /  | Unfiltered | 1177   | $31        | 61.7%    | 45.3   | 27.2% | 100.0% | $665    | 12.8%      | 11.0 |
| 1.0 /  |            |        |            |          |        |       |        |         |            |      |
| 5.0    |            |        |            |          |        |       |        |         |            |      |
+--------+------------+--------+------------+----------+--------+-------+--------+---------+------------+------+
| 2.5 /  | Filtered   | 318    | $70        | 66.0%    | 13.2   | 47.1% | 99.1%  | $3,022  | 30.6%      | 8.4  |
| 1.0 /  |            |        |            |          |        |       |        |         |            |      |
| 5.0    |            |        |            |          |        |       |        |         |            |      |
+--------+------------+--------+------------+----------+--------+-------+--------+---------+------------+------+

Repository Contents
-------------------

+---------------------------------------+------------------------------------------------+
| File                                  | Description                                    |
+=======================================+================================================+
| ``vwap_mean_reversion_futures.ipynb`` | Complete research notebook (15 sections)       |
+---------------------------------------+------------------------------------------------+
| ``README.md``                         | This file                                      |
+---------------------------------------+------------------------------------------------+
| ``images/``                           | Charts and figures referenced in this README   |
+---------------------------------------+------------------------------------------------+

*Raw Databento DBN files and derived market datasets are not distributed
with this repository because the underlying data requires a commercial
subscription.*

Research Workflow
-----------------

======= ==============================================
Section Content
======= ==============================================
1       Imports & constants
2       Data acquisition & quality validation
3       VWAP signal construction
4       AR(1) / Ornstein-Uhlenbeck diagnostics
5       Mean-reversion & VWAP drift decomposition
6       Walk-forward IS/OOS design
7       Backtest engine
8       Parameter & regime-filter comparison
9       Monte Carlo stress testing
10      Empirical Kelly position sizing
11      Contract-size optimization & historical replay
12      6E cross-market validation
13      Standalone strategy comparison
14      Calendar spread extension (theoretical)
15      Conclusions
======= ==============================================

Data Engineering
----------------

One of the most significant findings during the project was **systematic
contamination in commercial futures tick data**.

Using Databento parent symbology (``stype_in="parent"``), calendar
spreads and back-month contracts are silently mixed with outright
futures trades during aggregation. These prices are economically valid
for spread instruments but produce impossible OHLC values when combined
with outright bars:

ZN outright price ≈ 115.00 ZNH3-ZNM3 spread ≈ −0.63 ← becomes bar low
after aggregation

A single spread trade inside a one-minute window incorrectly became the
bar low, inflating apparent intrabar ranges by orders of magnitude.
Cross-validation against independently sourced Rithmic server-side bars
confirmed the issue — Rithmic showed a median 1-minute bar range of 2
ticks; raw parent-symbology data showed a mean range of ~10 ticks.

**Solution**: reconstruct the dataset by filtering every daily DBN file
to the active front-month contract using a quarterly roll calendar.
After correction: - Zero impossible prices - Median bar ranges
consistent with Rithmic ground truth - Backtest performance materially
more realistic than pre-correction results

Statistical Findings
--------------------

Mean Reversion Signal
~~~~~~~~~~~~~~~~~~~~~

VWAP deviations exhibit statistically significant mean reversion in both
instruments:

================================ ============ ========
Metric                           ZN           6E
================================ ============ ========
Intraday AR(1) half-life         **19.2 min** 38.0 min
Intraday AR(1) R²                **0.932**    0.963
2.5σ → 1.0σ reversion rate (4hr) **80.7%**    79.0%
Median time to reversion         **46 min**   45 min
================================ ============ ========

VWAP Drift Decomposition
~~~~~~~~~~~~~~~~~~~~~~~~

Rather than assuming price alone returns to VWAP, the project decomposes
observed z-score reversion into two independent components:

1. **Price movement toward VWAP**
2. **VWAP movement toward price** — as new trades enter the cumulative
   volume-weighted average

| For ZN reversions: price accounts for ~\ **54%**, VWAP drift
  ~\ **46%**.
| For 6E reversions: price accounts for ~\ **49%**, VWAP drift
  ~\ **51%**.

This decomposition is critical for exit design. A fixed-price target set
at entry ignores VWAP drift entirely. A **live z-score exit** — closing
when the current z-score reaches the target level using the current VWAP
and std — captures both components and materially outperforms
fixed-price alternatives.

Strategy Design
---------------

Signal
~~~~~~

- **Entry**: \|z-score\| crosses entry threshold between consecutive
  bars, with \|z-score\| < stop threshold (ensures valid stop geometry)
- **Exit**: live z-score returns to exit threshold using current
  VWAP/std
- **Stop**: fixed price level set at entry-time VWAP ± stop-threshold ×
  std

Regime Filters
~~~~~~~~~~~~~~

+------------------+-------------------------+-------------------------+
| Filter           | Threshold               | Rationale               |
+==================+=========================+=========================+
| ATR percentile   | < 85th pct (60-day      | Suppress entries on     |
|                  | window)                 | unusually volatile days |
+------------------+-------------------------+-------------------------+
| Prior-session    | < 3 bps                 | Rate shock days break   |
| yield move       |                         | mean reversion          |
+------------------+-------------------------+-------------------------+
| Volume ratio     | < 3× 20-bar trailing    | Avoid news-spike bars   |
|                  | average                 |                         |
+------------------+-------------------------+-------------------------+
| Warm-up bars     | ≥ 60 bars (1 hour)      | Require sufficient      |
|                  |                         | history for stable      |
|                  |                         | VWAP/std                |
+------------------+-------------------------+-------------------------+

Filters are applied using **only information available at session open**
— no look-ahead bias.

Validation
----------

The project intentionally retains intermediate analyses—including
unsuccessful parameterizations and a negative cross-market result—to
emphasize reproducibility and reduce selection bias. Multiple
independent techniques were used to evaluate robustness:

- **Walk-forward IS/OOS testing** — 13 interleaved holdout months across
  the full date range
- **Parameter sweep** — four entry/exit/stop configurations with and
  without regime filters
- **Monte Carlo simulation** — 10,000 paths using Student’s-t
  distribution (fat-tail aware)
- **Historical path replay** — actual trade sequence replayed against
  modeled prop-firm rules
- **Standalone account comparison** — Sharpe, Sortino, profit factor
  across configurations
- **Cross-market validation** — same methodology applied to 6E as a
  robustness test
- **Live deployment** — real fills via Rithmic API confirming executable
  strategy behavior

Live Deployment
---------------

The strategy was implemented as a fully automated trading system using
the Rithmic API (``async_rithmic``). The system included:

- **Asynchronous order execution** with bracket management (stop +
  target)
- **Real-time VWAP computation** — daily reset at midnight UTC,
  expanding std
- **Session-aware regime filtering** — ATR check and yield filter at
  session open
- **Telegram trade notifications** — entry, exit, and session summary
  alerts
- **Fault-tolerant reconnect logic** — exponential backoff with state
  preservation
- **Fill notification handling** — distinguishing entry fills from
  bracket exits

**Live deployment provided an execution-layer test of the broader
research framework**, confirming real-time data handling, order
submission, fills, bracket management, and operational reliability while
exposing several execution-specific issues that were subsequently
corrected. The deployed implementation was an earlier 6E version of the
strategy and did not incorporate all features of the final research
specification.

Calendar Spread Extension
-------------------------

Although electronic calendar spreads in Treasury and FX futures proved
too illiquid for practical backtesting (~18 trades/day for ZNH3-ZNM3),
the methodology appears well suited for **commodity calendar spreads**.

Markets such as crude oil (CL) and natural gas (NG) have substantially
more liquid calendar-spread markets, making them more suitable
candidates for extending this methodology. Calendar spreads also offer a
more interpretable mean-reversion anchor than intraday VWAP alone: their
relative pricing is linked to the economics of carrying inventory across
time, including storage costs, financing rates, and convenience yield.
This makes spread behavior particularly relevant to physical commodity
trading, where the shape and movement of the forward curve are central
to both risk management and trading decisions.

Technologies
------------

``Databento``  ·  ``FRED API``  ·  ``Matplotlib``  ·  ``NumPy``  · 
``Pandas``  ·  ``Python``  ·  ``Rithmic API (async_rithmic)``  · 
``SciPy``  ·  ``Statsmodels``

Author
------

| **Ein Clark**
| Independent Quantitative Research · 2024–2026
| B.S. Economics — Minors: Mathematics, Physics, Finance
| West Virginia University · 2023

