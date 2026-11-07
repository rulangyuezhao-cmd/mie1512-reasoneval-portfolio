# Project notebooks

The three clean notebooks are the recommended reading order. The two versions with saved outputs preserve the visual record of the course project.

| Notebook | Purpose | Run |
| --- | --- | --- |
| [0 — Project documentation](clean/F-chelsea-zhao-0.ipynb) | Research question, dataset rationale, time records, references. | [Open in Colab](https://colab.research.google.com/github/rulangyuezhao-cmd/mie1512-reasoneval-portfolio/blob/main/notebooks/clean/F-chelsea-zhao-0.ipynb) |
| [1 — Data preparation](clean/F-chelsea-zhao-1.ipynb) | CPU exploratory analysis and stratified sample construction. | [Open in Colab](https://colab.research.google.com/github/rulangyuezhao-cmd/mie1512-reasoneval-portfolio/blob/main/notebooks/clean/F-chelsea-zhao-1.ipynb) |
| [2 — Final analysis](clean/F-chelsea-zhao-2.ipynb) | CPU metrics and figures using bundled, precomputed scores. | [Open in Colab](https://colab.research.google.com/github/rulangyuezhao-cmd/mie1512-reasoneval-portfolio/blob/main/notebooks/clean/F-chelsea-zhao-2.ipynb) |
| [Preparation with saved outputs](with-saved-outputs/original-1.ipynb) | Earlier copy with EDA figures and printed outputs. | [Open in Colab](https://colab.research.google.com/github/rulangyuezhao-cmd/mie1512-reasoneval-portfolio/blob/main/notebooks/with-saved-outputs/original-1.ipynb) |
| [Analysis with saved outputs](with-saved-outputs/original-2.ipynb) | Earlier copy with metrics and result figures. | [Open in Colab](https://colab.research.google.com/github/rulangyuezhao-cmd/mie1512-reasoneval-portfolio/blob/main/notebooks/with-saved-outputs/original-2.ipynb) |

## Run the CPU analysis

In Colab, select a CPU runtime and run all cells. The bootstrap clones this public repository if needed, reads the bundled `F-data/` files, and writes generated outputs to `generated-results/`. No private Google Drive mount is required. Notebook 2 can run independently of Notebook 1.

For local use, clone the repository, install `requirements-analysis.txt`, and open the notebooks with Jupyter or VS Code. Run from the repository root or a directory inside it. These notebooks do not download model weights or perform GPU inference.

## Changes to the public copies

Student identifiers and private paths were removed. Paths were adapted for this repository; notebook numbering, paper attribution, distribution framing, stale sample-era conclusions, and list-score parsing were corrected. Saved outputs remain historical snapshots and can differ from updated explanatory text or rerun results. Course records and experimental notes are retained as historical documentation, not independently audited claims. The original private copies were not edited.

The notebook plots retain the course project's layouts. The polished `figures/` gallery is a separate presentation of the same saved data.

The local GPU inference notebook has not been recovered. Notebook 2 is an analysis notebook, not the missing inference implementation. See [source provenance](../docs/source-manifest.csv) for original and published hashes, using relative filenames only.

## Validation

The Python code cells in both clean CPU notebooks were executed in order against copies of the bundled Parquet files. Notebook 1 regenerated a 570-solution sample; Notebook 2 completed its tables and four figures. The dependency-install cell was handled by the local analysis environment. Notebook JSON and Python syntax were checked for all five files. Colab links are provided, but a live browser-based Colab run was not performed. The historical saved-output versions were not rerun or overwritten.
