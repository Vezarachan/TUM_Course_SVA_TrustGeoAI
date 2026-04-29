# Spatial Visual Analytics for Trustworthy GeoAI

Course materials for the TUM course **Spatial Visual Analytics (SVA) for Trustworthy Geospatial Artificial Intelligence**.
This repository hosts the exercise notebooks, case studies, and example datasets that students use throughout the course. Notebooks are designed to run end-to-end in **Google Colab** with no local setup.

> Technical University of Munich · Summer Semester 2026

---

## Course overview

Modern GeoAI systems make consequential predictions about land cover, urbanization, and climate risk — often from data whose **uncertainty**, **bias**, and **spatial scale** are easy to overlook. This course teaches students to *see* those issues with the help of visual analytics, and to reason about when a model output can be trusted.

The current materials focus on **uncertainty in spatial data**, examined from three angles:

- **Imbalanced sampling** of spatial data across regions.
- **Incompleteness** of spatial data across regions.
- **Spatial scale** and the Modifiable Areal Unit Problem (MAUP).

Additional topics (model calibration, fairness, explainability) will be added in future sessions.

---

## Repository structure

```
TUM_Course_SVA_TrustGeoAI/
├── Exercises/            # Weekly exercise notebooks
│   ├── TrustworthyGeoAI_case_1.ipynb
│   └── TrustworthyGeoAI_case_2.ipynb
├── data/                 # Small example datasets (parquet, csv, geojson)
│   └── eurosat_metadata.parquet
├── figures/              # Static figures referenced from notebooks
└── LICENSE
```

Larger datasets (e.g. the full EuroSAT image archive) are **not** stored in this repo. They are pulled from external hosts (Hugging Face / Zenodo) inside the notebooks.

---

## Getting started

### Option 1 — One-click in Google Colab (recommended)

Click the badge next to any notebook below and it will open directly in Colab. The first cell of each notebook handles dataset download, so you only need a Google account.

| # | Notebook | Open |
|---|----------|------|
| 1 | Uncertainty in Spatial Data | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Vezarachan/TUM_Course_SVA_TrustGeoAI/blob/main/Exercises/TrustworthyGeoAI_case_1.ipynb) |
| 2 | *(coming soon)* | — |

### Option 2 — Run locally

```bash
git clone https://github.com/Vezarachan/TUM_Course_SVA_TrustGeoAI.git
cd TUM_Course_SVA_TrustGeoAI
pip install -r requirements.txt
jupyter lab
```

Python 3.10+ is recommended.

---

## Loading datasets inside a notebook

Each notebook contains a setup cell that pulls the data it needs. The two patterns we use:

**Small files** — direct download from the repo:
```python
!wget -q https://raw.githubusercontent.com/Vezarachan/TUM_Course_SVA_TrustGeoAI/main/data/eurosat_metadata.parquet
```

**Larger image datasets** — Hugging Face:
```python
from datasets import load_dataset
ds = load_dataset("blanchon/EuroSAT_RGB", split="train")
```

---

## For students

- Submissions: please follow the instructions on the course Moodle page.
- Questions: open a GitHub issue, or post on the course forum.
- Bugs in a notebook? Open an issue with the notebook name and the cell number.

## For instructors / re-use

You are welcome to fork this repository and adapt it for your own course under the terms of the [MIT License](LICENSE). If you do, a citation back to this repo and to the TUM SVA course is appreciated.

---

## Acknowledgements

- The EuroSAT dataset is published by Helber et al. (2019), *EuroSAT: A Novel Dataset and Deep Learning Benchmark for Land Use and Land Cover Classification.*
- Course developed at the Technical University of Munich.

## Contact

Course instructor: Xiayin Lou — `xiayin.lou@tum.de`
