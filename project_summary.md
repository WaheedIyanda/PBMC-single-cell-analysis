# Summary of Work Completed

## Project
**Impact of Sample Processing (Fresh vs. Fixed vs. Fixed-Stored) on Single-Cell Transcriptomic Profiling of Diseased PBMCs in Primary Myelofibrosis**

---

## Objective
To systematically evaluate how sample processing and storage methods — specifically fresh processing, formaldehyde fixation, and fixation with long-term storage — affect the single-cell transcriptomic landscape of Peripheral Blood Mononuclear Cells (PBMCs) from a Myelofibrosis patient.

---

## Dataset
Three 10x Genomics 3' Chromium scRNA-seq datasets from the same donor (diseased PBMCs, Primary Myelofibrosis), differing only in sample handling:

| Condition | Cells | Genes | Description |
|-----------|-------|-------|-------------|
| **Fresh** | 12,516 | 38,606 | Immediate processing, no fixation |
| **Fixed** | 11,538 | 38,606 | Formaldehyde-fixed |
| **Fixed_Stored** | 5,825 | 38,606 | Formaldehyde-fixed + long-term storage |
| **Total (merged)** | **29,879** | **38,606** | Combined for comparative analysis |

> [!NOTE]
> All three datasets share identical gene spaces (38,606 genes, GRCh38 reference), confirming they were processed with the same 10x Genomics Cell Ranger pipeline.

---

## Completed Work

### 1. Environment Setup & Data Loading
- **Platform**: Google Colab with Python 3.12
- **Key libraries**: `scanpy 1.12.2`, `anndata`, `pandas`, `numpy`, `matplotlib`, `seaborn`
- **Dependency fix**: Resolved a `pandas`/`anndata` version incompatibility (`StringDtype.__init__()` error) by ensuring compatible package versions
- Loaded three `.h5` filtered feature-barcode matrices using `sc.read_10x_h5()`
- Mounted Google Drive for data access

### 2. Preprocessing
- Made variable (gene) names unique across all datasets using `var_names_make_unique()` to handle duplicate gene symbols
- Assigned condition labels (`Fresh`, `Fixed`, `Fixed_Stored`) to each dataset's `.obs` metadata
- Made observation (cell barcode) names unique to prevent index collisions during merging

### 3. Data Merging
- Concatenated all three datasets into a single unified `AnnData` object using `ad.concat()` with:
  - `join='inner'` — keeping only genes present in all three datasets
  - `index_unique='-'` — appending suffixes to disambiguate duplicate barcodes
- Verified merge integrity: **29,879 cells × 38,606 genes**
- Confirmed cell counts per condition match expected values

### 4. Quality Control (QC) Metrics
- Annotated **mitochondrial genes** (`MT-` prefix) and **ribosomal genes** (`RPS`/`RPL` prefix) in `adata.var`
- Computed per-cell QC metrics using `sc.pp.calculate_qc_metrics()`:
  - `n_genes_by_counts` — number of genes detected per cell
  - `total_counts` — total UMI counts per cell
  - `pct_counts_mt` — percentage of counts from mitochondrial genes
  - `pct_counts_ribo` — percentage of counts from ribosomal genes

### 5. QC Visualization
- Generated **violin plots** of all QC metrics grouped by condition, enabling visual comparison of data quality across processing methods
- Generated **scatter plots**:
  - Total counts vs. mitochondrial percentage (identifies dying cells)
  - Total counts vs. genes detected (identifies doublets and empty droplets)

### 6. Cell & Gene Filtering
Applied standard scRNA-seq quality filters:

| Filter | Threshold | Rationale |
|--------|-----------|-----------|
| Minimum genes per cell | ≥ 200 | Remove empty droplets / debris |
| Maximum genes per cell | < 4,000 | Remove potential doublets |
| Mitochondrial % | < 15% | Remove dead / dying cells |
| Minimum cells per gene | ≥ 3 | Remove rarely expressed genes (noise) |

---

## Tools & Technologies Used
- **Scanpy** — single-cell analysis framework (loading, QC, filtering)
- **AnnData** — data structure for annotated matrices
- **Pandas / NumPy** — data manipulation
- **Matplotlib / Seaborn** — visualization
- **Google Colab** — compute environment

---

## What's Next
The following analyses are planned to complete the project:
- [ ] Normalization & highly variable gene selection
- [ ] Dimensionality reduction (PCA, UMAP)
- [ ] Unsupervised clustering (Leiden algorithm)
- [ ] Cell type annotation using canonical PBMC markers
- [ ] Differential expression analysis between conditions
- [ ] ML classification to predict processing condition from gene expression
- [ ] Statistical testing of cell type composition changes
