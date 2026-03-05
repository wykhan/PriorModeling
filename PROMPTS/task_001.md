# Task 001 — Reproduce CenterNet Baseline

This task defines the first development objective of the project.

Goal:

Reproduce the baseline CenterNet detection pipeline on the target
dataset before introducing any prior modeling modifications.

No prior modeling should be implemented in this task.


------------------------------------------------------------
1. Task Objective
------------------------------------------------------------

Verify that the original CenterNet codebase can be successfully trained
and evaluated using the target dataset.

This baseline will serve as the reference for all subsequent experiments.


------------------------------------------------------------
2. Scope of This Task
------------------------------------------------------------

This task includes:

1. Dataset preparation
2. Dataset loader verification
3. Training pipeline verification
4. Inference pipeline verification
5. Baseline result recording


This task does NOT include:

- prior modeling
- loss modification
- model architecture changes


------------------------------------------------------------
3. Expected Outcome
------------------------------------------------------------

After completing this task, the following should be true:

1. The model can start training successfully.

2. Training runs for multiple epochs without runtime errors.

3. Inference produces detection outputs.

4. Evaluation produces detection metrics.

5. The experiment results are recorded.


------------------------------------------------------------
4. Required Steps
------------------------------------------------------------

Step 1 — Verify dataset structure

Confirm the dataset exists at:

/home/superws/dataset/OBE_chest_3

Verify that:

- image files are accessible
- annotation files are valid
- dataset format is compatible with COCO-style loaders


Step 2 — Verify dataset loader

Ensure the dataset can be loaded by the CenterNet dataset pipeline.

Typical code location:

src/lib/datasets/

Confirm that:

- dataset paths are configurable
- dataset class loads annotations correctly
- training samples are generated successfully


Step 3 — Run baseline training

Start baseline training using CenterNet.

Example command:

python src/main.py ctdet \
    --exp_id baseline_m0 \
    --dataset OBE_chest_3 \
    --arch dla_34 \
    --input_h 1024 \
    --input_w 512


Verify that:

- training begins successfully
- loss values are produced
- GPU utilization is normal


Step 4 — Verify checkpoint generation

Confirm that model checkpoints are created.

Expected output:

exp/ctdet/baseline_m0/

Files should include:

model_last.pth
model_best.pth


Step 5 — Run inference

Test the trained model.

Example:

python src/test.py ctdet \
    --exp_id baseline_m0 \
    --load_model exp/ctdet/baseline_m0/model_best.pth


Verify that:

- bounding boxes are generated
- detection scores appear
- outputs are valid


Step 6 — Evaluate detection performance

Run the evaluation pipeline.

Metrics should include:

mAP
AP50
AP75
Precision@0.5
Recall@0.5


Step 7 — Save experiment artifacts

Save the following outputs:

training logs
model checkpoints
evaluation metrics
sample detection visualizations


Step 8 — Record baseline experiment

Update:

CHANGELOG_DEV.md

Include:

experiment description
training command
dataset used
random seed


------------------------------------------------------------
5. Acceptance Criteria
------------------------------------------------------------

This task is complete only if:

1. Baseline training runs successfully.

2. Evaluation metrics are produced.

3. Checkpoints are saved.

4. Results are recorded in CHANGELOG_DEV.md.

5. No prior modeling code has been added.


------------------------------------------------------------
6. Allowed Code Changes
------------------------------------------------------------

Only minimal changes are allowed during this task.

Allowed:

- dataset loader adjustments
- configuration updates
- path configuration

Forbidden:

- modifying CenterNet model architecture
- adding prior modeling modules
- changing loss functions


------------------------------------------------------------
7. Deliverables
------------------------------------------------------------

The following outputs must exist after completion:

1. baseline training logs

2. baseline checkpoints

3. evaluation metrics

4. experiment record in CHANGELOG_DEV.md