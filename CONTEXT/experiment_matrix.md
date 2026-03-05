# Experiment Matrix

This document defines the experiment plan for the "Prior Modeling" project built on top of CenterNet.
It is both:
1) a paper-ready experiment blueprint, and
2) an auditable engineering checklist for vibe-coding.

Goals:
- Validate whether structural priors (position / size / joint) improve detection quality under mmWave-like constraints.
- Ensure every model variant is reproducible and traceable (branch/tag/config/log).

------------------------------------------------------------
0. Definitions
------------------------------------------------------------

Task:
- Center-based object detection (CenterNet ctdet pipeline)

Baseline:
- Original CenterNet (as close as possible), trained & evaluated with the same data splits and settings.

Priors (to be incorporated):
- Position prior: π_pos(x, y) or related bias on center heatmap / candidate centers.
- Size prior: π_size(w, h) or related bias on width/height regression.
- Joint prior: π_joint(x, y, w, h) or factorized fusion π_pos * π_size.

Integration level (where the prior is injected):
- H (Heatmap-level): affects center heatmap logits / probabilities.
- D (Decode-level): affects candidate selection / scoring during decoding.
- L (Loss-level): adds a regularization / likelihood term to training loss.
- P (Post-process): affects NMS / box scoring after decoding.

Notation:
- M0: Baseline (no prior)
- M1: Position prior only
- M2: Size prior only
- M3: Joint prior (position + size)
- M4: Ablation variants (controls / sanity checks)

Metrics:
- Detection: mAP@[.5:.95], AP50, AP75, Precision@0.5, Recall@0.5, FP/image
- Localization: AR, mean center error (if available), size error (if available)
- Calibration / confidence: ECE 
- Efficiency: FPS (inference), training time/epoch, memory

------------------------------------------------------------
1. Dataset & Splits (Fill-in)
------------------------------------------------------------

Dataset name:
- OBE_chest_3
Dataset path:
- /home/superws/dataset/OBE_chest_3

Data format:
- COCO-like JSON

annotation format:
- COCO style
- images/
- annotations/
    train.json
    val.json
    test.json

Splits:
- Train: 5534
- Val:   557
- Test:  555

Classes:
- num_classes: 5
class_names:
  - CIGARETTE
  - LIGHTER
  - MATCH
  - PHONEF
  - OBJECT

Notes:
- Keep the split fixed across all experiments.
- Record any filtering / preprocessing steps in doc/assumptions.md and CHANGELOG_DEV.md.

------------------------------------------------------------
2. Controlled Variables (Must be fixed across variants)
------------------------------------------------------------

Backbone / architecture:
- arch: res_50 (baseline)

Input size / resolution:
- input resolution: 1024 × 512 (H × W)

Training schedule:
- epochs: 140
- batch size: 16
- optimizer:  Adam
- lr: 1.25e-4
- lr_step: [90,120]
- random seeds: [11, 22, 33]


Decoding / NMS:
- topK: default
- score threshold: default
- nms: default

Evaluation script:
- Use the same script for all variants.
- Record exact commands in CHANGELOG_DEV.md.

------------------------------------------------------------
3. Experiment Table (Main Matrix)
------------------------------------------------------------

Each row = one reproducible run group (ideally 3 seeds for main claims).

Legend:
- Prior Type: None / Pos / Size / Joint
- Injection: H / D / L / P (see Section 0)
- Strength: λ (or temperature τ), or config key controlling effect
- Expected effect: qualitative hypothesis
- Outputs: what to save (models, logs, metrics, plots)

| ID  | Model Variant | Prior Type | Injection | Strength Config | Hypothesis / Expected Effect | Outputs Required |
|-----|---------------|------------|-----------|-----------------|-----------------------------|-----------------|
| M0  | CenterNet baseline | None  | -   | - | Reference performance; establish reproducibility | ckpt, train log, val metrics, test metrics |
| M1  | + Position Prior (default) | Pos   | H (primary) | lambda_pos=[TO FILL] | Reduce false positives in implausible regions; improve AP50 | ckpt, metrics, center heatmap visualization |
| M2  | + Size Prior (default) | Size  | L (primary) | lambda_size=[TO FILL] | Stabilize bbox regression; improve AP75 / size error | ckpt, metrics, width/height residual plots |
| M3  | + Joint Prior (default) | Joint | H+L (primary) | lambda_joint=[TO FILL] | Best overall; improve mAP@[.5:.95] and calibration | ckpt, metrics, calibration plot (optional) |
| M4a | Pos prior injected at Decode | Pos | D | lambda_pos=[TO FILL] | Compare injection location sensitivity | same as M1 |
| M4b | Size prior injected at Decode | Size | D | lambda_size=[TO FILL] | Compare injection location sensitivity | same as M2 |
| M4c | Joint prior only at Decode | Joint | D | lambda_joint=[TO FILL] | Decode-only may help without affecting training | same as M3 |
| M4d | Random prior (sanity check) | Random | H | lambda_rand=[TO FILL] | Should NOT improve; detect leakage/bug | same as M1 |
| M4e | Uniform prior (sanity check) | Uniform | H | lambda_uni=[TO FILL] | Should match baseline; verify implementation neutrality | same as M0 |

Notes:
- M1–M3 are the primary comparison group for the paper.
- M4* are ablations / controls to ensure claims are valid.

------------------------------------------------------------
4. Hyperparameter Sweep Plan (Minimal but defensible)
------------------------------------------------------------

Do not over-sweep; keep it publishable and auditable.

For each primary prior (M1/M2/M3), test:

lambda grid:
- λ ∈ {0.1, 0.3, 1.0}  (example; adjust per stability)

Optionally temperature / smoothing:
- τ ∈ {0.5, 1.0, 2.0}  (if you use temperature scaling)

Selection strategy:
- Use Val set to pick λ* (and τ* if applicable)
- Report Test using the chosen hyperparameters
- Record chosen values and rationale in doc/assumptions.md

------------------------------------------------------------
5. Evaluation Protocol
------------------------------------------------------------

Primary metrics (paper main table):
- mAP@[.5:.95]
- AP50
- AP75
- Precision@0.5

Secondary:
- AR (overall)
- ECE 
- Efficiency: FPS / latency

Statistical robustness:
- Main results should use >= 3 random seeds if compute allows.
- Report mean ± std on Val; Test can be single best config but must be declared.

Failure modes to monitor:
- Training instability / divergence (watch loss spikes)
- Over-regularization (performance drops across all metrics)
- Prior leakage (prior computed using GT at test time — forbidden)

------------------------------------------------------------
6. Required Artifacts & Logging Standard
------------------------------------------------------------

For each run (each seed), save:

1) Config snapshot
- full command line
- opts dump / yaml copy
- git commit hash

2) Checkpoints
- best_val.pth
- last.pth

3) Metrics
- metrics_val.json
- metrics_test.json (if available)

4) Visualizations (minimum set)
- qualitative detections (>= 20 samples)
- heatmap visualization for M0 vs M1 (position prior)
- size residual histogram for M0 vs M2 (size prior)

5) Time/efficiency
- train time per epoch
- inference FPS (same hardware)

Suggested directory convention:

runs/
  exp_{ID}/
    seed_{s}/
      config/
      ckpt/
      logs/
      metrics/
      viz/

------------------------------------------------------------
7. Paper-Ready Tables & Figures (Placeholders)
------------------------------------------------------------

Table T1 (Main results):
- Rows: M0, M1, M2, M3 (mean ± std over seeds)
- Columns: mAP, AP50, AP75, AR, FPS

Table T2 (Ablations):
- Rows: M4a, M4b, M4c, M4d, M4e
- Columns: mAP, AP50, AP75

Figure F1:
- Prior modeling diagram (pipeline: baseline vs prior-aware)
Figure F2:
- Qualitative comparison (M0 vs M3)
Figure F3 (optional):
- Calibration curve / reliability diagram (if ECE used)

------------------------------------------------------------
8. Acceptance Criteria (Definition of "Done")
------------------------------------------------------------

Engineering acceptance:
- M0 baseline reproduces within expected tolerance vs original CenterNet logs.
- All variants train and evaluate successfully with identical splits.
- All modifications are documented in CHANGELOG_DEV.md.
- Each variant is runnable with a single command.

Scientific acceptance:
- At least one of M1/M2/M3 improves primary metric (mAP or AP50/AP75) consistently across seeds.
- Ablations (random/uniform) behave as expected (no artificial gain).
- The chosen narrative (why priors help) is supported by qualitative and/or residual analysis.

------------------------------------------------------------
9. Next Actions (Implementation Order)
------------------------------------------------------------

Recommended order to implement:

Step A: Reproduce baseline M0 end-to-end (train+val+test) and freeze.
Step B: Implement M1 (Position prior) at one injection level (H) only.
Step C: Implement M2 (Size prior) at one injection level (L) only.
Step D: Implement M3 (Joint prior) based on B+C.
Step E: Add ablations M4*.
Step F: Generate plots/tables for paper.
