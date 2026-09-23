# Recidivism Dataset - Exploratory Data Analysis & Clustering

A two-phase Data Science project on the Rossi Recidivism Dataset (1,445 released prisoners),
covering statistical EDA and unsupervised clustering to identify behavioral risk segments.

## Dataset

**`recid.csv`** - Rossi et al. recidivism study (Maryland, 1970s)
1,445 rows x 19 columns. Each row is a released prisoner.
Key target: `durat` (weeks until re-arrest or end of follow-up) and `cens` (censoring flag).

| Column | Description |
|--------|-------------|
| black, alcohol, drugs, super, married, felon, workprg, property, person, cens | Binary indicators (0/1) |
| priors | Number of prior convictions |
| educ | Education (grade-level scale) |
| rules | Prison rule violations |
| age, tserved | Age and time served (months) |
| follow | Weeks in follow-up window |
| durat | Weeks until re-arrest or censoring |
| ldurat | log(durat) |

## How to Run

1. Open `EDA_Course_Project.ipynb` in Google Colab
2. Run cells top to bottom. Cell 2 (Upload dataset) will prompt for `recid.csv`
3. **Runtime -> Run all**

---

## Phase 1: Exploratory Data Analysis (Cells 0-37)

| # | Task |
|---|------|
| 1 | Load dataset |
| 2 | Basic statistical analysis (descriptives, prevalence, skewness/kurtosis) |
| 3 | Handle missing data |
| 4 | Data cleaning (duplicates, logical checks, outlier detection) |
| 5 | Data transformation (unit conversion, derived flags, binning, standardization) |
| 6 | Univariate (1D) analysis, 4 visualizations |
| 7 | Bivariate (2D) analysis, 4 visualizations |
| 8 | Multivariate (3D) analysis, 5 visualizations |

### Visualizations

**Univariate**
- Fig 6-A: `durat` distribution: histogram, KDE, log-transform, box plot
- Fig 6-B: Prevalence of all binary features
- Fig 6-C: KDE grid for age, time served, priors, rule violations
- Fig 6-D: Education level distribution

**Bivariate**
- Fig 7-A: `durat` by re-arrest status, violin + KDE
- Fig 7-B: Re-arrest rate by each binary risk factor
- Fig 7-C: Scatter + OLS regression for age vs durat, priors vs durat
- Fig 7-D: `durat` by education level (box plot)

**Multivariate**
- Fig 8-A: Pearson correlation heatmap (15 variables)
- Fig 8-B: Pair plot of age, time served, priors, durat, colored by re-arrest
- Fig 8-C: Faceted KDE of supervision x marital status, colored by race
- Fig 8-D: Re-arrest rate by prior-conviction band x alcohol history
- Fig 8-E: 3D scatter of age x priors x durat, colored by re-arrest status

---

## Phase 2: Clustering Analysis (Cells 38-52)

Unsupervised segmentation of prisoners into behavioral risk groups using
standardized numeric features: `age_years`, `tserved_years`, `priors`, `rules`, `educ`, `durat`.

| # | Task |
|---|------|
| 9 | Feature scaling (StandardScaler) |
| 10 | Optimal cluster count via elbow method and silhouette score |
| 11 | K-Means clustering |
| 12 | Hierarchical (Agglomerative, Ward linkage) clustering |
| 13 | Method comparison |

### Visualizations

- Fig 9-A: Elbow curve (inertia) and silhouette score across k = 2 to 10
- Fig 9-B: K-Means clusters, PCA-reduced 2D projection plus `durat` box plot by cluster
- Fig 9-C: Dendrogram (Ward linkage, 100-point sample)
- Fig 9-D: Hierarchical clusters, PCA-reduced 2D projection plus `durat` box plot by cluster

### Results

| Method | Clusters (k) | Silhouette Score |
|--------|:-:|:-:|
| K-Means | 2 | 0.361 |
| Hierarchical (Ward) | 2 | 0.303 |

- **Cluster 0** (n=275, K-Means): younger, higher priors (4.3), more rule violations, shorter survival time (34 wks), 71% re-arrest rate
- **Cluster 1** (n=1170, K-Means): fewer priors (0.75), longer survival time (60 wks), 31% re-arrest rate
- Label agreement between K-Means and Hierarchical clustering: 82%

---

## Tools Used
Python, Pandas, NumPy, Matplotlib, Seaborn, SciPy, Scikit-learn (KMeans, AgglomerativeClustering, StandardScaler, PCA), Google Colab

## Key Findings

- 38.2% of released prisoners were re-arrested within the follow-up window (median 74 weeks).
- Prior convictions and rule violations are the strongest drivers of shorter survival time.
- Marital status and alcohol history meaningfully affect re-arrest rate within each risk cluster.
- Clustering confirms two broad behavioral profiles: a smaller high-risk group (frequent priors, short survival time) and a larger low-risk group, consistent with the bivariate and multivariate findings from Phase 1.
