# ReasonEval: Local LLM Implementation & Evaluation

**MIE1512 course project · Chelsea Zhao**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Hugging Face](https://img.shields.io/badge/Hugging_Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![pandas](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Parquet](https://img.shields.io/badge/Parquet-245A81?style=for-the-badge)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge)
![Google Colab](https://img.shields.io/badge/Google_Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=black)

I implemented a **local LLM inference pipeline** for ReasonEval-7B and used it to **evaluate the mathematical reasoning of 12 language models** on ProcessBench. The project connects practical model deployment with evaluation of reasoning quality beyond final-answer accuracy.

| Major concept | My contribution |
| --- | --- |
| **Local LLM implementation** | Adapted the pretrained ReasonEval-7B evaluator for local inference on an NVIDIA RTX 5070, with GPU/CPU offloading and saved scoring outputs. |
| **LLM evaluation** | Scored reasoning steps for validity and redundancy, then assessed error detection against human annotations across four benchmark subsets. |

**3,400 solutions · 12 generators · 4 benchmarks**

## Tech stack

| Area | Tools | How I used them |
| --- | --- | --- |
| **Local LLM implementation** | Python, PyTorch, Hugging Face | Load and run the pretrained ReasonEval-7B evaluator locally, with GPU/CPU offloading. |
| **LLM evaluation** | ProcessBench, pandas, Matplotlib | Compare evaluator scores with human reasoning-error labels and analyze differences across generators. |
| **Data pipeline** | pandas, Parquet | Prepare benchmark labels and preserve scores for later analysis. |
| **Analysis & visualization** | Matplotlib, Google Colab | CPU analysis, metric comparisons, and step-level visual explanations. |

[Explore the results](#key-visuals) · [Generator summary](generator-summary.csv) · [Analysis notes](docs/analysis-notes.md)

## Reasoning quality, step by step

The highlighted step contains the first human-annotated error: an incorrect addition. ReasonEval assigns it a validity score of **0.495**, just below the **0.5** detection threshold.

![ReasonEval scores for a solution with an annotated calculation error](figures/step-scoring-example.png)

## Local LLM implementation

- Adapted the pretrained, Mistral-based **ReasonEval-7B** evaluator for local execution using Python, PyTorch, and Hugging Face.
- Configured **GPU/CPU offloading** to run inference on an NVIDIA RTX 5070 workstation.
- Built a scoring workflow with checkpointed outputs, preserving step- and solution-level validity and redundancy scores for later analysis.
- Stored results in **Parquet**, allowing downstream analysis to run on CPU without reloading the LLM.

## LLM evaluation

- Evaluated **existing mathematical solutions from 12 LLM generators**, spanning GSM8K, MATH, OlympiadBench, and OmniMath.
- Compared ReasonEval's validity scores with **human-annotated reasoning errors**, measuring how well the evaluator distinguishes valid and invalid solutions.
- Analyzed **solution-level AUC**, validity and redundancy distributions, and low-validity flags among correct-final-answer solutions.
- Built visual explanations linking individual reasoning steps to scores and human labels.

ReasonEval is the locally implemented evaluator; the 12 generators supplied the benchmark solutions. The project uses pretrained model weights and focuses on inference engineering and evaluation.

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
