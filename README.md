# 🧬 Impact of Sample Processing on Single-Cell Transcriptomic Profiling of PBMC in Myelofibrosis

[![Python 3.12](https://img.shields.io/badge/python-3.12-blue.svg)](https://www.python.org/downloads/release/python-3120/)
[![Scanpy](https://img.shields.io/badge/Scanpy-1.12.2-green.svg)](https://scanpy.readthedocs.io/en/stable/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains the complete single-cell RNA sequencing (scRNA-seq) workflow and analysis notebook investigating how different sample processing and storage methods impact the transcriptomic profiling of Peripheral Blood Mononuclear Cells (PBMCs) from patients with Primary Myelofibrosis. 

## 🎯 Project Overview
Single-cell transcriptomics often relies on fresh tissue, but logistical challenges frequently necessitate cell fixation and storage. This project systematically evaluates the transcriptomic differences between three conditions:
1. **Fresh**: Immediate processing of diseased PBMC samples.
2. **Fixed**: Formaldehyde-fixed diseased PBMC samples.
3. **Fixed & Stored**: Formaldehyde-fixed and long-term stored diseased PBMC samples.

By comparing these conditions, we aim to provide guidelines on the validity and potential biases introduced by fixation and storage in clinical scRNA-seq studies.

## 📊 Dataset
The analysis is performed on three 10X Genomics 3' scRNA-seq datasets (GRCh38 reference). *(Note: The large `.h5` data files are excluded from this repository via `.gitignore`.)*

| Condition | Cells Captured | % of Total | Genes |
|-----------|---------------|------------|-------|
| Fresh | 12,516 | 41.9% | 38,606 |
| Fixed | 11,538 | 38.6% | 38,606 |
| Fixed & Stored | 5,825 | 19.5% | 38,606 |
| **Total (merged)** | **29,879** | **100%** | **38,606** |

## 🔍 Preliminary Findings

### Cell Recovery
- **Fixation is relatively gentle**: Only ~8% cell loss from Fresh to Fixed (12,516 → 11,538 cells).
- **Storage is destructive**: ~50% cell loss from Fixed to Fixed & Stored (11,538 → 5,825 cells).
- This cell loss may not be random — fragile cell types may degrade preferentially, introducing compositional bias.

### Gene Space
All three datasets share the identical gene set (38,606 genes), confirming consistent Cell Ranger processing. Most genes are zeros in any given cell (~95% sparsity), which is expected in scRNA-seq.

### Quality Control Metrics
- **Mitochondrial %** (`pct_counts_mt`): Indicator of cell damage — when a cell dies, cytoplasmic RNA leaks out while mitochondrial RNA is retained, resulting in artificially high mt%.
- **Ribosomal %** (`pct_counts_ribo`): Shifts between conditions suggest fixation differentially preserves certain RNA species.
- **Genes per cell** (`n_genes_by_counts`): Used to identify empty droplets (< 200 genes) and doublets (> 4,000 genes).

### Filtering Applied

| Filter | Threshold | Rationale |
|--------|-----------|-----------|
| Min genes/cell | ≥ 200 | Remove empty droplets |
| Max genes/cell | < 4,000 | Remove potential doublets |
| Max MT % | < 15% | Remove dead/dying cells |
| Min cells/gene | ≥ 3 | Remove noise genes |

## ⚙️ Analysis Workflow
The core analysis is documented in `sc_analysis_Notebook_.ipynb` and currently includes:
- [x] **Data Ingestion**: Loading `.h5` feature barcode matrices via `Scanpy`.
- [x] **Preprocessing**: Handling duplicate gene names and unifying metadata.
- [x] **Merging**: Concatenating samples into a unified `AnnData` object for batch analysis.
- [x] **Quality Control (QC)**: Calculating and visualizing mitochondrial/ribosomal metrics.
- [x] **Filtering**: Removing empty droplets, potential doublets, and low-quality/dead cells.
- [x] **Normalization & Scaling**: Storing the log-normalized data in `.raw` and scaling.
- [x] **Dimensionality Reduction (PCA)**: Running PCA and inspecting elbow plots.
- [x] **Dimensionality Reduction (UMAP)**: Computing neighbor graphs and visualizing in 2D.
- [x] **Clustering & Cell Type Annotation**: Clustering cells via Leiden and identifying marker genes.
- [x] **Differential Expression Analysis (Condition vs. Condition)**: Evaluating gene changes between fresh, fixed, and stored conditions.
- [ ] **ML Classification**: Predict processing condition from gene expression
- [ ] **Statistical Testing**: Cell type composition changes, Shannon entropy, chi-squared tests

## 🚀 Getting Started
To run this notebook locally or in Google Colab:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/WaheedIyanda/PBMC-single-cell-analysis.git
   ```
2. **Install dependencies:**
   This project relies heavily on the `scanpy` ecosystem. 
   ```bash
   pip install 'scanpy[leiden]' 'pandas<3.0'
   ```
3. **Download Data:**
   You will need to acquire the corresponding `.h5` files and place them in your local directory (or mount them via Google Drive if using Colab) and update the `path` variable in the notebook.


## ⭐️ Show your support
Give a ⭐️ if this project helped you!
