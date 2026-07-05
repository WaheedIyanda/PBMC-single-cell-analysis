# PBMC Single-Cell Analysis

This repository contains the workflow and analysis notebook for processing and analyzing Peripheral Blood Mononuclear Cells (PBMCs) from Myelofibrosis patients. 

## Dataset
The analysis includes three 10X Genomics datasets:
- **Fresh**: Fresh diseased PBMC sample
- **Fixed**: Fixed diseased PBMC sample
- **Fixed & Stored**: Fixed and stored diseased PBMC sample

*(Note: The large `.h5` data files are excluded from this repository via `.gitignore`.)*

## Workflow
1. **Data Loading**: Importing `.h5` feature barcode matrices using Scanpy.
2. **Preprocessing**: Handling duplicate gene names and formatting metadata.
3. **Merging**: Concatenating Fresh, Fixed, and Fixed_Stored samples into a single AnnData object.
4. *(More steps will be added as the analysis progresses...)*
