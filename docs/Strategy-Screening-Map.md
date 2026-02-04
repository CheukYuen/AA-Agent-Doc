# Strategy Screening Map for 29 Asset-Allocation Strategies Using a 32-Indicator Macro Set

## What “strategy screening” means in your pipeline

Your pipeline separates three different decisions that are often conflated in asset-allocation systems:

Macro data → feature engineering → macro state recognition (LLM) → strategy pool screening (rules + LLM)

“Strategy pool screening” is best treated as **a gating and prioritization layer**, not the layer that produces final weights. In practice, screening answers questions like:

- **Is this strategy appropriate to run now?** (eligible / ineligible)
- **How aggressively should it run?** (risk-on sizing vs. risk-off sizing)
- **Should it be the “main engine” or only a satellite?** (priority ranking)

This distinction matters because several of your 29 strategies are **portfolio construction frameworks** whose true inputs are market return moments (expected returns, volatilities, correlations) rather than macro series (e.g., Markowitz mean–variance optimization, risk parity, dynamic risk budgeting). The macro set is still useful, but mainly to **set constraints, choose priors/views, and apply risk overrides**. Mean–variance optimization is explicitly built around expected return and covariance inputs, not macro indicators. citeturn0search8turn0search13

## Research-backed drivers that link strategy choice to macro and financial conditions

A defensible screening map should anchor each strategy to **the economic/financial variable(s) it is known to be sensitive to**, then match those variables to your 32 indicators.

**Optimization frameworks (MVO, Black–Litterman).** Markowitz-style mean–variance allocation is driven by the joint distribution of returns (means, variances, covariances). citeturn0search8 Black–Litterman is commonly described as a way to combine an equilibrium baseline (reverse optimization) with subjective “views” to improve robustness and interpretability of MVO outputs. citeturn0search13turn0search1 In a macro-driven stack, these “views” are naturally sourced from growth/inflation/liquidity regimes, but the optimizer still ultimately consumes return/covariance assumptions.

**Risk budgeting / risk parity / All Weather.** Risk budgeting decomposes portfolio risk into contributions; risk parity is a particular case focused on equalizing (or balancing) those contributions. citeturn0search14 Bridgewater’s All Weather is explicitly designed around performance across different economic environments (commonly framed around growth and inflation regimes). citeturn0search3turn9search6 The key macro states for screening these strategies therefore center on **growth, inflation, and the financing environment** (because leverage and duration exposures behave very differently if inflation and rates are rising). citeturn9search5

**Trend / momentum and volatility scaling.** Time-series momentum (“trend following” in a broad sense) is a documented empirical regularity across futures on equities, bonds, currencies, and commodities: past 1–12 month returns predict near-term direction in many samples. citeturn1search0turn1search4 Volatility-managed/volatility-targeting approaches reduce exposure when volatility is high; academic evidence shows volatility timing can materially change Sharpe ratios by reducing risk when volatility spikes. citeturn1search9turn9search0 Practitioner-focused risk research also flags that risk parity, volatility-targeting, and trend strategies can be forced to de-lever in rising-volatility episodes—supporting the idea that an “implied vol / stress” indicator is a first-class screening variable. citeturn9search4

**Carry and spread strategies.** “Carry” has been formalized as an ex-ante return component across asset classes (not just FX), and the carry premium is often associated with exposure to recession/liquidity/volatility risks—exactly the kinds of risks your credit and risk-sentiment block is trying to capture. citeturn2search1

**Financial conditions and financial stress.** Financial Conditions Indices (FCIs) are widely used to summarize the availability and cost of financing, typically from a mix of rates, spreads, prices, and volatility measures; they are monitored by central banks and institutions because they relate to future risks to activity and inflation. citeturn7search11turn7search7turn1search2 Financial stress indices similarly combine categories like funding, credit, valuation, safe assets, and volatility—very close to your `DR007`, spreads, `ERP_EP_10Y`, and `IV_OPTION_INDEX` set. citeturn3search2

**Credit spreads as a macro-financial warning signal.** Academic and policy research has shown that corporate credit spreads (and related “excess bond premium” components) contain information about credit supply shocks and subsequent economic activity and asset prices. citeturn3search5turn3search6 This supports putting `CREDIT_SPREAD_AAA_10Y` near the center of “stress-triggered de-risking” and “credit-cycle rotation.”

**Equity risk premium / earnings yield vs. bond yields.** The equity risk premium is a core risk-price variable in asset allocation. citeturn3search8 The practical “earnings yield minus 10-year yield” style measure has a long history in valuation discussions (often referred to in the context of “Fed model” debates), and AQR has also analyzed the spread between stock yields and bond yields when thinking about ERP-related signals. citeturn2search12turn3search3 That provides a research rationale for your `ERP_EP_10Y` as a primary screening variable for equity-vs-bond switching logic.

**Gold as inflation/tail hedge and diversifier.** Gold’s role as a long-run inflation hedge is supported by institutional research, with caveats about short-horizon unreliability. citeturn5search0 It is also commonly studied as a diversifier and potential tail-risk hedge component. citeturn5search15 This supports mapping gold (your `XAUUSD_AVG_MONTHLY`) to “inflation protection” and “tail hedge” screening.

## From 32 raw indicators to stable macro states used for screening

A practical screening system usually works on **a small number of engineered “states”** rather than raw series. The goal is to give the LLM (and the rules layer) a stable, explainable state vector that changes slowly and avoids overreacting to noisy releases.

Below is a common, defensible mapping from your 32 indicators into **screening states** (you can implement these as z-scores, rolling percentiles, and/or 3–12 month momentum of each component):

- **Growth state (G):** the 8 Growth indicators collectively. PMI is widely treated as a leading indicator of activity, and official NBS releases provide timely diffusion measures. citeturn7search6turn7search14  
  Inputs: `AUTO_RETAIL_SALES_YOY`, `INDUSTRY_YOY`, `INVESTMENT_FA_YOY`, `PMI_MANU`, `PMI_SERV`, `POWER_GEN_YOY`, `REAL_GDP_QOQ`, `RETAIL_SALES_YOY`.

- **Inflation state (π):** inflation pressure level and direction.  
  Inputs: `CPI_YOY`, `PPI_YOY`.

- **Liquidity/Credit state (L/C):** money growth and credit supply vs. funding cost. For China-specific “credit impulse” style thinking, broad credit and money aggregates are typically tracked alongside short rates; TSF is widely cited as a broad credit concept in China. citeturn4search12turn3search5  
  Inputs: `M2_YOY`, `M1_YOY`, `M1_M2_GAP`, `TSF_STOCK_YOY`, `DR007`.

- **Rates and term state (R):** level of rates and (approximate) curve pressure. With only a 10Y yield and short funding proxies, a workable “term spread proxy” is `(YIELD_10Y − DR007)` or `(YIELD_10Y − LPR_1Y)`. Yield-curve slope is widely used as a recession-probability predictor in many frameworks (even though your exact slope proxy differs). citeturn7search1turn7search5  
  Inputs: `LPR_1Y`, `YIELD_10Y`, plus derived spreads vs `DR007`.

- **Risk premia state (RP):** equity and credit risk pricing. Credit spreads and ERP-style measures both reflect changing risk appetite / compensation for risk. citeturn3search5turn3search8turn3search3  
  Inputs: `CREDIT_SPREAD_AAA_10Y`, `ERP_EP_10Y`.

- **Financial conditions / stress state (FCI/FSI):** composite of funding, spreads, valuation, FX stress, and volatility. Institutional definitions explicitly frame FCIs as availability and affordability of financing; stress indices similarly require a volatility component. citeturn7search11turn3search2turn1search6  
  Inputs: `DR007`, `LPR_1Y`, `YIELD_10Y`, `CREDIT_SPREAD_AAA_10Y`, `ERP_EP_10Y`, `FX_CNY_MID_MONTHLY`, `IV_OPTION_INDEX`.

- **External demand / trade state (X):** export/import growth (demand and cycle).  
  Inputs: `EXPORT_CUMULATIVE_YOY`, `IMPORT_CUMULATIVE_YOY`, `TRADE_TOTAL_CUMULATIVE_YOY`.

- **FX stress / hedging state (FX/Gold):** managed FX regimes still transmit macro stress; gold is often treated as an inflation/tail hedge input. citeturn5search0turn5search15  
  Inputs: `FX_CNY_MID_MONTHLY`, `XAUUSD_AVG_MONTHLY`.

- **Domestic demand drag state (Property/Labor):** property and labor-market slack, mainly to refine China cycle classification and “policy easing likelihood.”  
  Inputs: `RESID_HOUSE_AREA_CUMULATIVE_YOY`, `RESID_HOUSE_SALES_CUMULATIVE_YOY`, `URBAN_SURVEY_UNEMPLOYMENT`, `URBAN_YOUTH_UNEMPLOYMENT`.

- **Investment confidence state (I):** investment momentum split (public-ish vs private).  
  Inputs: `INVESTMENT_FA_PRIVATE_YOY`, `INVESTMENT_FA_YOY`.

- **Risk sentiment state (V):** implied volatility as forward-looking risk/uncertainty proxy; implied volatility indices are designed to reflect market-expected near-term volatility from option prices. citeturn8search1turn8search2  
  Input: `IV_OPTION_INDEX`.

These engineered states are what your LLM should “name” (e.g., “Growth improving”, “Liquidity tightening”, “Stress elevated”) and what your strategy screener should consume.

## Strategy-to-indicator mapping table

Interpretation: each row lists the **smallest practical set** of your indicators that should drive (a) whether the strategy is eligible, and (b) how aggressively it should be prioritized. When a strategy’s true signal requires market data not in your macro set (e.g., price momentum, portfolio drawdown), the table lists the macro indicators that best support **gating/overrides**, not the entire strategy.

| Strategy (your 29) | Macro indicators that decide screening (from your 32) |
|---|---|
| 60/40 equity–bond (banded constraints) | `ERP_EP_10Y`, `YIELD_10Y`, `CREDIT_SPREAD_AAA_10Y`, `IV_OPTION_INDEX`, `DR007` |
| Mean–variance optimization (MVO) | Macro gating for parameter stability/risk: `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `FX_CNY_MID_MONTHLY` (core optimizer still uses μ/Σ) |
| Black–Litterman | Macro “views” inputs: Growth block (8 Growth indicators), `CPI_YOY`, `PPI_YOY`, `TSF_STOCK_YOY`, `M1_M2_GAP`, `DR007`, `YIELD_10Y`, `ERP_EP_10Y`, `CREDIT_SPREAD_AAA_10Y` |
| Risk budgeting framework (SAA) | Screening for risk regime shifts: `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `YIELD_10Y`, `CPI_YOY` |
| Risk parity (equal risk contribution special case) | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `CPI_YOY`, `YIELD_10Y` (inflation/rates stress matters for duration risk) |
| All Weather | Growth block (8), inflation (`CPI_YOY`, `PPI_YOY`), plus stress/leverage constraints: `IV_OPTION_INDEX`, `DR007`, `YIELD_10Y` |
| Factor allocation (long-term tilts) | Regime context: Growth block (8), inflation (2), liquidity/credit (5), `ERP_EP_10Y` (factor returns themselves require extra data) |
| Target-date fund (TDF glidepath) | Mostly life-cycle driven; macro overlay for de-risking: `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007` |
| Core–satellite (core book) | Satellite enable/disable and risk budget: `IV_OPTION_INDEX`, `ERP_EP_10Y`, `CREDIT_SPREAD_AAA_10Y`, `DR007` |
| Macro cycle rotation | Growth block (8), inflation (2), liquidity/credit (5), rates ( `YIELD_10Y`, `LPR_1Y` ), risk premia ( `ERP_EP_10Y`, `CREDIT_SPREAD_AAA_10Y` ) |
| Policy cycle rotation | Funding + credit + fiscal impulse: `DR007`, `TSF_STOCK_YOY`, `M1_M2_GAP`, `LPR_1Y`, `YIELD_10Y`, `GOV_EXPEND_CUMULATIVE_YOY` (policy text/NLP is extra) |
| Credit cycle rotation | `CREDIT_SPREAD_AAA_10Y`, `TSF_STOCK_YOY`, `DR007`, `M1_M2_GAP`, `YIELD_10Y` |
| Momentum & trend (6–12M time-series) | Macro gating (trend signal needs prices): `IV_OPTION_INDEX`, `DR007`, `FX_CNY_MID_MONTHLY`, `CREDIT_SPREAD_AAA_10Y` |
| Valuation percentile adjustment | `ERP_EP_10Y`, `YIELD_10Y`, `CPI_YOY` (true valuation percentiles need additional valuation data beyond ERP proxy) |
| Equity–bond relative value (ERP switch) | `ERP_EP_10Y`, `YIELD_10Y`, `CPI_YOY` (real-rate context) |
| Contrarian / mean reversion under extremes | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `FX_CNY_MID_MONTHLY` |
| Factor timing | Growth block (8), inflation (2), liquidity/credit: `TSF_STOCK_YOY`, `M1_M2_GAP`, `DR007`, plus `IV_OPTION_INDEX` (factor P&L inputs are extra) |
| Financial Conditions Index (FCI) strategy | `DR007`, `LPR_1Y`, `YIELD_10Y`, `CREDIT_SPREAD_AAA_10Y`, `ERP_EP_10Y`, `FX_CNY_MID_MONTHLY`, `IV_OPTION_INDEX` |
| GTAA (global tactical asset allocation) | With your current domestic-heavy macro set: `FX_CNY_MID_MONTHLY`, `XAUUSD_AVG_MONTHLY`, `EXPORT_CUMULATIVE_YOY`, `IMPORT_CUMULATIVE_YOY`, `YIELD_10Y`, `IV_OPTION_INDEX` (true GTAA benefits from global macro series) |
| Carry / spread strategies | `YIELD_10Y`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `CPI_YOY` (carry premia are exposed to recession/liquidity/vol risks) |
| FX overlay | `FX_CNY_MID_MONTHLY`, `DR007`, `YIELD_10Y`, `IV_OPTION_INDEX`, `TRADE_TOTAL_CUMULATIVE_YOY` |
| Volatility targeting allocation | `IV_OPTION_INDEX`, `DR007`, `CREDIT_SPREAD_AAA_10Y` (portfolio vol uses return data; macro mainly gates) |
| Dynamic risk budgeting | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `ERP_EP_10Y` |
| CPPI / OBPI (portfolio insurance) | Stress + funding inputs: `IV_OPTION_INDEX`, `DR007`, `CREDIT_SPREAD_AAA_10Y`, `YIELD_10Y` (CPPI/OBPI mechanics depend on portfolio value path and/or options) |
| Maximum drawdown control | Macro early-warning + crisis filters: `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `FX_CNY_MID_MONTHLY` (drawdown metric comes from portfolio/price series) |
| Tail-risk hedging | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `XAUUSD_AVG_MONTHLY`, `FX_CNY_MID_MONTHLY` |
| Financial stress trigger de-risking | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `ERP_EP_10Y`, `FX_CNY_MID_MONTHLY` (stress indices conceptually mix these categories) |
| Liquidity-aware allocation | `DR007`, `M1_YOY`, `M2_YOY`, `M1_M2_GAP`, `TSF_STOCK_YOY`, `CREDIT_SPREAD_AAA_10Y` |
| Macro × trend × risk budgeting (integrated) | Macro regime: Growth block (8) + inflation (2) + credit/liquidity (5); execution risk: `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`; external shock: `FX_CNY_MID_MONTHLY` |

## Practical notes that make the screening robust in production

Your 32-indicator set is built from official and market sources that are released at different frequencies and lags, including the entity["organization","National Bureau of Statistics of China","beijing, china"] and the entity["organization","People's Bank of China","beijing, china"] for core macro and credit aggregates, and the entity["organization","London Bullion Market Association","london, uk"] for gold pricing. citeturn7search6turn4search12turn5search0 A screening layer breaks quickly if it ignores release timing, revisions, and “staleness,” so three practices are consistently useful:

First, **separate “level” from “direction.”** Many regimes flip not when a metric is high/low, but when it is accelerating/decelerating. This is especially relevant for growth and credit, where yoy series can remain weak even as momentum improves.

Second, **promote a small set of “stress/finance” indicators to first-class gating variables.** The literature on FCIs and stress indices emphasizes that spreads and volatility contain incremental information about financing tightness and risk appetite beyond slow-moving macro. citeturn7search11turn3search2turn3search5 In your set, that means `CREDIT_SPREAD_AAA_10Y`, `DR007`, and `IV_OPTION_INDEX` should almost always appear in risk-control screening, and they should also cap aggressiveness for trend, carry, and leveraged risk-parity style allocations. citeturn9search4turn2search1turn1search9

Third, **treat “FCI” as a composite, not a single variable.** The BIS and IMF describe FCIs as summaries over multiple financial prices and spreads; your indicator list already contains a compact, China-relevant subset (rates, spreads, FX, volatility). citeturn7search11turn7search7turn1search6 In practice, you will get more stable screening by building a composite “tightening score” than by using any single component alone.

## Limitations and what you may need to add later

Some of your strategies cannot be screened “purely macro” without losing their defining signal:

- **Trend/momentum, drawdown control, volatility targeting, CPPI/OBPI**: these need portfolio/asset price paths and/or option surfaces; macro indicators are best used for risk overlays rather than as the primary signal. citeturn1search4turn6search12turn1search11  
- **Valuation percentile adjustment and factor timing**: your `ERP_EP_10Y` is a strong start (it captures an earnings-yield vs. bond-yield spread concept), but true valuation percentile frameworks usually require explicit equity valuation series (index earnings yield, PE/PB, profit margins) and factor return histories. citeturn3search3turn0search13  
- **GTAA**: a robust GTAA screen usually benefits from global growth/inflation/rates and cross-country valuation/rate differentials; with only domestic macro and a limited FX/gold view, your screening logic should be treated as partial. citeturn9search10turn9search6

Even with these limitations, your current 32-indicator macro set is sufficient to implement a production-quality screening layer if you treat it as: **macro regime identification + financial conditions/stress gating + strategy prioritization**, with true alpha/position sizing left to the strategy engines that consume market price data. citeturn7search11turn1search9turn2search1