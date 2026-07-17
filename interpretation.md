# Interpretation of Results So Far

## What the Data Tells Us

---

### The Cell Counts — The First Big Finding

| Condition | Cells Captured | % of Total |
|-----------|---------------|------------|
| Fresh | 12,516 | 41.9% |
| Fixed | 11,538 | 38.6% |
| Fixed_Stored | 5,825 | 19.5% |

**What this means:**
- **Fresh → Fixed**: Only a small drop (~8% fewer cells). Fixation itself doesn't destroy many cells. This is good news — it means fixation is relatively gentle on PBMCs.
- **Fixed → Fixed_Stored**: A dramatic **50% loss** of cells. Long-term storage after fixation is destructive. Half the cells are either destroyed, damaged beyond detection, or can no longer be captured by the 10x Chromium system.
- **The takeaway**: If you must fix samples, process them soon. Storage is the real killer, not fixation itself.

> [!IMPORTANT]
> This cell loss is NOT random. Some cell types are more fragile than others (e.g., granulocytes, monocytes degrade faster than lymphocytes). This means Fixed_Stored samples may have a **biased** representation of cell types — you're not just losing cells, you're losing *specific* cell types preferentially.

---

### 38,606 Genes — What This Number Means

All three datasets have exactly **38,606 genes** because they were all aligned to the same human reference genome (**GRCh38**) using 10x Genomics Cell Ranger. This number includes:
- ~20,000 protein-coding genes
- ~18,000 non-coding genes (lncRNAs, pseudogenes, etc.)

**Most of these genes are zeros.** In single-cell RNA-seq, a typical cell only expresses 1,000–4,000 genes. The rest are recorded as zero counts. This extreme sparsity (~90–95% zeros) is normal and expected — it's called the **dropout problem** in scRNA-seq.

---

### QC Metrics — What Each One Reveals

#### `n_genes_by_counts` (genes detected per cell)
- **What it measures**: How many unique genes were detected in each cell
- **Healthy range**: 200 – 4,000 for PBMCs
- **Too low (< 200)**: The "cell" is probably an empty droplet (just ambient RNA, no real cell inside)
- **Too high (> 4,000)**: Likely a **doublet** — two cells captured in one droplet, so you see twice as many genes
- **What to watch for between conditions**: If Fixed_Stored cells show fewer genes per cell on average, it means fixation + storage causes RNA degradation (genes are lost)

#### `total_counts` (total UMI counts per cell)
- **What it measures**: Total number of RNA molecules detected per cell
- **What it means**: Higher = more RNA captured = better quality library
- **Between conditions**: Fresh cells typically have the highest counts. A drop in Fixed/Stored suggests RNA is degrading during processing

#### `pct_counts_mt` (mitochondrial %)
- **What it measures**: Percentage of a cell's RNA coming from mitochondrial genes (MT-ND1, MT-CO1, MT-CYB, etc.)
- **Why it matters**: When a cell is **dying or damaged**, its cell membrane breaks, and cytoplasmic mRNA leaks out. But mitochondrial RNA is protected inside the mitochondria. So dying cells show **high mt%** because they've lost their cytoplasmic RNA but retained mitochondrial RNA.
- **Threshold < 15%**: Cells above this are likely dead or dying
- **Between conditions**: If Fixed or Stored samples show higher mt% overall, it confirms that processing damages cell membranes

#### `pct_counts_ribo` (ribosomal %)
- **What it measures**: Percentage of RNA from ribosomal protein genes (RPS, RPL)
- **Why it matters**: Ribosomal genes are very highly expressed and stable. A shift in ribosomal % can indicate:
  - **High ribo%**: Could mean other RNA has degraded, leaving ribosomal RNA as a larger proportion
  - **Different ribo%** across conditions: Suggests fixation differentially preserves certain RNA types

---

### The Filtering Step — What We Removed and Why

```
Before filtering: 29,879 cells
After filtering:  ~XX,XXX cells (run your notebook to see exact number)
```

| Filter | What it removes | Why |
|--------|----------------|-----|
| `min_genes ≥ 200` | Empty droplets | Droplets with < 200 genes have no real cell — just ambient RNA floating in the solution |
| `n_genes < 4,000` | Doublets | Two cells stuck together look like one "super cell" with too many genes |
| `pct_counts_mt < 15%` | Dead/dying cells | High mitochondrial % = cell membrane is broken, RNA leaked out |
| `min_cells ≥ 3` | Noise genes | Genes detected in fewer than 3 cells are unreliable — could be sequencing errors |

> [!NOTE]
> **Filtering is the most subjective step in scRNA-seq.** The thresholds (200, 4000, 15%) are starting points. Ideally you adjust them based on the violin and scatter plots. Too strict = you lose real cells. Too lenient = noise pollutes downstream analysis.

---

### The Bigger Picture — What It All Means for the Research Question

Your project is asking: *"Can I trust data from fixed/stored samples as much as fresh?"*

**What the data so far suggests:**

1. **Cell recovery drops with storage** — Fixed_Stored captures only half the cells. This alone means stored samples give you less statistical power and potentially biased cell type representation.

2. **The gene space is preserved** — All conditions detect the same set of genes. Fixation doesn't make genes invisible; it may just reduce their counts.

3. **QC distributions differ by condition** — The violin and scatter plots (when you examine them) will show whether mt%, gene counts, and total counts shift between conditions. These shifts quantify the *degree* of processing damage.

4. **Filtering removes different amounts per condition** — If Fixed_Stored loses a higher proportion of cells to QC filtering, it confirms those cells are lower quality.

---

### What Comes Next — Why It Matters

The completed work answers: **"How much data do we lose?"**

The upcoming analysis will answer:
- **"What kind of data do we lose?"** → Cell type annotation will reveal if specific cell types disappear
- **"Can a machine detect the damage?"** → ML classifier will show if fixation leaves a detectable molecular fingerprint
- **"How bad is the distortion?"** → Statistical tests will quantify whether biological conclusions change depending on processing method
