# Gut Microbiome and Metabolome Signatures in Colorectal Cancer

**Primary data source:** Yachida et al. (2019)  
**Validation data source:** Wirbel et al. (2019)  
**Omics layers:** Gut metagenomics and fecal metabolomics  
**Primary cohort:** 347 paired samples  

## Project Overview

This project integrates gut metagenomic and metabolomic data to investigate whether colorectal neoplasia can be distinguished from healthy controls using statistical analysis, multi-omics integration, and machine learning.

The primary analysis uses the Yachida et al. (2019) cohort, which contains paired whole-genome shotgun metagenomic and CE-TOFMS metabolomic profiles. The analysis includes species-level, genus-level, and metabolite features from 347 paired samples.

The project compares Random Forest, LASSO logistic regression, and XGBoost models across stage-specific classification tasks. MOFA+ is used for unsupervised multi-omics integration, while SHAP values are used to interpret important microbial and metabolic features.

External validation is performed using the independent Wirbel et al. (2019) colorectal cancer microbiome cohort. Taxonomy harmonization, token-based feature matching, and robust centered log-ratio transformation are used to address differences between the Yachida and Wirbel datasets.

## Repository Structure

```text
.
├── data/
│   ├── primary_data/                    # Yachida cohort input data
│   │   ├── metadata.tsv                 # Clinical metadata
│   │   ├── genera.tsv                   # Genus-level relative abundances
│   │   ├── genera.counts.tsv            # Genus-level count matrix
│   │   ├── species.tsv.zip              # Species-level relative abundances
│   │   ├── species.counts.tsv           # Species-level count matrix
│   │   ├── mtb.tsv                      # Metabolite abundance matrix
│   │   └── mtb.map.tsv                  # Metabolite annotation table
│   │
│   ├── validation_data/                 # Wirbel external validation data
│   │   ├── Wirbel_Validation_Aligned_Ready.csv
│   │   ├── metadata_ST6.xlsx            #(Clinical & Demographic Metadata) contains the comprehensive patient metadata and clinical information for all individuals in the cohort.
│   │    ── Taxonomic_profiles_SD1.xlsx   #(Microbial Abundance Matrix)stores the high-throughput metagenomic abundance matrix for the gut microbiome across all processed samples.
│   │   
│   │  
│   │
│   └── README.md                        # Data source and file descriptions
│
├── notebooks/
│   ├── 01_data_preparation.ipynb        # Loading, matching, QC, and preprocessing
│   ├── 02_exploratory_analysis.ipynb    # PCA, UMAP, metadata distributions
│   ├── 03_statistical_analysis.ipynb    # Mann–Whitney, FDR, Spearman correlation
│   ├── 04_baseline_models.ipynb         # Random Forest and LASSO
│   ├── 05_mofa_xgboost_shap.ipynb       # MOFA+, XGBoost, late integration, SHAP
│   └── 06_cross_cohort_validation.ipynb # Yachida-to-Wirbel external validation
│
├── figures/
│   ├── eda/                             # PCA, UMAP, metadata and QC plots
│   ├── statistics/                      # Volcano, violin, and correlation plots
│   ├── machine_learning/                # ROC curves and model comparisons
│   ├── shap/                            # SHAP summary and feature plots
│   ├── pathway_annotation/              # KEGG enrichment plots
│   └── validation/                      # External-validation ROC curves
│
├── results/
│   ├── differential_abundance/          # Significant taxa and FDR results
│   ├── correlations/                    # Genus–metabolite correlations
│   ├── model_performance/               # Cross-validation and holdout metrics
│   ├── mofa/                            # MOFA factors and variance explained
│   ├── shap/                            # Ranked important features
│   └── validation/                      # Wirbel validation outputs
│
├── presentation/
│   └── final_presentation.pdf
│
├── requirements.txt
└── README.md
```

> The notebook and file names above should be changed to match the files actually uploaded to the repository.

## How to Reproduce

### 1. Environment

Create and activate a Python virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
```

On Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```

Alternatively:

```bash
pip install numpy pandas scipy scikit-learn matplotlib seaborn \
umap-learn xgboost shap mofapy2 openpyxl jupyter
```

### 2. Obtain the data

The processed Yachida et al. (2019) abundance matrices and metadata are publicly available from the curated microbiome–metabolome data repository.

The Wirbel et al. (2019) validation data were obtained from the study resources and processed for taxonomy alignment.

Large source files may not be committed to the repository. See `data/README.md` for data-access instructions and file descriptions.

### 3. Run data preparation

Open and run:

```text
notebooks/01_data_preparation.ipynb
```

This notebook performs:

- sample-ID matching
- paired-sample selection
- missing-value checks
- duplicate checks
- prevalence filtering
- zero-variance filtering
- log transformation
- scaling where required

### 4. Run exploratory data analysis

Open and run:

```text
notebooks/02_exploratory_analysis.ipynb
```

Outputs include:

- disease-group distributions
- age, sex, and BMI comparisons
- PCA plots
- UMAP plots
- outlier assessment

### 5. Run statistical analysis

Open and run:

```text
notebooks/03_statistical_analysis.ipynb
```

This notebook includes:

- Mann–Whitney U testing
- Benjamini–Hochberg FDR correction
- log2 fold-change calculation
- volcano plots
- violin plots
- Spearman genus–metabolite correlations
- hierarchical clustering of correlation patterns

### 6. Run baseline models

Open and run:

```text
notebooks/04_baseline_models.ipynb
```

The baseline analysis compares:

- Random Forest
- LASSO logistic regression
- single-omics feature sets
- combined feature sets
- stage-specific classification tasks

### 7. Run MOFA+, XGBoost, and SHAP analysis

Open and run:

```text
notebooks/05_mofa_xgboost_shap.ipynb
```

This notebook performs:

- MOFA+ multi-omics integration
- extraction of latent factors
- XGBoost classification
- late integration
- stratified cross-validation
- ROC-AUC evaluation
- SHAP-based feature interpretation
- KEGG pathway annotation

### 8. Run cross-cohort validation

Open and run:

```text
notebooks/06_cross_cohort_validation.ipynb
```

The validation workflow includes:

- taxonomy harmonization
- token-based feature matching
- removal of incompatible taxonomic prefixes
- shared-feature selection
- robust centered log-ratio transformation
- training on Yachida
- testing on Wirbel

## Key Results

### Statistical Analysis

Four genera remained significant after Benjamini–Hochberg FDR correction:

- `Parvimonas`
- `RGIG1874`
- `Lonepinella`
- `Lachnospira`

`Parvimonas`, `RGIG1874`, and `Lonepinella` were enriched in colorectal neoplasia, whereas `Lachnospira` was enriched in healthy controls.

Spearman correlation analysis identified distinct microbial–metabolite association patterns:

- `Parvimonas` showed predominantly positive correlations with metabolites.
- `RGIG1874` showed mainly negative correlations.
- `Lachnospira` showed mixed positive and negative correlations.
- `Lonepinella` showed comparatively fewer strong associations.

### Machine-Learning Results

#### Healthy vs Stage III/IV

| Model | Feature set | ROC-AUC |
|---|---|---:|
| XGBoost | Genera | 0.849 |
| XGBoost | Metabolomics | 0.736 |
| Late integration | Genera + metabolomics | 0.842 |
| XGBoost | MOFA+ factors | 0.701 |

The genus-level XGBoost model achieved the best later-stage classification performance.

#### Early-Stage Classification

Late integration of genus and metabolomics predictions achieved a ROC-AUC of approximately:

```text
0.740
```

### MOFA+ Results

MOFA+ identified latent factors representing variation across the microbiome and metabolome. However, the factors showed limited disease-group separation and lower predictive performance than models trained directly on the original features.

This suggests that the dominant shared variation captured by the unsupervised model was not strongly aligned with disease status.

### SHAP and Biological Interpretation

Important recurrent features included:

- `Parvimonas`
- `Fusobacterium`
- `Leptotrichia`
- `N1-Acetylspermine`
- `2,5-Dihydroxybenzoate`
- leucine and other amino-acid-related metabolites

These findings support biological links between CRC, oral-associated anaerobic bacteria, and altered amino-acid metabolism.

### Cross-Cohort Validation

Alignment results for core feature token matching

Successful alignment of early core features (Top 25):3 
Successful alignment of key late-stage characteristics (Top 25): 7 

[Early Stage] Specific core species successfully aligned:
1. Alloprevotella tannerae
2. Streptococcus australis_B
3. Peptostreptococcus stomatis

[Late stage] Specific core species successfully aligned:
1. Gemella morbillorum
2. Peptostreptococcus stomatis
3. Fusobacterium nucleatum_J
4. Parvimonas micra
5. Enterococcus_B hirae
6. Streptococcus mutans
7. Holdemania massiliensis

External Validation:
Models trained on the Yachida cohort were evaluated on the Wirbel cohort.

| Task | Random Forest AUC | XGBoost AUC |
|---|---:|---:|
| Early-stage 3 signatures | 0.5456 | 0.5487 |
| Late-stage 7 signatures | 0.7075 | 0.7108 |

Feature(internal) Validation:
Models trained and testes only on the Wirbel cohort.

| Task | Random Forest AUC | XGBoost AUC |
|---|---:|---:|
| Early-stage 3 signatures | 0.651178 | 0.655562 |
| Late-stage 7 signatures | 0.744841	 | 0.732610 |

External validation performance was substantially lower than internal performance.

The results indicate limited transferability across cohorts and highlight the effects of:

- taxonomic naming differences
- GTDB versus mOTU annotation
- sequencing-depth differences
- laboratory protocols
- population heterogeneity
- batch and covariate shift
- differences in abundance distributions

The weak external performance is treated as an important scientific result rather than only a model failure.

## Data Availability

### Yachida cohort

Processed abundance matrices and metadata are publicly available from the curated dataset repository associated with:

Yachida, S. et al. (2019).  
*Metagenomic and metabolomic analyses reveal distinct stage-specific phenotypes of the gut microbiota in colorectal cancer.*  
Nature Medicine, 25, 968–976.

### Wirbel cohort

The external validation cohort is based on:

Wirbel, J. et al. (2019).  
*Meta-analysis of fecal metagenomes reveals global microbial signatures that are specific for colorectal cancer.*  
Nature Medicine, 25, 679–689.

Large raw and functional data files are not necessarily stored in this repository. Processed files required for the reported analyses are included where file size and licensing permit.

## Dependencies

| Package | Purpose |
|---|---|
| `numpy` | Numerical array operations |
| `pandas` | Data loading and manipulation |
| `scipy` | Statistical testing |
| `scikit-learn` | Preprocessing, cross-validation, and baseline models |
| `matplotlib` | Data visualization |
| `seaborn` | Statistical plots and heatmaps |
| `umap-learn` | UMAP dimensionality reduction |
| `xgboost` | Gradient-boosting classification |
| `shap` | Model interpretation |
| `mofapy2` | MOFA+ integration |
| `openpyxl` | Reading Excel validation files |
| `jupyter` | Notebook execution |

## Authors

**Group 6**

- Xiaolin Lai
- Jaskirat Kaur Sohal
- Jiaqi Li

## AI Assistance Disclosure

AI assistance was used for:

- code compilation and organization
- debugging and resolving technical errors
- improving code readability and documentation
- README preparation
- background explanations of statistical and machine-learning methods

All scientific decisions, preprocessing choices, experimental design, model evaluation, biological interpretation, and conclusions were made and critically reviewed by the project team. AI-generated suggestions were checked and corrected where necessary.