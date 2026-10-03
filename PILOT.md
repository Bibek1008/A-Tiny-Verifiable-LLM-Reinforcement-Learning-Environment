# VeriRL Experiment Receipt

The results below were generated on my local Windows environment for this
project.

## Environment

- Operating system: Windows
- Device: CPU
- Python: 3.12.10
- PyTorch: 2.14.1+cpu
- Model parameters: 19,876
- Tokenizer history limit: 0

## Baseline pilot

### Configuration

- Seed: `101`
- Evaluation split: `validation`
- Validation puzzles evaluated: `73`
- SFT epochs: `30`
- SFT examples: `1,347`
- RL updates: `80`
- RL batch size: `64`
- Reward: BFS-based progress reward
- RL sampler: uniform

### Result

The SFT-only condition achieved a greedy solve rate of:

`38.36%`

The SFT + RL condition achieved:

`69.86%`

The paired improvement was:

`+31.51 percentage points`

The title state was solved by the RL condition using:

`DDLDRR`

This was a one-seed pilot and is not treated as a final multi-seed result.

## Curriculum pilot

A second experiment compared uniform task sampling with adaptive curriculum
sampling.

Both RL conditions started from the same 30-epoch SFT checkpoint and used the
same model, seed, reward formulation, RL budget, and validation split.

### Uniform RL

- Sampler: `uniform`
- Final solve rate: `69.86%`
- Final boundary: `2`

### Curriculum RL — window 3

- Sampler: `curriculum`
- Curriculum window: `3`
- Final solve rate: `60.27%`
- Final boundary: `10`
- Difference from uniform: `-9.59 percentage points`

### Curriculum RL — window 5

- Sampler: `curriculum`
- Curriculum window: `5`
- Final solve rate: `64.38%`
- Final boundary: `10`
- Difference from uniform: `-5.48 percentage points`

### Curriculum RL — window 10

- Sampler: `curriculum`
- Curriculum window: `10`
- Final solve rate: `61.64%`
- Final boundary: `10`
- Difference from uniform: `-8.22 percentage points`

## Interpretation

These curriculum experiments are one-seed pilot studies. They show that the
tested curriculum configurations reached higher training difficulty
boundaries, but did not outperform uniform sampling on the held-out validation
set in these runs.

The results are therefore used as development evidence rather than as a
general claim about curriculum learning.

## Reproducibility

The experiment configurations used for these runs are stored under:

`configs/`

The generated experiment outputs are stored under:

`results/`

Configuration hashes are recorded automatically in the experiment summaries.