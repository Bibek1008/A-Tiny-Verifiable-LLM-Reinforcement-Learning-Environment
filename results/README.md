# Result Artifacts

Experiment outputs are generated locally and stored under `results/`.

Each experiment records:

- Configuration used for the run.
- Random seed and experiment metadata.
- Per-condition metrics.
- Evaluation results.
- Training logs and learning curves where available.

Generated artifacts are kept separate from source code.

Intermediate checkpoints and temporary training files are ignored by Git unless
they are intentionally selected for release.

For reproducibility, each reported result should be associated with the
configuration and experiment output that produced it.

Results in this repository are generated from experiments performed for this
project and should not be confused with results from the reference project.