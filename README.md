# ReasonEval on ProcessBench

**MIE1512 course project · Chelsea Zhao**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Hugging Face](https://img.shields.io/badge/Hugging_Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![pandas](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Parquet](https://img.shields.io/badge/Parquet-245A81?style=for-the-badge)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge)
![Google Colab](https://img.shields.io/badge/Google_Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=black)

I applied ReasonEval-7B to human-annotated mathematical solutions to evaluate the reasoning behind the final answer.

**3,400 solutions · 12 generators · 4 benchmarks**

## Tech stack

| Area | Tools | How I used them |
| --- | --- | --- |
| **Model evaluation** | Python, PyTorch, Hugging Face | Local ReasonEval-7B scoring of reasoning steps. |
| **Data pipeline** | pandas, Parquet | Prepare benchmark labels and preserve scores for later analysis. |
| **Analysis & visualization** | Matplotlib, Google Colab | CPU analysis, metric comparisons, and step-level visual explanations. |

[Explore the results](#key-visuals) · [Generator summary](generator-summary.csv) · [Analysis notes](docs/analysis-notes.md)

## Reasoning quality, step by step

The highlighted step contains the first human-annotated error: an incorrect addition. ReasonEval assigns it a validity score of **0.495**, just below the **0.5** detection threshold.

![ReasonEval scores for a solution with an annotated calculation error](figures/step-scoring-example.png)

## What I built

| Component | What it does | Built with |
| --- | --- | --- |
| **Data preparation** | Organizes ProcessBench labels, reasoning steps, and generator metadata. | Python, pandas, Parquet |
| **Local scoring** | Applies ReasonEval-7B and preserves step- and solution-level scores. | PyTorch, Hugging Face |
| **CPU analysis** | Compares detection performance and score distributions using saved results. | pandas, Matplotlib, Colab |

## Key visuals

**Validity and redundancy across generators**

Score distributions grouped by final-answer correctness reveal how reasoning assessments vary across model families. Final-answer correctness and reasoning validity are distinct evaluation targets.

![Validity and redundancy distributions across 12 generators](figures/validity-and-redundancy.png)

**Answer accuracy versus detection performance**

Each point represents a generator. Solution-level AUC measures how well ReasonEval distinguishes solutions with and without annotated reasoning errors; higher is better. The plotted AUC values were recomputed from the saved scores and match the notebook's reported values to one decimal place.

![Generator answer accuracy versus ReasonEval detection AUC](figures/accuracy-vs-detection-auc.png)

**Low validity scores despite correct final answers**

This chart measures the fraction of correct-final-answer solutions with a solution-validity score below 0.25. A correct final answer can still contain reasoning errors, so this measure is not a false-positive rate against human reasoning-validity labels. This portfolio view focuses on the ProcessBench results.

![Low validity scores among solutions with correct final answers](figures/accuracy-vs-false-positive-rate.png)

## Dataset and reproducibility

| ProcessBench subset | Solutions |
| --- | ---: |
| GSM8K | 400 |
| MATH | 1,000 |
| OlympiadBench | 1,000 |
| OmniMath | 1,000 |
| **Total** | **3,400** |

The saved dataset and scoring results contain matching unique IDs, with no missing result fields. Available source notebooks cover documentation, data preparation, and CPU analysis of precomputed scores. This repository presents the selected visual results and a compact [generator summary](generator-summary.csv).

The original inference notebook still needs to be located to complete the reproduction workflow. These figures analyze an existing run; they do not constitute a fresh inference reproduction. See [analysis notes](docs/analysis-notes.md) for metric definitions and scope.

## Future work

Extend the evaluation with confidence calibration, comparisons against other evaluators using precomputed outputs, and analysis of how reasoning length affects error detection. Calibration is a proposed extension, separate from the completed scoring analysis.

## References

- **ReasonEval:** Xia et al., [*Evaluating Mathematical Reasoning Beyond Accuracy*](https://arxiv.org/abs/2404.05692), AAAI 2025. [Official code](https://github.com/GAIR-NLP/ReasonEval).
- **Evaluation data:** [Qwen/ProcessBench](https://huggingface.co/datasets/Qwen/ProcessBench). The step example contains a ProcessBench problem and generated solution; the scores and visual analysis are from this project.
- **Calibration direction:** Park et al., [*Know What You Don't Know: Uncertainty Calibration of Process Reward Models*](https://arxiv.org/abs/2506.09338), NeurIPS 2025.
