# AI Usage

This project used **Claude (Anthropic)** as an AI assistant. Course policy allows AI use as long as it is documented. This file records what the AI did, what we did, and how the AI's output was checked.

## What the AI was used for

| Stage | AI contribution | Our role |
|---|---|---|
| Topic selection | Suggested project directions; checked the class sheet for overlap with other students' topics | Chose the topic |
| Literature and data | Found the Schurch et al. (2016) dataset, its GitHub count files, and the Python packages (PyDESeq2, InMoose) | Read the papers; confirmed the data source |
| Proposal | Drafted the 2-page proposal in the course template | Reviewed and edited before submission |
| Code | Wrote the Colab notebook: data loading, the four DE methods, TMM normalization, the subsampling loop, metrics and figures | Ran the notebook, read the code, checked the outputs (below) |
| Revision after instructor feedback | Added the validation pilot and gate, result caching, and the "reference / disagreement" terminology | Reviewed the changes; confirmed the cached benchmark reproduces the earlier results exactly |
| Debugging | Fixed InMoose API issues (exact-test output columns, limma `topTable` coefficient lookup) and the R fold-change sign convention | Re-ran to confirm the fixes |
| Reports | Drafted the draft and final report text from the generated results | Reviewed every number against the CSV outputs |

## How AI output was verified

0. **Against an independent implementation.** The TMM normalization we implemented was checked against the `conorm` package on the full data and on pilot subsets of 3, 6 and 20 replicates (max difference 0.04%). This check is built into the notebook as a validation gate that must pass before the benchmark runs.
1. **Against published results.** On the full data, the Python tools agree closely with the original R results (Jaccard 0.95–0.98, Spearman ρ of log2 fold changes 0.986–1.000). This shows the code reproduces the established methods.
2. **Against the paper's findings.** Recall with 3 replicates (30–34% for PyDESeq2, edgeR and limma) is consistent with the 20–40% reported by Schurch et al. (2016).
3. **Against theory.** With 3 vs 3 samples, the Wilcoxon test cannot reach a two-sided p-value below 0.1, so finding zero genes is the expected result, not a bug.
4. **Sanity checks.** Library sizes, the gene count after filtering (6,887), and the sample counts (42 WT, 44 Snf2) match the dataset documentation.
5. **Reproducibility.** Fixed per-draw seeds. Running the notebook from scratch produces identical tables.

## Ethics and data privacy

The data is public, non-human (yeast) RNA-seq. There is no personal or sensitive information, and no unpublished lab data was uploaded to any AI tool.

## Prompt log

Key prompts (summarized):

- "Help me with my BIOL 5340 final project; templates attached" → topic options, then the proposal draft.
- "Give the code to run in Colab, the data sources, and explain the project simply" → the notebook and an explanation.
- "Is this project related to data science?" → mapping of project steps to data-science skills.
- "Make the proposal and report documents using the templates" → the report drafts.
