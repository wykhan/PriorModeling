# Acceptance Criteria

This document defines the conditions under which a development task
is considered **successfully completed**.

These criteria apply to all code modifications made within this project,
including those generated through AI-assisted coding.

The purpose is to ensure:

- engineering correctness
- experimental reproducibility
- scientific validity


------------------------------------------------------------
1. Engineering Acceptance Criteria
------------------------------------------------------------

A development task is considered technically complete only if:

1. The code compiles and runs without runtime errors.

2. Training can start successfully using the standard training command.

Example:

python src/main.py ctdet \
    --exp_id baseline_test \
    --dataset <dataset_name> \
    --arch dla_34

3. Inference runs successfully using the trained model.

Example:

python src/test.py ctdet \
    --exp_id baseline_test \
    --load_model <checkpoint_path>

4. The model produces valid detection outputs including:

- bounding boxes
- detection scores
- class predictions

5. No unrelated modules are modified during development.


------------------------------------------------------------
2. Baseline Reproducibility Criteria
------------------------------------------------------------

Before introducing any prior modeling modifications,
the baseline CenterNet model must be reproduced.

Baseline acceptance requires:

1. Training completes successfully.

2. Evaluation produces valid metrics.

3. Results are consistent with expected CenterNet behavior.

4. The baseline run is recorded in:

CHANGELOG_DEV.md


------------------------------------------------------------
3. Prior Modeling Integration Criteria
------------------------------------------------------------

A prior modeling modification is accepted only if:

1. The model can still be trained end-to-end.

2. Prior modules do not break the existing detection pipeline.

3. The prior integration produces valid outputs during inference.

4. Prior parameters can be configured through experiment settings.


------------------------------------------------------------
4. Experiment Reproducibility Criteria
------------------------------------------------------------

Every experiment must satisfy the following conditions:

1. The experiment configuration is recorded.

2. The Git commit hash is recorded.

3. Random seed values are fixed.

Example seeds:

11
22
33

4. The experiment command can be executed by another user
   to reproduce the results.

5. All outputs are stored in a structured directory.


------------------------------------------------------------
5. Logging and Artifacts
------------------------------------------------------------

Each experiment run must produce the following artifacts.

Configuration

- training command
- model architecture
- dataset information
- random seed

Model checkpoints

- best model checkpoint
- last epoch checkpoint

Metrics

- validation metrics
- test metrics (if applicable)

Visualizations

- qualitative detection examples
- prior distribution visualization (if applicable)


------------------------------------------------------------
6. Scientific Acceptance Criteria
------------------------------------------------------------

A research contribution is considered valid only if:

1. The prior-based method improves at least one primary metric.

Primary metrics include:

- mAP@[.5:.95]
- AP50
- AP75
- Precision@0.5

2. Improvements are consistent across multiple random seeds.

3. Control experiments confirm that improvements are not caused by:

- random priors
- implementation artifacts
- data leakage


------------------------------------------------------------
7. Code Quality Requirements
------------------------------------------------------------

All new code must satisfy the following requirements:

1. Functions include docstrings explaining their purpose.

2. Variable names are descriptive and consistent.

3. Code follows the structure of the existing repository.

4. No hard-coded dataset paths should appear in the code.


------------------------------------------------------------
8. Documentation Requirements
------------------------------------------------------------

Every completed task must include documentation updates.

Required updates include:

CHANGELOG_DEV.md

Description of the change
Files modified
Verification steps

doc/assumptions.md

Any new experimental assumptions
Dataset changes
Environment dependencies


------------------------------------------------------------
9. Completion Definition
------------------------------------------------------------

A development task is considered **fully completed**
only when all of the following are satisfied:

- code runs successfully
- experiments execute correctly
- outputs are reproducible
- documentation is updated
- changes are logged in CHANGELOG_DEV.md

10. AI Modification Constraint

AI-generated code must not modify more than one module at a time.
Large structural refactoring is not allowed.