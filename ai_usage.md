# AI Usage

We used Claude (Anthropic) as an assistant during this project.

## What the AI was used for

Topic and literature: Claude suggested possible project directions. It also helped us find the Schurch et al. (2016) dataset, its count files on GitHub, and the Python packages we used (PyDESeq2 and InMoose).
Understanding the biology: As data science students, we used Claude to explain biology we were not familiar with, such as what RNA-seq measures, why biological replicates matter, what the snf2 mutation does, and how the DE tools differ.
Proposal and reports: We planned the project and wrote the reports ourselves, and used Claude to help check the reports against the course template and rubric. We reviewed every section and checked every number against our results files.
Code: We worked through the notebook to understand each step and compared its results with published values. Claude wrote the parts that needed methods we did not know well, such as the TMM normalization, the InMoose edgeR and limma calls, and the validation checks added after the professor’s feedback.
Debugging: Claude helped fix errors, including InMoose function issues, the sign flip needed to compare with the R results, and a package version problem in Colab.

## How AI output was verified

0. **Against an independent implementation.** The TMM normalization we implemented was checked against the `conorm` package on the full data and on pilot subsets of 3, 6 and 20 replicates (max difference 0.04%). This check is built into the notebook as a validation gate that must pass before the benchmark runs.
1. **Against published results.** On the full data, the Python tools agree closely with the original R results (Jaccard 0.95–0.98, Spearman ρ of log2 fold changes 0.986–1.000). This shows the code reproduces the established methods.
2. **Against the paper's findings.** Recall with 3 replicates (30–34% for PyDESeq2, edgeR and limma) is consistent with the 20–40% reported by Schurch et al. (2016).
3. **Against theory.** With 3 vs 3 samples, the Wilcoxon test cannot reach a two-sided p-value below 0.1, so finding zero genes is the expected result, not a bug.
4. **Sanity checks.** Library sizes, the gene count after filtering (6,887), and the sample counts (42 WT, 44 Snf2) match the dataset documentation.
5. **Reproducibility.** Fixed per-draw seeds. Running the notebook from scratch produces identical tables.

## Ethics and data privacy

The data is public, non-human (yeast) RNA-seq. There is no personal or sensitive information, and no unpublished lab data was uploaded to any AI tool.


