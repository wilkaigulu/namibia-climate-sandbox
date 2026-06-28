# Namibian Station Audit — NOAA ISD Data Quality Assessment

**Prepared by:** Wilka Igulu, Quantil Risk CC  
**Date:** June 2026  
**Data source:** NOAA NCEI Global Historical Climatology Network Daily (GHCND)  
**API endpoint:** https://www.ncei.noaa.gov/access/services/data/v1

---

## Background

As a first step in building the climate risk pipeline for the Goreangab Water 
Reclamation Plant, I pulled daily weather records for ten Namibian stations from 
the NOAA NCEI database. The goal was to understand what data is actually available 
before committing to any modelling choices downstream.

The station IDs listed in most references to NOAA's Integrated Surface Database 
(ISD) are not directly compatible with the GHCND daily-summaries API. Correct 
GHCND identifiers were obtained from the NOAA master stations file at:
https://www.ncei.noaa.gov/pub/data/ghcn/daily/ghcnd-stations.txt

---

## Station inventory

| Station | GHCND ID | Period | Years | Completeness | PRCP Coverage | DQS |
|---|---|---|---|---|---|---|
| Windhoek Main (GSN) | WA007401540 | 1970–2024 | 55.0 | 98.5% | 70.6% | **0.921** |
| Rundu | WA012084750 | 1970–2024 | 55.0 | 85.2% | 51.4% | **0.819** |
| Mariental | WA005688170 | 1970–1986 | 16.3 | 96.7% | 99.9% | **0.827** |
| Windhoek B | WA007401240 | 1970–1985 | 15.4 | 98.4% | 100.0% | **0.823** |
| Windhoek C | WA007401850 | 1970–1984 | 14.4 | 98.3% | 100.0% | **0.811** |
| Windhoek A | WA007400630 | 1970–1984 | 14.4 | 96.6% | 100.0% | **0.804** |
| Keetmanshoop | WA004192150 | 1970–1985 | 15.4 | 93.0% | 100.0% | **0.802** |
| Gobabis | WA007878380 | 1970–2024 | 55.0 | 60.9% | 60.2% | **0.744** |
| Walvis Bay | WAM00068098 | 1990–2024 | 34.9 | 58.3% | 6.6% | **0.600** ⚠ |
| Ondangwa | WAM00068006 | 1973–2024 | 51.9 | 35.0% | 15.9% | **0.530** ⚠ |
| Lüderitz | — | — | — | — | — | **NOT IN GHCND** |

The Data Quality Score (DQS) is a weighted composite: record length (0.35), 
completeness (0.40), and PRCP coverage (0.25). Scores range from 0 to 1.

---

## Range validation

All retrieved records were checked against physically plausible bounds for 
Namibia: TMAX (−5°C to 50°C), TMIN (−10°C to 40°C), PRCP (0–300 mm/day), 
and wind speed (0–50 m/s). Zero outliers were detected across all stations. 
This is consistent with NOAA NCEI applying its own automated QC before 
serving data through the API, flagged values are withheld rather than 
passed through as bad numbers.

---

## Flags

**Ondangwa (DQS = 0.530):** Marginally above the 0.50 threshold. Record 
coverage is only 35% of expected days, and precipitation data is sparse 
throughout. The station had no records at all during the 1990s. Any output 
derived from Ondangwa data should be treated as indicative only.

**Walvis Bay (DQS = 0.600):** Records begin only in 1990 and precipitation 
coverage is just 6.6%, reflecting the hyperarid coastal climate where rain 
events are so rare that the station may not systematically record them. 
Walvis Bay temperature and wind data are more reliable than its precipitation 
figures suggest.

**Lüderitz:** Absent from the GHCND database entirely. This is a genuine 
data gap in NOAA's Namibian coverage. Lüderitz is noted here for 
transparency but cannot be included in the analysis.

---

## Conclusion

Three stations are suitable for Extreme Value Theory modelling: Windhoek Main 
(WA007401540), Rundu (WA012084750), and Gobabis (WA007878380). These are the 
only stations with records spanning 1970 to the present day, which is the 
minimum length needed for credible return level estimation.

Windhoek Main is the primary station for this pipeline. It carries the highest 
DQS in the network (0.921), covers 55 years without interruption and is the 
station closest to the Goreangab Water Reclamation Plant the target asset. 
All EVT outputs in subsequent phases are derived from this station, with the 
DQS attached to every result as a transparency mechanism.

Rundu and Gobabis are retained as secondary stations for spatial 
cross-validation. Their lower PRCP coverage scores are noted and will be 
referenced where their data is used.
