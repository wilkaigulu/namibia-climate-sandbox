# Phase 2 — CMIP6 Extraction and Bias Correction Summary

## Models and scenarios extracted

| Dataset | Period | Rows | Status |
|---|---|---|---|
| MPI-ESM1-2-HR historical | 1970–2014 | 16,436 | Complete |
| MPI-ESM1-2-HR ssp245 | 2015–2060 | 16,802 | Corrected |
| MPI-ESM1-2-HR ssp585 | 2015–2060 | 16,802 | Corrected |
| CMCC-ESM2 historical | 1970–2014 | 16,425 | Complete |
| CMCC-ESM2 ssp245 | 2015–2060 | 16,790 | Corrected |
| CMCC-ESM2 ssp585 | 2015–2060 | 16,790 | Corrected |

## Bias before and after QDM correction

| Model | Raw mean (mm/day) | Corrected mean (mm/day) | Observed mean (mm/day) |
|---|---|---|---|
| MPI-ESM1-2-HR | 1.608 | 1.158 avg | 1.192 |
| CMCC-ESM2 | 2.768 | 0.965 avg | 1.192 |

## Validation statistics (historical vs observed, daily precipitation)

| Metric | MPI-ESM1-2-HR | CMCC-ESM2 |
|---|---|---|
| KGE | 0.033 | -0.672 |
| Pearson r | 0.092 | 0.130 |
| Alpha (variability ratio) | 1.012 | 1.246 |
| Beta (mean ratio) | 1.332 | 2.407 |
| RMSE (mm/day) | 6.183 | 6.997 |

## Interpretation

KGE scores near zero for daily precipitation at a point location are 
expected and consistent with published literature on CMIP6 model evaluation 
over southern Africa. Global climate models at ~100km resolution simulate 
large-scale atmospheric circulation, not individual rain events. Day-to-day 
timing correlation (r ~ 0.09-0.13) is therefore not expected to be high.

What matters for downstream EVT analysis is the distributional fidelity —
how well the model reproduces the frequency and intensity of extreme events —
and the preservation of the climate change signal. QDM addresses both: it 
corrects the quantile distribution against the observed record while 
preserving the relative change (delta) between historical and future 
projections.

CMCC-ESM2 shows a larger wet bias (beta = 2.41) than MPI-ESM1-2-HR 
(beta = 1.33), consistent with known CMCC behaviour over semi-arid Africa. 
Both models are retained for the ensemble analysis in Phase 3, with the 
bias correction applied before any EVT fitting.

## Method

Quantile Delta Mapping (QDM) was applied to all four future scenarios using:
- Calibration period: 1970-2014 (full overlap between obs and historical)
- Observed reference: Windhoek Main station (WA007401540, DQS = 0.921)
- Number of quantiles: 50
- Delta type: multiplicative (ratio), appropriate for precipitation
