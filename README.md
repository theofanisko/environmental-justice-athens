# Environmental Justice Athens
### Does the climate crisis hit everyone equally?

A data science case study mapping environmental inequality across 66 Attica municipalities using satellite imagery, census data and fire service records — and proving the **Luxury Effect** exists in Athens.

🌐 **Live site:** [theofanisko.github.io/environmental-justice-athens](https://theofanisko.github.io/environmental-justice-athens)

---

## What this project does

This study combines four independent data sources to build an **Environmental Vulnerability Index (EVI)** for every municipality in Attica, covering the summers of 2020–2024.

The central hypothesis — the **Luxury Effect** — states that affluent neighbourhoods enjoy more green space and lower temperatures, while deprived areas act as thermal traps and face greater flood exposure. The analysis confirms it: municipalities with above-median unemployment are on average **+0.53°C hotter** than wealthier ones, with a vegetation gap of up to **54%**.

---

## Key findings

| Finding | Value |
|---|---|
| Temperature rise across Attica (2020→2024) | **+3.82°C** |
| Luxury Effect delta (deprived vs affluent) | **+0.53°C** |
| Vegetation gap (min vs max NDVI) | **−54%** |
| Most vulnerable municipality | **Athens (EVI: 0.768)** |
| Least vulnerable municipality | **Dionysos (EVI: 0.231)** |
| Peak flood year | **2023 (691 incidents)** |

---

## Data sources

| Pillar | Source | Data |
|---|---|---|
| Environment | Landsat 8/9 — Google Earth Engine | LST, NDVI, NDBI at 30m resolution |
| Society | ELSTAT Census 2021 | Unemployment rate per municipality |
| Risk | Hellenic Fire Service | Water pumping incidents 2020–2024 |
| Geography | GADM Level 3 | Municipal boundaries — Attica |

---

## Python pipeline

The analysis runs through 6 sequential scripts:

```
01_extract_satellite_data.py    ← Landsat imagery extraction via GEE
02_extract_flood_data.py        ← Fire Service flood incident filtering
03_extract_socioeconomic_data.py← ELSTAT Census 2021 processing
04_merge_master_dataset.py      ← All sources merged into master dataset
05_vulnerability_index.py       ← EVI computation per municipality × year
06_prepare_powerbi_data.py      ← Final cleaning and PowerBI export
```

**Output:** 330 observations (66 municipalities × 5 years) with LST, NDVI, NDBI, flood risk and unemployment rate — ready for analysis and visualisation.

---

## Acronyms

| Acronym | Full name | Description |
|---|---|---|
| **EVI** | Environmental Vulnerability Index | Composite score (0–1) measuring a municipality's overall climate vulnerability. Computed as equal-weight average of LST, NDVI deficit, flood risk and unemployment. Higher = more vulnerable. |
| **LST** | Land Surface Temperature | Temperature of the Earth's surface (roads, rooftops, soil) as measured by satellite. Different from air temperature — urban surfaces can reach 50°C in summer. Measured in °C. |
| **NDVI** | Normalized Difference Vegetation Index | Satellite-derived measure of vegetation density, ranging from −1 to +1. Values above 0.4 indicate dense vegetation; urban cores typically fall below 0.2. |
| **NDBI** | Normalized Difference Built-up Index | Satellite-derived measure of built-up surface density (concrete, asphalt). Higher values indicate more impervious surfaces, which amplify the urban heat island effect. |
| **GEE** | Google Earth Engine | Cloud-based geospatial analysis platform used to process Landsat satellite imagery at scale. |
| **GADM** | Global Administrative Areas | Open-source database of administrative boundaries used to define municipal borders for zonal statistics. |
| **ELSTAT** | Hellenic Statistical Authority | Greece's national statistics agency. Source of Census 2021 data on unemployment, education and population per municipality. |

---

## Methodology

The EVI is computed following **Cutter, Boruff & Shirley (2003)**:

```
EVI = 0.25 × LST_norm
    + 0.25 × (1 − NDVI_norm)
    + 0.25 × flood_risk_norm
    + 0.25 × unemployment_norm
```

Each component is min-max normalised to [0, 1]. Equal weights (25% each) follow the standard approach for social vulnerability indices in the absence of domain-specific weighting evidence.

> Cutter, S.L., Boruff, B.J. & Shirley, W.L. (2003). *Social Vulnerability to Environmental Hazards.* Social Science Quarterly, 84(2), 242–261.

---

## Tools & technologies

`Python` · `Google Earth Engine` · `Landsat 8/9` · `geopandas` · `geemap` · `pandas` · `GADM` · `PowerBI` · `DAX` · `GitHub Pages`

---

*Theofanis Kotsis · Ioannina, Greece · 2025*
