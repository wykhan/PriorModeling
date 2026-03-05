# Repository Map

This document describes the key structure of the CenterNet codebase.
It is intended to help developers and AI coding agents understand where
different functionalities are implemented.

Reference repository:
https://github.com/xingyizhou/CenterNet


------------------------------------------------------------
1. High-Level Repository Structure
------------------------------------------------------------

CenterNet/

│
├── src/
│   Main source code directory
│
├── experiments/
│   Training configuration files
│
├── models/
│   Pretrained model storage
│
├── images/
│   Example outputs and figures
│
└── data/
    Dataset organization utilities


The majority of implementation logic lives under:

src/lib/


------------------------------------------------------------
2. Core Code Directory
------------------------------------------------------------

src/lib/

│
├── datasets/
│   Dataset loading and preprocessing
│
├── detectors/
│   High-level detector classes
│
├── models/
│   Neural network architectures
│
├── trains/
│   Training loop implementations
│
├── utils/
│   Utility functions
│
└── opts.py
    Global training configuration


------------------------------------------------------------
3. Dataset Pipeline
------------------------------------------------------------

src/lib/datasets/

Purpose:
Load datasets and convert annotations into CenterNet training targets.

Important components:

dataset_factory.py
    Registers available datasets

sample/
    Defines training sample generation

dataset/
    Dataset-specific implementations


Typical data flow:

Dataset → Annotation parsing → Target heatmap generation


------------------------------------------------------------
4. Detector Layer
------------------------------------------------------------

src/lib/detectors/

Purpose:
Define inference logic for each task.

Examples:

ctdet.py
    Center-based object detection

multi_pose.py
    Human pose estimation


Responsibilities:

- network forward pass
- decoding predicted heatmaps
- producing bounding boxes


------------------------------------------------------------
5. Model Architecture
------------------------------------------------------------

src/lib/models/

Purpose:
Define neural network architectures and heads.

Submodules:

networks/
    Backbone networks (ResNet, DLA, etc.)

decode.py
    Heatmap decoding logic

losses.py
    Loss functions

model.py
    Model construction interface


Key concept:

CenterNet predicts object centers using heatmaps.


------------------------------------------------------------
6. Training Pipeline
------------------------------------------------------------

src/lib/trains/

Purpose:
Training loop implementations.

Examples:

ctdet.py
    Training logic for object detection

Responsibilities:

- forward pass
- loss computation
- optimizer step


------------------------------------------------------------
7. Utility Functions
------------------------------------------------------------

src/lib/utils/

Common utilities including:

image processing

post-processing

evaluation helpers


------------------------------------------------------------
8. Configuration System
------------------------------------------------------------

src/lib/opts.py

Central configuration entry point.

Handles:

- dataset selection
- model architecture
- training hyperparameters
- evaluation options


------------------------------------------------------------
9. Training Entry Point
------------------------------------------------------------

src/main.py

Main script used to start training.

Typical command:

python src/main.py ctdet \
    --exp_id exp1 \
    --dataset coco \
    --arch dla_34


------------------------------------------------------------
10. Inference Entry Point
------------------------------------------------------------

src/test.py

Used for model evaluation and inference.


------------------------------------------------------------
11. Planned Modifications for This Project
------------------------------------------------------------

This project will extend the baseline CenterNet detector
to incorporate structural priors.

Possible modification locations:

Prior modeling module

    src/lib/models/
    (new prior modeling component)

Detection fusion

    src/lib/detectors/ctdet.py

Training loss modification

    src/lib/models/losses.py

Experiment scripts

    experiments/


------------------------------------------------------------
12. Coding Rules for This Repository
------------------------------------------------------------

When modifying the codebase:

1. Avoid refactoring unrelated modules
2. Prefer adding new modules rather than modifying core logic
3. Document all modifications in CHANGELOG_DEV.md
4. Ensure training and inference scripts remain runnable