# Analysis notes

## What was verified

- Dataset and scoring artifacts each contain 3,400 unique solution IDs; the ID sets match.
- There are 12 generators and no missing fields in the scoring artifact.
- Per-generator solution-level AUC was recalculated using average ranks, with ties handled, and matches the saved notebook values to one decimal place.
- Final-answer accuracy and the rate of validity scores below 0.25 among correct-final-answer solutions were recomputed from the saved data.
- Original notebook and result files were left unchanged when producing these figures.

## Metric definitions

- `accuracy`: percentage of final answers marked correct by ProcessBench.
- `auc`: percentage-scale ROC AUC of solution-validity scores against human labels (`label == -1` means no annotated reasoning error). Higher scores predict valid solutions.
- `fpr`: historical column name retained in the summary CSV. Its denominator is solutions with correct final answers, not solutions with no human-annotated reasoning error. Its numerator is those with `solution_validity < 0.25`. Interpret this as a flagging rate conditional on answer correctness, not a reasoning-label false-positive rate.
- Box plots show solution-level validity and redundancy, grouped by final-answer correctness. Redundancy scores are descriptive: ProcessBench supplies no human redundancy labels for validating them.
- Example step indices are zero-based. The stored evaluator scores are not calibrated probabilities.

## Reproduction scope

### Effort and runtime provenance

The project documentation records approximately 10 hours for V1 and 20 additional hours for the final phase, or 30 hours in total. These are retrospective effort records, not independently measured timesheets. The totals include exploratory work that was scoped out of the final analysis. Primary-paper reading and GPU compute time were excluded from the recorded project-effort totals.

The final score artifact contains 3,400 non-null per-solution `inference_time_sec` values. Their sum is 8,281 seconds (2 h 18 min 1 s); mean 2.4356 seconds; median 2.1 seconds; range 1.3–44.5 seconds. This aggregates persisted row timings, potentially including scores reused from the prototype. It does not establish the wall-clock duration of a single uninterrupted full run. The notes' roughly two-hour inference estimate is consistent in scale but less precise than the aggregate row timings.

Recorded setup subtasks are 0.5 hours for dependencies/environment and 1.5 hours for model-class integration, tokenizer, and device mapping. Final packaging subtasks total 2.5 hours: Parquet migration (0.5), notebook documentation (1.0), and cleanup/validation (1.0). These are subsets of the 30-hour effort total. No separate model-weight download/loading timing is available. Quantization is unconfirmed; GPU/CPU offloading is documented.

The original workflow used local GPU scoring followed by CPU analysis of saved Parquet artifacts. The original GPU inference notebook has not been recovered, so a complete rerun cannot yet be claimed. Step-level AUC and F1 values are not independently verified here.

To obtain upstream resources without keeping model caches in this repository, use the [official ReasonEval setup and usage instructions](https://github.com/GAIR-NLP/ReasonEval) and download [ProcessBench](https://huggingface.co/datasets/Qwen/ProcessBench). Record exact upstream revisions and environment versions for any new run. The current dataset may contain annotation revisions relative to the saved course-project artifacts.

ProcessBench and the ReasonEval-7B model cards list Apache-2.0. Their upstream terms apply to those resources; this repository does not relicense upstream code or weights. No model weights, caches, full benchmark copies, or original submission notebooks are included.

The original presentation is omitted from this public repository because it displays a student identifier. A public-safe presentation link can be added after that copy is prepared.
