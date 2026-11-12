# ReasonEval: Local LLM Implementation & Evaluation

**MIE1512H · Data Analytics · Winter 2026 (January–April) · Chelsea Zhao**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Hugging Face](https://img.shields.io/badge/Hugging_Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![pandas](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Parquet](https://img.shields.io/badge/Parquet-245A81?style=for-the-badge)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge)
![Google Colab](https://img.shields.io/badge/Google_Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=black)

I implemented a **local LLM inference pipeline** for ReasonEval-7B and used it to **evaluate the mathematical reasoning of 12 language models** on ProcessBench: **MATH, GSM8K, OlympiadBench, and OmniMath**. The evaluation spans a MATH-domain reference and out-of-distribution problem domains, testing reasoning quality beyond final-answer accuracy.

## Objective: make reasoning review scalable

Manually checking every step of thousands of mathematical solutions requires substantial human time and expertise. Checking only the final answer can also miss flawed logic or unnecessary steps. Automated reasoning evaluation offers a way to screen solutions consistently and flag potential issues for closer review. [ReasonEval](https://arxiv.org/abs/2404.05692) and [ProcessBench](https://arxiv.org/abs/2412.06559) motivate this focus on reasoning processes.

**My objective:** implement a local LLM evaluator that scores reasoning steps automatically, then test how reliably its judgments agree with human annotations across familiar and harder problem domains. The intended benefit is to reduce the manual review burden while preserving human oversight; this project measures evaluator performance, not a demonstrated reduction in human-review costs.

## Course context

This project was completed for **MIE1512H — Data Analytics**, in **Winter 2026 (January–April 2026)**, as part of my **Master of Engineering (MEng) at the University of Toronto**. The title and term follow my academic history; the date range identifies the course session. The course is a research seminar connecting published analytics research with open datasets, data preparation, and computational notebooks. Its project emphasizes reproducing published research with attention to data management. [Course description](https://sgs.calendar.utoronto.ca/course/mie1512h).

For this project, I practiced the full research workflow: **reviewing papers, finding and comparing datasets on Hugging Face, selecting suitable human annotations, preparing the data, implementing a local LLM evaluator, and critically analyzing its results**.

| Major concept | My contribution |
| --- | --- |
| **Local LLM implementation** | Adapted the pretrained ReasonEval-7B evaluator for local inference on an NVIDIA RTX 5070, with GPU/CPU offloading and saved scoring outputs. |
| **LLM evaluation** | Scored reasoning steps for validity and redundancy, then assessed error detection against human annotations across four benchmark subsets. |

**3,400 solutions · 12 generators · 4 benchmarks**

**≈30 hours documented project effort · 2.30 hours aggregate inference time**

## Tech stack

| Area | Tools | How I used them |
| --- | --- | --- |
| **Local LLM implementation** | Python, PyTorch, Hugging Face | Load and run the pretrained ReasonEval-7B evaluator locally, with GPU/CPU offloading. |
| **LLM evaluation** | ProcessBench, pandas, Matplotlib | Compare evaluator scores with human reasoning-error labels and analyze differences across generators. |
| **Data pipeline** | pandas, Parquet | Prepare benchmark labels and preserve scores for later analysis. |
| **Analysis & visualization** | Matplotlib, Google Colab | CPU analysis, metric comparisons, and step-level visual explanations. |

[Explore the results](#key-visuals) · [Notebooks & Colab](notebooks/README.md) · [Data & scores](F-data/README.md) · [Analysis notes](docs/analysis-notes.md)

## Notebooks and data

All five available notebook versions and the three saved data/result files are bundled here. Start with the clean notebooks; the saved-output copies preserve the original visual record.

| Notebook | Purpose | Open |
| --- | --- | --- |
| **0 — Documentation** | Research question, dataset rationale, timeline, and references. | [GitHub](notebooks/clean/F-chelsea-zhao-0.ipynb) · [Colab](https://colab.research.google.com/github/rulangyuezhao-cmd/mie1512-reasoneval-portfolio/blob/main/notebooks/clean/F-chelsea-zhao-0.ipynb) |
| **1 — Data preparation** | CPU data exploration and 570-solution stratified sampling. | [GitHub](notebooks/clean/F-chelsea-zhao-1.ipynb) · [Colab](https://colab.research.google.com/github/rulangyuezhao-cmd/mie1512-reasoneval-portfolio/blob/main/notebooks/clean/F-chelsea-zhao-1.ipynb) |
| **2 — Final analysis** | CPU metrics and figures from the bundled scores. | [GitHub](notebooks/clean/F-chelsea-zhao-2.ipynb) · [Colab](https://colab.research.google.com/github/rulangyuezhao-cmd/mie1512-reasoneval-portfolio/blob/main/notebooks/clean/F-chelsea-zhao-2.ipynb) |

**[Browse notebooks with saved outputs](notebooks/with-saved-outputs/)** · **[Browse bundled Parquet and CSV files](F-data/)**

The Colab bootstrap retrieves this repository and reads its bundled data; no private Drive mount is required. Public copies remove student identifiers and private paths. See the [notebook guide](notebooks/README.md) for changes and reading order. The original GPU inference notebook remains missing; Notebook 2 analyzes saved outputs.

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

## Project timeline

| Phase | Recorded effort | Milestone |
| --- | ---: | --- |
| **Prototype** | **≈10 h** | Set up the local environment, adapt the evaluator, prepare data, and score a 570-solution sample. |
| **Final phase** | **≈20 h additional** | Scale to 3,400 solutions, recover from GPU memory issues, produce analysis and figures, explore extensions, and prepare the report and notebooks. |
| **Total** | **≈30 h** | Documented project effort; primary-paper reading and GPU compute time were tracked separately. |

**Setup and packaging, within those totals:** dependency/environment configuration **0.5 h**; model-class integration, tokenizer and device mapping **1.5 h**; final Parquet migration, notebook documentation and cleanup **2.5 h**.

**Inference:** the saved `inference_time_sec` values sum to **8,281 seconds — 2 h 18 min 1 s** across all 3,400 solutions (**2.44 s mean; 2.10 s median**). This is aggregate recorded scoring time, not a measured end-to-end wall-clock duration; model download/loading, setup, and unrecorded interruptions are not captured by that sum.

**Model weights:** used pretrained ReasonEval-7B weights for local inference with GPU/CPU offloading. Weight download and initial loading durations were not separately recorded. The preserved notes document offloading; a specific 4-bit or 8-bit quantization setting has not been recovered.

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

**Datasets and distribution shifts.** The pretrained ReasonEval evaluator was trained on **PRM800K**, whose annotated solutions are based on **MATH** problems. Relative to that training domain, I use the following framing:

| ProcessBench subset used | Solutions | Distribution framing | What it tests |
| --- | ---: | --- | --- |
| **MATH** | 1,000 | In-distribution-like at the problem-domain level | Competition mathematics from the same source domain as PRM800K. |
| **GSM8K** | 400 | Out-of-distribution: grade-school domain | Transfer to simpler, multi-step arithmetic word problems. |
| **OlympiadBench** | 1,000 | Out-of-distribution: harder domain | Transfer to olympiad-level mathematical reasoning. |
| **OmniMath** | 1,000 | Out-of-distribution: harder domain | Transfer to a broad collection of olympiad-level problems. |
| **Total** | **3,400** | | |

Here, **in-distribution (ID)** and **out-of-distribution (OOD)** refer to the evaluator's training domain. MATH is an ID-like reference, not a claim that the full ProcessBench distribution matches training: solution generators, writing styles, and annotations can also shift. This is a domain-based framing rather than a measured distribution test.

**Source distinction:** PRM800K is the pretrained evaluator's training source; I did not retrain the model on it. My evaluation uses the four **ProcessBench subsets** above. The original ReasonEval paper's **MR-MATH** and **MR-GSM8K** datasets provide reference context and were not the datasets scored in this run. See the [ReasonEval paper](https://arxiv.org/html/2404.05692v2) and [ProcessBench dataset card](https://huggingface.co/datasets/Qwen/ProcessBench).

The saved dataset and scoring results contain matching unique IDs, with no missing result fields. Bundled notebooks cover documentation, data preparation, and CPU analysis of precomputed scores. This repository presents the selected visual results and a compact [generator summary](generator-summary.csv).

The original inference notebook still needs to be located to complete the reproduction workflow. These figures analyze an existing run; they do not constitute a fresh inference reproduction. See [analysis notes](docs/analysis-notes.md) for metric definitions and scope.

## Future work: knowing when the evaluator is uncertain

Following my professor's recommendation, the next research direction is **uncertainty calibration**, guided by Park et al.'s *Know What You Don't Know: Uncertainty Calibration of Process Reward Models* (NeurIPS 2025).

**[Paper](https://arxiv.org/abs/2506.09338) · [Official GitHub](https://github.com/young-j-park/prm-calibration) · [Project and visual explanations](https://young-j-park.github.io/know-what-you-dont-know/)**

The paper examines how process reward models can be overconfident and studies calibration of their success estimates. For this project, the proposed extension is to investigate uncertainty-aware evaluation: compare calibration methods on held-out data, examine shifts across generators and problem difficulty, and identify uncertain cases that may warrant human review. Applying the paper's approach to ReasonEval requires matching the calibration target to the evaluation task; the current validity scores are not calibrated probabilities.

Other extensions include comparing evaluators using precomputed outputs and examining how the number of reasoning steps affects error detection. **These are future directions, separate from the completed local inference and scoring analysis.**

## Three core papers

| Paper | Role in this project | Resources |
| --- | --- | --- |
| **Xia et al. — Evaluating Mathematical Reasoning Beyond Accuracy** · AAAI 2025 | **Method:** ReasonEval's step-level validity and redundancy evaluation. | [Paper](https://arxiv.org/abs/2404.05692) · [GitHub](https://github.com/GAIR-NLP/ReasonEval) |
| **Zheng et al. — ProcessBench: Identifying Process Errors in Mathematical Reasoning** · ACL 2025 | **Benchmark:** human-annotated solutions used to evaluate reasoning-error detection. | [Paper](https://arxiv.org/abs/2412.06559) · [GitHub](https://github.com/QwenLM/ProcessBench) · [Dataset](https://huggingface.co/datasets/Qwen/ProcessBench) |
| **Park et al. — Know What You Don't Know: Uncertainty Calibration of Process Reward Models** · NeurIPS 2025 | **Future work:** uncertainty calibration and more reliable use of evaluator scores. | [Paper](https://arxiv.org/abs/2506.09338) · [GitHub](https://github.com/young-j-park/prm-calibration) · [Project](https://young-j-park.github.io/know-what-you-dont-know/) |

The step-scoring example contains a ProcessBench problem and generated solution. Scores and visual analysis shown here are from this course project; the evaluation method and benchmark are credited above.
