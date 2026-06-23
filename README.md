# Spatial Visual Analytics for Trustworthy GeoAI

Course materials for the TUM course **Spatial Visual Analytics (SVA) for Trustworthy Geospatial Artificial Intelligence**.
This repository hosts the exercise notebooks, case studies, and datasets used throughout the course. Every notebook is designed to run end-to-end in **Google Colab** with **no local setup** — you only need a Google account.

> Technical University of Munich · Summer Semester 2026

---

## 🚀 Quick start (students start here)

Open a notebook in Colab and run it top to bottom. **Each notebook is self-contained**: its first cells install the packages it needs and download the data automatically (1–2 min on Colab the first time). Because Colab gives every notebook its own runtime, each one sets up its own environment — just open the one you want and run it.

| # | Notebook | Topic | Open in Colab |
|---|----------|-------|---------------|
| 1 | **Case 1 — Uncertainty in Spatial Data** | Imbalanced sampling, incompleteness, and the MAUP | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Vezarachan/TUM_Course_SVA_TrustGeoAI/blob/main/notebooks/TrustworthyGeoAI_case_1.ipynb) |
| 2 | **Case 2 — Uncertainty in Spatially Explicit Modeling** | Spatial inductive bias and spatial transferability | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Vezarachan/TUM_Course_SVA_TrustGeoAI/blob/main/notebooks/TrustworthyGeoAI_case_2.ipynb) |

**How to run a notebook in Colab**

1. Click an **Open in Colab** badge above.
2. Run the cells top to bottom (`Shift`+`Enter`), or **Runtime → Run all**.
3. The first setup cell installs a few geospatial packages (1–2 min the first time) and downloads the data automatically. No GPU needed.

That's it — you never have to download anything by hand.

---

## Course overview

Modern GeoAI systems make consequential predictions about land cover, urbanization, and climate risk — often from data and models whose **uncertainty**, **bias**, and **spatial scale** are easy to overlook. This course teaches students to *see* those issues with visual analytics, and to reason about when a model output can be trusted.

The current materials cover two themes:

- **Case 1 — Uncertainty in spatial *data*:** imbalanced sampling across regions, incompleteness of coverage, and spatial scale / the Modifiable Areal Unit Problem (MAUP).
- **Case 2 — Uncertainty in spatially explicit *modeling*:** **spatial inductive bias** (how a model uses geography — e.g. coordinates as features, or a spatial model like GWR — and how confident it is, via GeoConformal Prediction) and **spatial transferability** (how performance changes when a model is moved between regions, and how the choice of spatial split changes what you measure).

Additional topics (calibration, fairness, explainability) will be added in future sessions.

---

## Repository structure

```
TUM_Course_SVA_TrustGeoAI/
├── notebooks/                         # Exercise notebooks (open in Colab)
│   ├── TrustworthyGeoAI_case_1.ipynb
│   └── TrustworthyGeoAI_case_2.ipynb
├── geocp/                             # GeoConformal Prediction package (used in Case 2)
├── data/                              # Datasets (see "Datasets" below)
├── figures/                           # Static figures referenced from notebooks
├── requirements.txt                   # For local installs (Colab installs automatically)
└── LICENSE
```

Larger datasets (e.g. the full EuroSAT image archive) are **not** stored in this repo; they are pulled from external hosts inside the notebooks.

### Datasets

| File | What it is | Used in |
|------|------------|---------|
| `eurosat_metadata.parquet` | Per-tile metadata for the [EuroSAT](https://github.com/phelber/EuroSAT) land-cover dataset. | Case 1 |
| `deu_buildup.tif` | Built-up area raster of Germany — reference layer for OSM building completeness. | Case 1 |
| `{city}_buildings.parquet` × 6 | OSM building footprints for Berlin, Hamburg, München, Frankfurt, Leipzig, Dresden. | Case 1 |
| `city_boundaries.parquet`, `city_subdivisions.parquet` | City outlines and `admin_level=9` subdivisions for the 6 cities. | Case 1 |
| `us_election.gpkg` | 3,108 US county polygons with demographics + 2012/2016 vote shares (MAUP demo). | Case 1 |
| `king_county.gpkg` | Multi-level admin boundaries for King County, WA (county / places / tracts). | Case 1 & 2 |
| `seattle_sample_3k.csv` | 3,000 Seattle home sales: log price, 8 features, UTM coordinates. | Case 2 |
| `US_Climate_ERA5_CLIMATE.csv` | A continuous target + 9 predictors across the 9 NOAA US climate regions. | Case 2 |

The `geocp/` folder is a small **GeoConformal Prediction** package (Lou et al., 2025) used in Case 2 to produce spatially-varying, calibrated prediction intervals. The notebooks download it automatically.

---

## Running locally (optional)

Colab is recommended, but you can also run everything locally:

```bash
git clone https://github.com/Vezarachan/TUM_Course_SVA_TrustGeoAI.git
cd TUM_Course_SVA_TrustGeoAI
pip install -r requirements.txt
jupyter lab
```

Python 3.10+ is recommended. The `geocp` package is included in the repo, so no extra install is needed for it.

---

## For students

- Each notebook installs what it needs on its first run — just open it in Colab and **Run all**.
- Submissions: please follow the instructions on the course Moodle page.
- Found a bug in a notebook? Open a GitHub issue with the notebook name and the cell number.

## For instructors / re-use

You are welcome to fork this repository and adapt it for your own course under the [MIT License](LICENSE). A citation back to this repo and to the TUM SVA course is appreciated.

---

## Acknowledgements

- The EuroSAT dataset is published by Helber et al. (2019), *EuroSAT: A Novel Dataset and Deep Learning Benchmark for Land Use and Land Cover Classification.*
- GeoConformal Prediction: Lou, Luo & Meng (2025).
- Course developed at the Technical University of Munich.

## Contact

Course instructor: Xiayin Lou — `xiayin.lou@tum.de`
