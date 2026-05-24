# Environmental Justice Athens
### Does the climate crisis hit everyone equally?

A data science case study mapping compound environmental vulnerability across 66 Attica municipalities using satellite imagery, census data and fire service records — and demonstrating a statistically significant socio-environmental disparity between wealthier and poorer areas.

🌐 **Live site:** [theofanisko.github.io/environmental-justice-athens](https://theofanisko.github.io/environmental-justice-athens)

📄 **Preprint:** Kotsis, T. (2025). *Socio-Environmental Inequality in Urban Attica: A Five-Year Multi-Source Assessment of Compound Climate Vulnerability Across 66 Municipalities (2020–2024).* Submitted to Cities (Elsevier).

---

## What this project does

This study integrates four independent data sources to build an **Environmental Vulnerability Index (EVI)** for every municipality in Attica, covering the summers of 2020–2024.

The central finding is that compound environmental vulnerability — a composite of thermal stress, vegetation deficit, flood exposure and socioeconomic disadvantage — is significantly higher in socioeconomically deprived municipalities than in affluent ones. This gap is not episodic: it is **structurally persistent across all five years** of analysis (annual p < 0.001, Cohen's d = 1.117).

---

## Key findings

| Finding | Value |
|---|---|
| Temperature rise across Attica (2020→2024) | **+3.83°C** |
| EVI gap: deprived vs affluent municipalities | **+0.114 (p < 0.001, d = 1.117)** |
| Higher compound vulnerability in deprived areas | **+35%** |
| Vegetation effect on surface temperature | **β = −1.37°C per SD (R² = 0.48)** |
| Most vulnerable municipality | **Athens (EVI: 0.768)** |
| Least vulnerable municipality | **Dionysos (EVI: 0.231)** |
| Peak flood year | **2023 (691 incidents, +55% vs 2020)** |
| EVI gap persistent across | **All 5 years (2020–2024)** |

---

## Data sources

| Pillar | Source | Variables |
|---|---|---|
| Environment | Landsat 8/9 Collection 2 — Google Earth Engine | LST, NDVI, NDBI at 30m resolution |
| Society | ELSTAT Census 2021 | Unemployment rate per municipality |
| Risk | Hellenic Fire Service | Water pumping incidents 2020–2024 |
| Geography | GADM Level 3 | Municipal boundaries — Attica |

> **Note:** Raw Fire Service data are not publicly redistributable (restricted access agreement). Processed outputs and all analysis scripts are available in this repository.

---

## Python pipeline

The analysis runs through 8 sequential scripts:

```
01_extract_satellite_data.py     ← Landsat imagery extraction via GEE (LST, NDVI, NDBI)
02_extract_flood_data.py         ← Fire Service flood incident filtering (ΑΝΤΛΗΣΗ ΝΕΡΟΥ)
03_extract_socioeconomic_data.py ← ELSTAT Census 2021 processing
04_merge_master_dataset.py       ← All sources merged into master dataset (330 obs)
05_vulnerability_index.py        ← EVI computation per municipality × year
06_prepare_powerbi_data.py       ← Final cleaning and PowerBI export
07_statistical_analysis.py       ← T-tests, correlations, yearly trend analysis
08_regression_model.py           ← OLS regression: LST ~ NDVI + unemployment + flood
```

**Output:** 330 observations (66 municipalities × 5 years) with LST, NDVI, NDBI, flood risk and unemployment rate — ready for analysis and visualisation.

---

## EVI Formula

The Environmental Vulnerability Index is computed following **Cutter, Boruff & Shirley (2003)**:

```
EVI = 0.25 × LST_norm
    + 0.25 × (1 − NDVI_norm)
    + 0.25 × flood_risk_norm
    + 0.25 × unemployment_norm
```

Each component is min-max normalised to [0, 1]. Equal weights (25% each) follow the standard approach for social vulnerability indices in the absence of domain-specific weighting evidence.

> Cutter, S.L., Boruff, B.J. & Shirley, W.L. (2003). *Social Vulnerability to Environmental Hazards.* Social Science Quarterly, 84(2), 242–261.

---

## Statistical results

| Test | Result |
|---|---|
| EVI t-test (affluent vs deprived) | t = 4.28, p < 0.001, Cohen's d = 1.117 (large) |
| EVI gap persistent 2020–2024 | Annual p < 0.001, ΔEVI ≈ 0.113–0.118 |
| LST vs NDVI (Pearson) | r = −0.689, p < 0.001 |
| EVI vs Unemployment (Pearson) | r = +0.641, p < 0.001 |
| EVI vs Flood Risk (Pearson) | r = +0.400, p = 0.002 |
| OLS: LST ~ NDVI (Model 2) | β(NDVI) = −1.37, p < 0.001, R² = 0.477 |
| Shapiro-Wilk (residuals) | W = 0.974, p = 0.237 (normal) |
| Durbin-Watson (Model 2) | 1.928 (no autocorrelation) |

---

## Acronyms

| Acronym | Full name | Description |
|---|---|---|
| **EVI** | Environmental Vulnerability Index | Composite score (0–1) measuring compound climate vulnerability per municipality. Higher = more vulnerable. |
| **LST** | Land Surface Temperature | Radiometric surface temperature as measured by Landsat. Different from air temperature — urban surfaces can reach 50°C in summer. |
| **NDVI** | Normalized Difference Vegetation Index | Satellite-derived vegetation density (−1 to +1). Values above 0.4 = dense vegetation; urban cores typically below 0.2. |
| **NDBI** | Normalized Difference Built-up Index | Satellite-derived built-up surface density. Higher values amplify the urban heat island effect. |
| **GEE** | Google Earth Engine | Cloud-based geospatial platform used to process Landsat imagery at municipal scale. |
| **GADM** | Global Administrative Areas | Open-source municipal boundary database (Level 3) used for zonal statistics. |
| **ELSTAT** | Hellenic Statistical Authority | Greece's national statistics agency. Source of Census 2021 unemployment data. |
| **OLS** | Ordinary Least Squares | Linear regression used to model LST as a function of NDVI, unemployment and flood risk. |

---

## Output files

```
output_phase1/
├── master_dataset.csv              ← 330 observations (66 municipalities × 5 years)
├── 05_vulnerability_index.csv      ← EVI per municipality × year
├── 05_vulnerability_index_2024.csv ← EVI snapshot 2024
└── attiki_municipalities.geojson   ← Municipal boundaries for mapping

output_stats/
├── 07_ttest/
│   ├── boxplot_evi.png             ← Main finding: EVI group comparison
│   ├── boxplot_lst.png
│   ├── boxplot_ndvi.png
│   ├── scatter_lst_vs_ndvi.png
│   ├── scatter_evi_vs_unemployment.png
│   ├── correlation_matrix.png
│   ├── line_evi_trend.png          ← 5-year EVI trend: affluent vs deprived
│   ├── line_lst_trend.png
│   ├── yearly_trend_results.csv
│   └── statistical_report.txt
└── 08_regression/
    ├── regression_coefficients.png
    ├── regression_residuals.png
    └── regression_report.txt

figure5_evi_choropleth_attica_2024.png  ← Choropleth map of EVI across Attica
```

---

## Tools & technologies

`Python 3` · `Google Earth Engine` · `Landsat 8/9` · `geopandas` · `geemap` · `pandas` · `scipy` · `matplotlib` · `seaborn` · `GADM` · `PowerBI` · `DAX` · `GitHub Pages`

---

## Citation

```
Kotsis, T. (2025). Socio-Environmental Inequality in Urban Attica: A Five-Year
Multi-Source Assessment of Compound Climate Vulnerability Across 66 Municipalities
(2020–2024). Submitted to Cities (Elsevier).
```

---

*Theofanis Kotsis · Independent Researcher · Ioannina, Greece · 2025*
