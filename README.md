# Recidivism Dataset - EDA, Clustering, PCA & Classification

A three-phase Data Science project on the Rossi Recidivism Dataset (1,445 released prisoners),
covering statistical EDA, unsupervised clustering, dimensionality reduction (PCA) and
classification (Logistic Regression) to understand and predict re-arrest.

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

## Project Structure

```
EDA_Course_Project.ipynb   # Single notebook, all three phases
recid.csv                  # Dataset
README.md                  # This file
```

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

Unsupervised segmentation of prisoners into behavioral risk groups using standardized
numeric features: `age_years`, `tserved_years`, `priors`, `rules`, `educ`, `durat`.

| # | Task |
|---|------|
| 9 | Feature scaling (StandardScaler) |
| 10 | Optimal cluster count via elbow method and silhouette score |
| 11 | K-Means clustering |
| 12 | Hierarchical (Agglomerative, Ward linkage) clustering |
| 13 | Method comparison |

- Fig 9-A: Elbow curve (inertia) and silhouette score across k = 2 to 10
- Fig 9-B: K-Means clusters, PCA-reduced 2D projection plus `durat` box plot by cluster
- Fig 9-C: Dendrogram (Ward linkage, 100-point sample)
- Fig 9-D: Hierarchical clusters, PCA-reduced 2D projection plus `durat` box plot by cluster

| Method | Clusters (k) | Silhouette Score |
|--------|:-:|:-:|
| K-Means | 2 | 0.361 |
| Hierarchical (Ward) | 2 | 0.303 |

- **Cluster 0** (n=275, K-Means): younger, higher priors (4.3), more rule violations, shorter survival time (34 wks), 71% re-arrest rate
- **Cluster 1** (n=1170, K-Means): fewer priors (0.75), longer survival time (60 wks), 31% re-arrest rate
- Label agreement between K-Means and Hierarchical clustering: 82%

---

## Phase 3: PCA and Classification (Cells 53-67)

**Module 6 (PCA)** reduces 14 predictor variables to uncorrelated principal components.
**Module 7 (Logistic Regression)** classifies whether a released prisoner is re-arrested.

`durat`, `ldurat`, `cens` and `follow` are excluded from all Phase 3 features because they
directly encode the outcome (data leakage).

| # | Task |
|---|------|
| 14 | Standardize 14 predictors for PCA |
| 15 | Explained variance, scree plot, cumulative variance |
| 16 | Component loadings and biplot |
| 17 | Stratified 80/20 train-test split |
| 18 | Logistic Regression (class-balanced) with accuracy, precision, recall, F1, ROC-AUC |
| 19 | Confusion matrix, ROC curve, 5-fold cross-validation |
| 20 | Coefficients and odds ratios |
| 21 | Logistic Regression on PCA components vs full feature set |

- Fig 10-A: Scree plot and cumulative explained variance
- Fig 10-B: PCA loadings heatmap (first 4 components)
- Fig 10-C: PCA biplot (PC1 vs PC2) colored by re-arrest status
- Fig 11-A: Confusion matrix and ROC curve
- Fig 11-B: Logistic Regression coefficients (standardized features)
- Fig 11-C: Model performance vs number of PCA components

### Results

| Item | Value |
|------|-------|
| Components for 80% / 90% variance | 9 / 11 |
| PC1 / PC2 variance explained | 18.1% / 13.5% |
| Logistic Regression accuracy | about 0.65 |
| Logistic Regression ROC-AUC | about 0.68 |
| Re-arrest recall | about 0.60 |

- PCA does not compress this dataset well because most predictors are binary flags, so variance is spread across many components.
- Age at release and felony conviction lower re-arrest risk. Prior convictions, property crime, time served and alcohol history raise it.
- Using all 14 original features performs as well or better than any reduced PCA set, which confirms there is little redundancy in the predictors.
- An AUC of about 0.68 is modest but stable across 5-fold cross-validation and is typical when the survival time variable is excluded.

---

## Tools Used
Python, Pandas, NumPy, Matplotlib, Seaborn, SciPy, Scikit-learn (KMeans, AgglomerativeClustering, PCA, LogisticRegression, StandardScaler), Google Colab

## Key Findings

- 38.2% of released prisoners were re-arrested within the follow-up window (median 74 weeks).
- Prior convictions and rule violations are the strongest drivers of shorter survival time.
- Marital status and alcohol history meaningfully affect re-arrest rate within each risk cluster.
- Clustering confirms two broad behavioral profiles: a smaller high-risk group and a larger low-risk group.
- Logistic Regression can predict re-arrest better than chance, with age, felony status and prior convictions among the most influential predictors.
