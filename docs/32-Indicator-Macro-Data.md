# Full 32-Indicator Macro Data CSV Template With Populated Sample Values

## Objective and assumptions

You asked for the **complete “32 indicators” output in the same row-wise CSV schema** you showed (`indicator_id, ref_month, value, unit, adjustment, source, data_type`), with **real values copied from your pasted sample where available**, and **mock values for the indicators that were not present** in your sample.

Two implementation assumptions are embedded in your example and preserved here:

The `ref_month` field is a **month key formatted as `YYYY-MM`**, even for series that are naturally **daily** (e.g., repo rates) or **quarterly** (GDP). The GDP example you provided already uses `2024-12`, `2025-03`, etc. as the quarter-end month. This aligns with how NBS describes its **seasonally adjusted quarter-on-quarter GDP growth** in its GDP releases. citeturn1search6

Your “source” abbreviations correspond to specific publishers/benchmark administrators (defined below), including the entities that publish **LPR**, **repo rates**, **yield curves**, and **gold benchmark prices**. citeturn2search0turn0search2turn2search1turn0search3

Throughout the CSVs below:
- **Values are “EXTRACT”** when they come directly from the dataset snippet you pasted.
- **Values are “MOCK”** when your snippet did not include that indicator (you can replace later with live data without changing schema).

## Schema specification and formatting conventions

This schema is consistent with your sample blocks and is suitable for both storage and downstream feature engineering:

- `indicator_id`: stable string identifier for the time series (e.g., `CPI_YOY`).
- `ref_month`: `YYYY-MM` month key.
  - For **quarterly** series (e.g., `REAL_GDP_QOQ`), use the **quarter-end month** (`03/06/09/12`) as in your example. citeturn1search6
  - For **daily** series (e.g., `DR007`), store a **monthly aggregated value** (commonly month average or month-end) under that month key; keep the raw daily store separately if needed. The underlying rate itself is quoted daily in interbank markets and is published via ChinaMoney/CFETS channels. citeturn0search2turn0search10
- `value`: numeric; use decimals for yields/rates and consistent precision per indicator family.
- `unit`: string; blank allowed (your PMI example uses empty unit).
- `adjustment`: string; examples include `YoY`, `Level`, `QoQ_SA`, `Spread(Level)`, `Spread(YoY)`.
- `source`: short code string (e.g., `NBS`, `PBOC`), with a publisher mapping defined in the next section.
- `data_type`: module label used for grouping (e.g., `Growth`, `Inflation`, `Credit`).

## Source abbreviation map and what each source typically publishes

The “source” column in your CSV rows is best treated as a controlled vocabulary tied to data licensors/publishers:

- `NBS` = entity["organization","National Bureau of Statistics of China","china government statistics"] (monthly macro indicators such as retail sales releases, PMI releases, and quarterly GDP notes). citeturn1search4turn1search9turn1search6  
- `PBOC` = entity["organization","People's Bank of China","central bank of china"] (monthly money supply such as M2/M1 YoY in financial statistics reports; and aggregate financing / social financing related releases). citeturn0search0turn0search9  
- `CFETS/ChinaMoney` = entity["organization","China Foreign Exchange Trade System","china interbank market infra"] / entity["organization","ChinaMoney","cfets market data portal"] (interbank market benchmarks such as pledged repo rates like DR007; and benchmark pages for LPR). citeturn0search2turn2search0turn0search10  
- `NIFC` = entity["organization","National Interbank Funding Center","china interbank benchmark admin"] (administrator/calculator of the Loan Prime Rate as described on the LPR benchmark page). citeturn2search0  
- `CHINA_BOND` = entity["organization","ChinaBond","china bond yield curve service"] (yield curves and historical yields, including government bond yield curve data and methodology). citeturn2search1turn2search5turn2search9  
- `LBMA` = entity["organization","London Bullion Market Association","precious metals trade group"] (LBMA Gold Price benchmark; administered by entity["company","ICE Benchmark Administration","benchmark administrator"] as described by LBMA). citeturn0search3  
- `MOF` = entity["organization","Ministry of Finance of the People's Republic of China","china finance ministry"] (fiscal expenditure statistics series, per your design).
- `SSE/SZSE/Wind` = entity["organization","Shanghai Stock Exchange","china stock exchange"] / entity["organization","Shenzhen Stock Exchange","china stock exchange"] / entity["company","Wind Information Co., Ltd.","china financial data vendor"] (option-implied volatility series; academic literature commonly references the China volatility index/iVX being released by SSE and derived from ETF options). citeturn3search22turn3search6  
- `CSI/Wind+CHINA_BOND` = entity["company","China Securities Index Co., Ltd.","china index provider"] + Wind + ChinaBond (to compute ERP-like spreads such as `E/P - 10Y`, per your internal definition; the spread itself is a derived metric).

## Complete CSV examples for all modules and all 32 indicator entries

Report generation time: **2026-02-04** (Asia/Taipei)  
Data time range shown in samples: **mostly 2025-09 to 2026-02** (EXTRACT where available; otherwise MOCK)

### Module Growth_Indicators (8 entries)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
AUTO_RETAIL_SALES_YOY,2025-09,1.6,%,YoY,NBS,Growth
AUTO_RETAIL_SALES_YOY,2025-10,-6.6,%,YoY,NBS,Growth
AUTO_RETAIL_SALES_YOY,2025-11,-8.3,%,YoY,NBS,Growth
INDUSTRY_YOY,2025-09,6.5,%,YoY,NBS,Growth
INDUSTRY_YOY,2025-10,4.9,%,YoY,NBS,Growth
INDUSTRY_YOY,2025-11,4.8,%,YoY,NBS,Growth
INVESTMENT_FA_YOY,2025-09,-0.5,%,YoY,NBS,Growth
INVESTMENT_FA_YOY,2025-10,-1.7,%,YoY,NBS,Growth
INVESTMENT_FA_YOY,2025-11,-2.6,%,YoY,NBS,Growth
PMI_MANU,2025-10,49.0,,Level,NBS,Growth
PMI_MANU,2025-11,49.2,,Level,NBS,Growth
PMI_MANU,2025-12,50.1,,Level,NBS,Growth
PMI_SERV,2025-10,50.2,,Level,NBS,Growth
PMI_SERV,2025-11,49.5,,Level,NBS,Growth
PMI_SERV,2025-12,49.7,,Level,NBS,Growth
POWER_GEN_YOY,2025-09,1.5,%,YoY,NBS,Growth
POWER_GEN_YOY,2025-10,7.9,%,YoY,NBS,Growth
POWER_GEN_YOY,2025-11,2.7,%,YoY,NBS,Growth
REAL_GDP_QOQ,2025-03,1.2,%,QoQ_SA,NBS,Growth
REAL_GDP_QOQ,2025-06,1.0,%,QoQ_SA,NBS,Growth
REAL_GDP_QOQ,2025-09,1.1,%,QoQ_SA,NBS,Growth
RETAIL_SALES_YOY,2025-09,3.0,%,YoY,NBS,Growth
RETAIL_SALES_YOY,2025-10,2.9,%,YoY,NBS,Growth
RETAIL_SALES_YOY,2025-11,1.3,%,YoY,NBS,Growth
```

### Module Inflation_Indicators (2 entries)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
CPI_YOY,2025-09,-0.1,%,YoY,NBS,Inflation
CPI_YOY,2025-10,-0.1,%,YoY,NBS,Inflation
CPI_YOY,2025-11,0.0,%,YoY,NBS,Inflation
PPI_YOY,2025-09,-2.3,%,YoY,NBS,Inflation
PPI_YOY,2025-10,-2.1,%,YoY,NBS,Inflation
PPI_YOY,2025-11,-2.2,%,YoY,NBS,Inflation
```

### Module Monetary_Credit_Environment (5 entries)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
M2_YOY,2025-09,8.4,%,YoY,PBOC,Credit
M2_YOY,2025-10,8.2,%,YoY,PBOC,Credit
M2_YOY,2025-11,8.0,%,YoY,PBOC,Credit
M1_YOY,2025-09,2.1,%,YoY,PBOC,Credit
M1_YOY,2025-10,2.4,%,YoY,PBOC,Credit
M1_YOY,2025-11,2.2,%,YoY,PBOC,Credit
M1_M2_GAP,2025-09,-6.3,%,Spread(YoY),PBOC,Credit
M1_M2_GAP,2025-10,-5.8,%,Spread(YoY),PBOC,Credit
M1_M2_GAP,2025-11,-5.8,%,Spread(YoY),PBOC,Credit
TSF_STOCK_YOY,2025-09,8.7,%,YoY,PBOC,Credit
TSF_STOCK_YOY,2025-10,8.5,%,YoY,PBOC,Credit
TSF_STOCK_YOY,2025-11,8.5,%,YoY,PBOC,Credit
DR007,2025-11,1.95,%,Level,CFETS/ChinaMoney,Credit
DR007,2025-12,2.05,%,Level,CFETS/ChinaMoney,Credit
DR007,2026-01,2.10,%,Level,CFETS/ChinaMoney,Credit
```

### Module Interest_Rate_Indicators (4 entries)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
LPR_1Y,2025-10,3.0,%,Level,PBOC,Rate
LPR_1Y,2025-11,3.0,%,Level,PBOC,Rate
LPR_1Y,2025-12,3.0,%,Level,PBOC,Rate
YIELD_10Y,2025-11,1.817,%,Level,CHINA_BOND,Rate
YIELD_10Y,2025-12,1.844,%,Level,CHINA_BOND,Rate
YIELD_10Y,2026-01,1.861,%,Level,CHINA_BOND,Rate
CREDIT_SPREAD_AAA_10Y,2025-11,82,bp,Spread(Level),CHINA_BOND,Rate
CREDIT_SPREAD_AAA_10Y,2025-12,86,bp,Spread(Level),CHINA_BOND,Rate
CREDIT_SPREAD_AAA_10Y,2026-01,84,bp,Spread(Level),CHINA_BOND,Rate
ERP_EP_10Y,2025-11,4.2,%,Spread(Level),CSI/Wind+CHINA_BOND,Rate
ERP_EP_10Y,2025-12,4.0,%,Spread(Level),CSI/Wind+CHINA_BOND,Rate
ERP_EP_10Y,2026-01,4.1,%,Spread(Level),CSI/Wind+CHINA_BOND,Rate
```

### Module Fiscal_And_Government_Balance (1 entry)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
GOV_EXPEND_CUMULATIVE_YOY,2025-09,3.1,%,YoY,MOF,Fiscal
GOV_EXPEND_CUMULATIVE_YOY,2025-10,2.0,%,YoY,MOF,Fiscal
GOV_EXPEND_CUMULATIVE_YOY,2025-11,1.4,%,YoY,MOF,Fiscal
```

### Module Real_Estate_Market (2 entries)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
RESID_HOUSE_AREA_CUMULATIVE_YOY,2025-09,-5.6,%,YoY,NBS,Real_Estate
RESID_HOUSE_AREA_CUMULATIVE_YOY,2025-10,-7.0,%,YoY,NBS,Real_Estate
RESID_HOUSE_AREA_CUMULATIVE_YOY,2025-11,-8.1,%,YoY,NBS,Real_Estate
RESID_HOUSE_SALES_CUMULATIVE_YOY,2025-09,-7.6,%,YoY,NBS,Real_Estate
RESID_HOUSE_SALES_CUMULATIVE_YOY,2025-10,-9.4,%,YoY,NBS,Real_Estate
RESID_HOUSE_SALES_CUMULATIVE_YOY,2025-11,-11.2,%,YoY,NBS,Real_Estate
```

### Module External_Balance (3 entries)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
EXPORT_CUMULATIVE_YOY,2025-09,6.1,%,YoY,NBS,Trade
EXPORT_CUMULATIVE_YOY,2025-10,5.3,%,YoY,NBS,Trade
EXPORT_CUMULATIVE_YOY,2025-11,5.4,%,YoY,NBS,Trade
IMPORT_CUMULATIVE_YOY,2025-09,-1.1,%,YoY,NBS,Trade
IMPORT_CUMULATIVE_YOY,2025-10,-0.9,%,YoY,NBS,Trade
IMPORT_CUMULATIVE_YOY,2025-11,-0.6,%,YoY,NBS,Trade
TRADE_TOTAL_CUMULATIVE_YOY,2025-09,3.1,%,YoY,NBS,Trade
TRADE_TOTAL_CUMULATIVE_YOY,2025-10,2.7,%,YoY,NBS,Trade
TRADE_TOTAL_CUMULATIVE_YOY,2025-11,2.9,%,YoY,NBS,Trade
```

### Module FX_Market (2 entries)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
FX_CNY_MID_MONTHLY,2025-11,7.085,CNY/USD,Level,PBOC,FX
FX_CNY_MID_MONTHLY,2025-12,7.059,CNY/USD,Level,PBOC,FX
FX_CNY_MID_MONTHLY,2026-01,7.02,CNY/USD,Level,PBOC,FX
XAUUSD_AVG_MONTHLY,2025-12,4308.93,USD/oz,Level,LBMA,FX
XAUUSD_AVG_MONTHLY,2026-01,4750.92,USD/oz,Level,LBMA,FX
XAUUSD_AVG_MONTHLY,2026-02,4754.97,USD/oz,Level,LBMA,FX
```

### Module Labor_Market (2 entries)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
URBAN_SURVEY_UNEMPLOYMENT,2025-09,5.2,%,Level,NBS,Labor
URBAN_SURVEY_UNEMPLOYMENT,2025-10,5.1,%,Level,NBS,Labor
URBAN_SURVEY_UNEMPLOYMENT,2025-11,5.1,%,Level,NBS,Labor
URBAN_YOUTH_UNEMPLOYMENT,2025-09,17.7,%,Level,NBS,Labor
URBAN_YOUTH_UNEMPLOYMENT,2025-10,17.3,%,Level,NBS,Labor
URBAN_YOUTH_UNEMPLOYMENT,2025-11,16.9,%,Level,NBS,Labor
```

### Module Investment_Indicators (2 entries)

This reproduces your label choice where `INVESTMENT_FA_YOY` appears again under `Investment` (same `indicator_id`, different `data_type`).

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
INVESTMENT_FA_PRIVATE_YOY,2025-09,-3.1,%,YoY,NBS,Investment
INVESTMENT_FA_PRIVATE_YOY,2025-10,-4.5,%,YoY,NBS,Investment
INVESTMENT_FA_PRIVATE_YOY,2025-11,-5.3,%,YoY,NBS,Investment
INVESTMENT_FA_YOY,2025-09,-0.5,%,YoY,NBS,Investment
INVESTMENT_FA_YOY,2025-10,-1.7,%,YoY,NBS,Investment
INVESTMENT_FA_YOY,2025-11,-2.6,%,YoY,NBS,Investment
```

### Module Risk_Sentiment (1 entry)

```csv
indicator_id,ref_month,value,unit,adjustment,source,data_type
IV_OPTION_INDEX,2025-11,22.5,%,Level,SSE/SZSE/Wind,Risk_Sentiment
IV_OPTION_INDEX,2025-12,21.0,%,Level,SSE/SZSE/Wind,Risk_Sentiment
IV_OPTION_INDEX,2026-01,23.8,%,Level,SSE/SZSE/Wind,Risk_Sentiment
```

## Mock coverage and replacement notes

Only these indicators required mock values because your pasted sample did not include them: `M1_YOY`, `M1_M2_GAP`, `DR007`, `CREDIT_SPREAD_AAA_10Y`, `ERP_EP_10Y`, `IV_OPTION_INDEX`.

When replacing with live data, keep schema unchanged and ensure your pipelines respect the source-specific publishing patterns:

- `M1_YOY` and `M2_YOY` are explicitly reported in PBOC financial statistics releases (along with YoY growth framing), so your stored fields `unit="%"` and `adjustment="YoY"` are consistent with their reporting conventions. citeturn0search0  
- `LPR` is described as calculated by the National Interbank Funding Center and published as a benchmark rate series; storing it as `Level` with `unit="%"` matches typical benchmark presentation. citeturn2search0  
- `DR007` is part of the pledged repo rates published via ChinaMoney/CFETS market data pages; for a monthly table, choose a consistent aggregation rule (month-end or month average) and document it in your ETL. citeturn0search2turn0search10  
- `YIELD_10Y` and yield curve data sourced from ChinaBond are naturally daily; the ChinaBond site provides yield curves and historical queries, so the monthly value should be derived by aggregation. citeturn2search1turn2search9  
- `XAUUSD_AVG_MONTHLY` can be constructed as a monthly average sourced from LBMA benchmark prices; LBMA describes the benchmark and its administration by ICE Benchmark Administration. citeturn0search3  
- For `IV_OPTION_INDEX`, academic literature documents that SSE has released an implied volatility index (iVX/iVIX family) derived from ETF options; if you are using vendor computed IV (e.g., 50ETF IV or 300ETF IV), keep it as `Level` in `%` and clarify which underlying you selected. citeturn3search22turn3search6