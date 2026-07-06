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
The analysis is performed on three 10X Genomics 3' scRNA-seq datasets. *(Note: The large `.h5` data files are excluded from this repository via `.gitignore`.)*

* **Total Cells Processed**: ~29,879 cells
* **Genes Tracked**: 38,606 genes

## ⚙️ Analysis Workflow
The core analysis is documented in `sc_analysis_Notebook_.ipynb` and currently includes:
- [x] **Data Ingestion**: Loading `.h5` feature barcode matrices via `Scanpy`.
- [x] **Preprocessing**: Handling duplicate gene names and unifying metadata.
- [x] **Merging**: Concatenating samples into a unified `AnnData` object for batch analysis.
- [x] **Quality Control (QC)**: Calculating and visualizing mitochondrial/ribosomal metrics.
- [x] **Filtering**: Removing empty droplets, potential doublets, and low-quality/dead cells.
- [ ] **Normalization & Scaling** *(Upcoming)*
- [ ] **Dimensionality Reduction (PCA & UMAP)** *(Upcoming)*
- [ ] **Clustering & Cell Type Annotation** *(Upcoming)*
- [ ] **Differential Expression Analysis (Condition vs. Condition)** *(Upcoming)*

## 🚀 Getting Started
To run this notebook locally or in Google Colab:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/WaheedIyanda/PBMC-single-cell-analysis.git
   ```
2. **Install dependencies:**
   This project relies heavily on the `scanpy` ecosystem. 
   ```bash
   pip install 'scanpy[leiden]'
   ```
3. **Download Data:**
   You will need to acquire the corresponding `.h5` files and place them in your local directory (or mount them via Google Drive if using Colab) and update the `path` variable in the notebook.

## 🤝 Contributing
Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](../../issues).

## ⭐️ Show your support
Give a ⭐️ if this project helped you!
