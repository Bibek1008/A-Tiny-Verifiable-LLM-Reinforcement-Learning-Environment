# VeriRL

## A Tiny Verifiable LLM Reinforcement Learning Environment

VeriRL is a small, executable reinforcement-learning environment for studying
how a decoder-only language model can learn to interact with a deterministic
symbolic environment.

The project uses a one-box Sokoban environment together with an exact BFS
verifier, a small decoder-only Transformer, supervised behavior cloning, and
online REINFORCE training.

The system is intentionally small enough to run on a local computer without
requiring a pretrained language model or a large GPU.

---

## Overview

The complete training pipeline is:

```text
Sokoban Environment
        |
        v
     BFS Solver
        |
        v
Generated Demonstrations
        |
        v
   SFT Warm Start
        |
        v
   Online REINFORCE
        |
        v
   Frozen Evaluation

