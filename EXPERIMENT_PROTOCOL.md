# VeriRL Experiment Protocol

The experiments and results reported in this repository are generated for this
project.

## Question

The project investigates whether 80 updates of executable RL fine-tuning
improve greedy solve rate on unseen one-box Sokoban states beyond the
corresponding BFS-imitation warm start.

This is a small mechanism study using a verifiable executable environment.

## Environment and hidden verifier

- Board: the fixed 6 x 6 wall topology defined by the Sokoban environment.
- State: player position, one box position, one goal position, and fixed walls.
- Actions: `U`, `D`, `L`, `R`.
- Transition: deterministic Sokoban movement and pushing.
- Base reward: `-0.1` per attempted action, `+1` when the box reaches the goal,
  and `+10` when solved.
- Termination: solved; truncation after 20 actions; deadlocks are recorded.
- Generator: enumerate solvable configurations and label shortest-path length
  using exact BFS.

The BFS oracle is not included in the policy input. It is used for three
purposes: generating shortest-action imitation examples, measuring puzzle
difficulty, and providing the RL progress signal.

The solution and shortest-path distance are not provided as observations to
the policy during RL interaction.

## Policy

- Autoregressive causal decoder-only Transformer initialized from scratch.
- 19,876 trainable parameters.
- Two Transformer blocks.
- Model width 32.
- Four attention heads.
- Feed-forward width 64.
- Input: row-major board tokens and an action-prediction token.
- No action history is retained.
- Output: a distribution over the four action tokens.
- No pretrained checkpoint, external tokenizer, external dataset, or model
  download.

## Paired conditions

For every seed, the reported conditions share the same random initialization
and the same behavior-cloned checkpoint.

1. `sft`: 30 epochs of cross-entropy training on shortest-action pairs
   generated from training states with solution length at most 10.

2. `sft_plus_rl`: starts from the same SFT checkpoint and performs 80 batches
   of 64 executable on-policy rollouts. Tasks are sampled uniformly by
   shortest distance 1-10. REINFORCE uses return-to-go, a batch baseline, an
   entropy coefficient of 0.005, and the hidden BFS-potential reward.

The two conditions are evaluated on the same held-out evaluation split.

## Reproducibility

- Config: `configs/final.json`
- Config SHA-256: generated from the project configuration
- Split seed: `314159`
- Split SHA-256: generated from the project split
- Paired model seeds: `2001, 2002, 2003, 2004, 2005`
- Test identifiers are disjoint from training and validation identifiers.

## Metrics and receipts

### Primary metric

- Greedy solve rate on unseen test puzzles with optimal length 1-10.

### Secondary metrics

- Overall solve rate.
- Solve rate by puzzle distance.
- Invalid-action rate.
- Deadlock rate.
- Excess steps when solved.
- Predesignated title replay where applicable.

### Experiment receipts

- Configuration hash.
- Split hash.
- Per-seed CSV/JSON results.
- Raw trajectory logs.
- Training logs.
- Runtime and device metadata.
- Model checkpoint hash.
- Serialized replay where applicable.

## Allowed conclusion

The experiment determines whether RL fine-tuning improves held-out greedy
solve rate relative to the BFS-imitation warm start under the specified
tiny-policy setup.

Any conclusion is limited to the fixed-wall, one-box Sokoban environment and
the tested training configuration.

The experiment does not establish general Sokoban competence, natural-language
reasoning ability, transfer to unseen wall layouts, or equivalence to
frontier-scale agent training.