# Recidivism Dataset - Phase 1 Exploratory Data Analysis

## Dataset
**`recid.csv`** - Rossi et al. recidivism study (Maryland, 1970s)  
1 445 rows × 19 columns. Each row is a released prisoner.  
Key target: **`durat`** (weeks until re-arrest or end of follow-up) and **`cens`** (censoring flag).

## Project Structure
```
recidivism_eda/
├── phase1_eda.py        # Main EDA script (all 8 tasks)
├── requirements.txt     # Python dependencies
├── README.md            # This file
└── outputs/             # Generated figures (created on first run)
    ├── fig6a_univariate_durat.png
    ├── fig6b_univariate_binary_prevalence.png
    ├── fig6c_univariate_continuous_kde.png
    ├── fig6d_univariate_education.png
    ├── fig7a_bivariate_durat_rearrested.png
    ├── fig7b_bivariate_rearrest_by_factor.png
    ├── fig7c_bivariate_scatter_regression.png
    ├── fig7d_bivariate_durat_education.png
    ├── fig8a_multivariate_correlation.png
    ├── fig8b_multivariate_pairplot.png
    ├── fig8c_multivariate_facet_kde.png
    ├── fig8d_multivariate_priors_alcohol_rearrest.png
    └── fig8e_multivariate_3d_scatter.png
```

## Setup & Run
```bash
pip install -r requirements.txt
python phase1_eda.py
```
Place `recid.csv` at `/mnt/user-data/uploads/recid.csv` (or update `RAW_PATH` in the script).

---

## Phase 1 Task Index

| # | Task | Location in script |
|---|------|--------------------|
| 1 | Load dataset | `# 1. LOAD DATASET` |
| 2 | Basic statistical analysis | `# 2. BASIC STATISTICAL ANALYSIS` |
| 3 | Handle missing data | `# 3. HANDLE MISSING DATA` |
| 4 | Data cleaning | `# 4. DATA CLEANING` |
| 5 | Data transformation | `# 5. DATA TRANSFORMATION` |
| 6 | Univariate analysis | `# 6. UNIVARIATE ANALYSIS` |
| 7 | Bivariate analysis | `# 7. BIVARIATE ANALYSIS` |
| 8 | Multivariate analysis | `# 8. MULTIVARIATE ANALYSIS` |

---

## Visualisation Summary

### Univariate (4 figures)
| Figure | What it shows |
|--------|--------------|
| 6-A | `durat` — histogram + KDE, log-transformed KDE, box plot |
| 6-B | Prevalence (%) of all binary features |
| 6-C | KDE + histogram for age, time served, prior convictions, rule violations |
| 6-D | Count of prisoners by education tier |

### Bivariate (4 figures)
| Figure | What it shows |
|--------|--------------|
| 7-A | `durat` violin + KDE split by re-arrest status |
| 7-B | Re-arrest rate (%) for each binary risk factor (feature=1 vs feature=0) |
| 7-C | Scatter + OLS regression: age → durat, priors → durat |
| 7-D | Box plots of `durat` across education levels |

### Multivariate (5 figures)
| Figure | What it shows |
|--------|--------------|
| 8-A | Full Pearson correlation heatmap (15 variables) |
| 8-B | Pair plot of 4 key continuous variables, coloured by re-arrest status |
| 8-C | Faceted KDE of `durat` by supervision × marital status, coloured by race |
| 8-D | Re-arrest rate by prior-conviction band, split by alcohol history |
| 8-E | 3-D scatter: age × priors × durat, coloured by re-arrest status |

---

## Key Findings

- **38.2 % re-arrested** within the follow-up window (median 74 weeks).
- Re-arrested prisoners had substantially shorter survival times (median ≈ 25 weeks vs. full follow-up for censored).
- **Prior convictions** show a weak negative correlation with survival time (more priors → sooner re-arrest).
- **Married** prisoners had notably lower re-arrest rates across all supervision and race subgroups.
- **Age** shows a slight positive relationship with survival time - older prisoners take longer to re-offend.
- **Alcohol history** amplifies the effect of prior convictions on re-arrest rate.
- Supervision (`super`) is nearly universal (69 %) and does not strongly differentiate outcomes on its own.
