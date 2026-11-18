# Benchmarking Python Differential Expression Tools Across Replicate Numbers

BIOL 5340 (Fall 2026) final project, by Allan Thiyagaseelan and Habib Wouleo Fipa.

**Question:** As the number of biological replicates per condition drops from more than 40 to 3, what fraction of the truly differentially expressed (DE) genes do four Python DE methods still recover, and how well do they agree?

**Methods compared:** PyDESeq2 · edgeR (InMoose port) · limma-trend (InMoose port) · Wilcoxon rank-sum test

**Data:** Yeast (*S. cerevisiae*) wild type vs. Δ*snf2*, 48 biological replicates per condition (Schurch et al., 2016; ENA PRJEB5348). The notebook downloads the gene count tables and the authors' original R results automatically from <https://github.com/bartongroup/profDGE48>.

## Repository contents

| Path | What it is |
|---|---|
| `DE_Benchmark_Colab.ipynb` | The complete analysis in three stages: (1) validation pilot (must pass before anything else runs), (2) cached subsampling benchmark, (3) evaluation figures and tables |
| `results/` | Output written by the notebook (CSV tables and PNG figures, listed below) |
| `cache/` | Created when the notebook runs: the raw DE results of every random draw (one `.csv.gz` per draw), reused instead of recomputed. Not committed (about 30 MB). |
| `reports/` | Proposal and draft report |
| `ai_usage.md` | How AI tools were used and how their output was checked |

### Files in `results/`

| File | Content |
|---|---|
| `count_matrix_clean.csv` | 6,887 genes × 86 samples after removing the 10 flagged replicates |
| `table0_validation.csv` | Validation pilot: TMM vs. conorm, Python vs. published R, pilot draws (all must pass) |
| `full_<method>.csv` | Full-data **reference** lists (42 vs 44): log2 fold change and adjusted p-value for every gene (cached) |
| `subsample_metrics.csv` | One row per method per random draw: n, number of DE genes, recall vs. reference, disagreement with the consensus reference, effect-size recall, pairwise Jaccard |
| `table1_python_vs_R.csv` | Agreement between each Python tool and the published R result |
| `table2_summary_by_n.csv` | Mean metrics per method and replicate number |
| `table3_min_replicates.csv` | Minimum replicates per method to reach 50% / 80% recall |
| `fig0`–`fig5` PNGs | Library sizes, Python vs R, UpSet overlap, recall vs n, recall by effect size, agreement heatmaps |

## Terminology

- **Reference:** a method's DE list on the full data (42 vs 44 replicates). It is the best available estimate, not biological ground truth.
- **Consensus reference:** genes called DE by at least 3 of the 4 methods on the full data.
- **Recall:** the share of the reference recovered by a smaller experiment.
- **Disagreement:** the share of a method's calls that fall outside the consensus reference. It measures disagreement with the reference, not proven biological errors.

## How to run

**Google Colab (recommended):** upload `DE_Benchmark_Colab.ipynb`, then Runtime → Run all.

- `QUICK_MODE = True`: 5 draws per sample size, about 15–20 min.
- `QUICK_MODE = False`: 20 draws, the setting used for the report, about 1–1.5 h.

Progress is saved after every draw, so a disconnected session resumes where it stopped. When the run finishes, download `results.zip`.

**Locally:** use Python 3.11 or 3.12.

```bash
pip install pydeseq2==0.5.4 inmoose==0.9.1 upsetplot pandas numpy scipy matplotlib seaborn jupyter
jupyter nbconvert --to notebook --execute DE_Benchmark_Colab.ipynb
```

## Reproducibility notes

- **Validation gate:** before the benchmark, the notebook checks (A) the TMM normalization against the independent `conorm` package (factors must agree within 0.5%), (B) the full-data Python results against the published R 3.2.2 results (Jaccard ≥ 0.90, Spearman ρ ≥ 0.95), and (C) two pilot draws end to end. If any check fails, it stops.
- **Caching:** full-data references, raw per-draw results and metrics are all cached, so a re-run only computes what is missing (a complete re-run from cache takes seconds).

- Every random draw uses its own fixed seed (`[2026, n, draw]`), so results are identical across runs and machines.
- All methods use Benjamini–Hochberg FDR < 0.05.
- InMoose 0.9.1 has no `calcNormFactors` or `voom`. TMM normalization is implemented in the notebook following edgeR's algorithm. limma is run as limma-trend (log-CPM + `eBayes(trend=True)`) instead of voom.
- The R reference results are from R 3.2.2 (Schurch et al., 2016). The R edgeR and limma files report WT vs Snf2, so their fold changes are sign-flipped before comparison.
