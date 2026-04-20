#  Single-Cell RNA-Seq Complete Pipeline

### Scanpy · AnnData · scverse

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python\&logoColor=white)
![Scanpy](https://img.shields.io/badge/Scanpy-1.9+-green)
![AnnData](https://img.shields.io/badge/AnnData-0.10+-orange)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

##  Overview

A complete, modular single-cell RNA sequencing (**scRNA-seq**) analysis pipeline built with **Scanpy** and **AnnData**.

The workflow is divided into three structured modules:

| # | Module                  | Description                                  |
| - | ----------------------- | -------------------------------------------- |
| 1 | Preprocessing           | QC, filtering, normalization, HVG selection  |
| 2 | Clustering & Annotation | PCA, UMAP, Leiden clustering, marker genes   |
| 3 | AnnData Exploration     | Deep dive into data structure & manipulation |

---

##  Repository Structure

```
scrna-complete-pipeline/
│
├── 1_Preprocessing/
│   ├── 01_preprocessing.ipynb
│   ├── data/
│   └── output_data/
│       └── preprocessed_adata.h5ad
│
├── 2_Basic_Tutorial/
│   ├── 02_clustering_and_umap.ipynb
│   └── output_data/
│
├── 3_AnnData/
│   ├── 03_anndata_exploration.ipynb
│   └── output_data/
│
├── README.md
└── requirements.txt
```

---

# 🔬 Module 1 — Pre-processing of scRNA-seq Data

 **Notebook:** `1_Preprocessing/01_preprocessing.ipynb`

##  Pipeline

### 🔹 Step 1 — Quality Control (QC)

Compute per-cell metrics:

* Number of genes per cell
* Total counts per cell
* Mitochondrial gene percentage (`pct_counts_mt`)

**Visualizations:**
Violin plots · Scatter plots

---

### 🔹 Step 2 — Filtering

Remove low-quality data:

* Cells with very few genes
* Genes expressed in very few cells
* Cells with high mitochondrial percentage

---

### 🔹 Step 3 — Normalization

```python
sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)
```

---

### 🔹 Step 4 — Highly Variable Genes (HVGs)

Select genes with highest biological variation.

---

### 🔹 Step 5 — Scaling & Regression

Remove technical noise and scale data.

---

### Outputs

* `preprocessed_adata.h5ad`
* QC plots (violin, scatter)

---

#  Module 2 — Clustering & Cell Type Annotation

📓 **Notebook:** `2_Basic_Tutorial/02_clustering_and_umap.ipynb`

##  Pipeline

### 🔹 Step 1 — PCA

Dimensionality reduction to capture major variance.

---

### 🔹 Step 2 — Neighborhood Graph

```python
sc.pp.neighbors(adata, n_neighbors=10, n_pcs=40)
```

---

### 🔹 Step 3 — UMAP Visualization

```python
sc.tl.umap(adata)
sc.pl.umap(adata, color=["leiden"])
```

---

### 🔹 Step 4 — Leiden Clustering

| Resolution | Granularity |
| ---------- | ----------- |
| 0.02       | Coarse      |
| 0.5        | Moderate    |
| 2.0        | Fine        |

---

### 🔹 Step 5 — Marker Gene Analysis

* Wilcoxon rank-sum test

```python
sc.pl.rank_genes_groups(adata)
```

---

### 🔹 Step 6 — Cell Type Annotation

| Cell Type    | Marker Genes |
| ------------ | ------------ |
| CD4+ T Cells | CD4, IL7R    |
| CD8+ T Cells | CD8A, CD8B   |
| B Cells      | MS4A1        |
| NK Cells     | GNLY, NKG7   |
| Monocytes    | CD14, FCN1   |

---

###  Outputs

* UMAP plots
* Cluster labels (`adata.obs`)
* Marker gene tables
* Annotated dataset

---

#  Module 3  AnnData: Structure & Exploration

📓 **Notebook:** `3_AnnData/03_anndata_exploration.ipynb`

##  Based on

* AnnData Official Tutorial
* scverse Getting Started Guide

---

##🧩 AnnData Structure

```
AnnData (n_obs × n_vars)

├── .X      → Expression matrix
├── .obs    → Cell metadata
├── .var    → Gene metadata
├── .uns    → Unstructured metadata
├── .obsm   → Embeddings (PCA, UMAP)
├── .varm   → Gene embeddings
├── .layers → Multiple data versions
└── .obsp   → Cell–cell relationships
```

---

##  Topics Covered

### 🔹 Core Structure

* `.X` → expression matrix
* `.obs` → cell metadata
* `.var` → gene metadata
* `.uns` → unstructured data

---

### 🔹 Advanced Components

* `.obsm` → embeddings
* `.layers` → raw / normalized data
* `.obsp` → distance matrices

---

### 🔹 Data Operations

```python
adata_bcells = adata[adata.obs.cell_type == "B cell"].copy()
adata_subset = adata[:5, ["GeneA", "GeneB"]]

adata.layers["counts"] = adata.X.copy()
adata.layers["log1p"]  = np.log1p(adata.X.toarray())
```

 Subsetting returns a view — use `.copy()` for independent data.

---

### 🔹 File Handling

```python
adata.write_h5ad("output_data/results.h5ad")
adata = ad.read_h5ad("output_data/results.h5ad")
```

---

###  Outputs

* Cell metadata tables
* UMAP coordinates
* `.h5ad` files

---

#  Tools & Libraries

| Library    | Purpose             |
| ---------- | ------------------- |
| scanpy     | scRNA-seq analysis  |
| anndata    | data structure      |
| numpy      | numerical computing |
| pandas     | metadata handling   |
| matplotlib | visualization       |
| scrublet   | doublet detection   |
| scipy      | sparse matrices     |

---

#  Installation

```bash
git clone https://github.com/SyedaMomina7/scrna-complete-pipeline.git
cd scrna-complete-pipeline

pip install -r requirements.txt
```

---

#  How to Run

```bash
cd 1_Preprocessing
colab notebook 01_preprocessing.ipynb

cd ../2_Basic_Tutorial
colab notebook 02_clustering_and_umap.ipynb

cd ../3_AnnData
colab notebook 03_anndata_exploration.ipynb
```

---

#  References

* Scanpy Documentation
* AnnData Documentation
* scverse Tutorials
* scRNA-seq Best Practices — Theis Lab

---

# 👩‍💻 Author

**Syeda Momina Assad**


