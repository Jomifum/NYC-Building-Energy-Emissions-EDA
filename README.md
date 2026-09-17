# NYC Building Energy & Emissions Analysis

**An exploratory analysis of ~38,000 NYC buildings' energy use and greenhouse gas emissions, with a Local Law 97 penalty-risk screening.**

> Data analyst with environmental engineering domain expertise — applying Python data analysis to NYC's building decarbonization data.

---

## Overview

New York City's [Local Law 84](https://www.nyc.gov/site/buildings/codes/ll84-benchmarking-law.page) requires large buildings to report annual energy and water use, and [Local Law 97](https://accelerator.nyc/building-laws/ll97) now caps their carbon emissions with financial penalties for exceedance. This project analyzes the public benchmarking data to answer three questions:

1. **Which building types and boroughs drive NYC's building emissions?**
2. **Is that a story about inefficiency, or simply about floor area?**
3. **Which buildings are at risk of Local Law 97 penalties?**

## Key findings

- **Emissions are highly concentrated:** ~10% of buildings produce roughly half of all reported emissions — meaning a targeted decarbonization program could capture most of the available reduction.
- **Absolute emissions are a floor-area story, not an efficiency story.** Multifamily housing is ~59% of total emissions but ranks 8th of 13 property types by emissions *intensity*. Its dominance reflects the volume of residential square footage in NYC, not per-building inefficiency.
- **The least efficient building types are a different group entirely** — senior living, hotels, and universities lead on intensity while contributing little to the citywide total. This distinction maps directly onto Local Law 97, which regulates intensity rather than absolute emissions.
- **LL97 screening** flags approximately the expected share of buildings (under ~10%) as at risk of penalties in the 2024–2029 compliance period, consistent with Urban Green Council's published estimate.

## Data

- **Source:** [NYC Open Data — Building Energy and Water Data Disclosure for Local Law 84 (2023–Present)](https://data.cityofnewyork.us/Environment/NYC-Building-Energy-and-Water-Data-Disclosure-for-/5zyy-y8am)
- **Scope:** Calendar years 2022–2024; ~103,000 raw records covering privately owned buildings >25,000 ft² and City buildings >10,000 ft².
- **After cleaning:** 38,199 deduplicated properties (one row per property, most recent reporting year).

## Method

| Stage | What it does |
|-------|--------------|
| **1. Load & inspect** | Reads the 265-column source (handling UTF-8/Latin-1 encoding artifacts), profiles structure and missingness. |
| **2. Clean & validate** | Converts `"Not Available"` placeholders to `NaN`, strips thousands-separators, coerces numerics, and applies physically defensible plausibility bounds (removing ~1.8% of rows as data-entry/metering errors). |
| **3. Univariate EDA** | Distributions of EUI, emissions, floor area, and ENERGY STAR score. |
| **4. Category analysis** | Emissions by property type and borough — separating *absolute* totals from *intensity*. |
| **5. Relationships** | Size vs efficiency; validation of ENERGY STAR score against emissions intensity. |
| **6. LL97 risk snapshot** | Screens each building's intensity against approximate LL97 2024–2029 occupancy-group limits and estimates penalty exposure. |

## Tech stack

`Python` · `pandas` · `NumPy` · `Matplotlib` · `Seaborn` · `Jupyter`

## Repository structure

```
.
├── README.md
├── notebooks/
│   └── nyc_building_emissions_eda.ipynb   # full analysis
├── data/
│   └── README.md                          # how to download the source (data not committed)
├── figures/                               # exported charts
└── requirements.txt
```

> **Note:** the raw dataset is not committed (it's large and public). See `data/README.md` for the one-line download instructions.

## How to reproduce

```bash
git clone https://github.com/<your-username>/nyc-building-emissions.git
cd nyc-building-emissions
pip install -r requirements.txt
# download the CSV per data/README.md, then:
jupyter notebook notebooks/nyc_building_emissions_eda.ipynb
```

## Caveats

The Local Law 97 screening is an **approximate** analysis for exploratory purposes, not a compliance determination: it maps ESPM property types to the nearest building-code occupancy group, applies a single limit per building (rather than a floor-area-weighted blend for mixed use), and uses reported location-based GHG intensity as a proxy for LL97's fuel-coefficient methodology. Deferred/adjusted timelines for rent-stabilized and other exempt categories are not modeled.

## About

Part of a portfolio applying data science to environmental and climate problems. Built by a data analyst (M.S. Data Science, CUNY) with a background in environmental engineering.
