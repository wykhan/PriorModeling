# System Rules for AI-Assisted Development

This document defines the behavioral rules for AI-assisted coding
within this repository.

All AI agents (Codex CLI, ChatGPT, or other tools) must follow these
rules when modifying the codebase.

The goal is to ensure:

- stable development
- minimal unintended code changes
- reproducible experiments
- clear development history


------------------------------------------------------------
1. General Principles
------------------------------------------------------------

AI-assisted development must follow the principle:

SMALL, CONTROLLED, AND TRACEABLE CHANGES.

Each task should:

- modify only a small part of the codebase
- introduce minimal structural changes
- preserve the original CenterNet implementation


------------------------------------------------------------
2. Allowed Types of Modifications
------------------------------------------------------------

AI agents are allowed to perform the following operations:

1. Add new modules related to prior modeling.

Example:

src/lib/models/prior_model.py

2. Add new experiment scripts.

Example:

experiments/prior_experiments/

3. Modify specific modules required for prior integration.

Typical locations:

src/lib/models/
src/lib/detectors/
src/lib/models/losses.py

4. Add configuration options in opts.py.

5. Add documentation and comments.


------------------------------------------------------------
3. Forbidden Operations
------------------------------------------------------------

AI agents must NOT perform the following operations.

1. Large-scale refactoring of the repository.

2. Deleting existing CenterNet code.

3. Changing baseline logic without explicit task instructions.

4. Moving core files or directories.

5. Renaming existing modules.

6. Introducing hard-coded dataset paths.

Example of forbidden pattern:

dataset_path = "/home/user/dataset"

Instead use configuration parameters.

7. Modifying more than one major module in a single task.


------------------------------------------------------------
4. Maximum Change Size
------------------------------------------------------------

Each AI coding task must satisfy the following limits.

Maximum number of modified files:

≤ 5 files

Maximum diff size:

≤ 400 lines

If the change exceeds this size, the task must be split into
multiple smaller tasks.


------------------------------------------------------------
5. Code Quality Requirements
------------------------------------------------------------

All AI-generated code must satisfy the following requirements.

1. Every new function must include a docstring.

Example:

def compute_position_prior(...):
    """
    Compute spatial prior distribution of object centers.
    """

2. Variable names must be descriptive.

Avoid names like:

x1, x2, tmp, data2

Prefer names like:

center_heatmap
bbox_width_prior

3. Follow existing repository style conventions.


------------------------------------------------------------
6. Dataset Handling Rules
------------------------------------------------------------

Dataset paths must NOT be hard-coded.

Correct approach:

Use configuration arguments.

Example:

--dataset_path

Dataset loaders must remain compatible with the CenterNet pipeline.


------------------------------------------------------------
7. Experiment Safety Rules
------------------------------------------------------------

All modifications must preserve the ability to run:

Baseline training

Example command:

python src/main.py ctdet \
    --exp_id baseline \
    --dataset <dataset_name>

Inference

python src/test.py ctdet \
    --load_model <checkpoint>

No modification should break baseline functionality.


------------------------------------------------------------
8. Documentation Rules
------------------------------------------------------------

Every completed coding task must include documentation updates.

Required updates:

CHANGELOG_DEV.md

- description of change
- files modified
- verification steps

doc/assumptions.md

- new assumptions
- dataset changes
- environment dependencies


------------------------------------------------------------
9. Verification Requirement
------------------------------------------------------------

Before a task is considered complete, AI must verify:

1. The code runs without syntax errors.

2. Training can start successfully.

3. Inference produces detection outputs.

4. No unrelated files are modified.


------------------------------------------------------------
10. Git Workflow Rules
------------------------------------------------------------

All development must follow a branch-based workflow.

Example:

main

exp/prior-modeling

AI agents must never push directly to the main branch.

All changes must be committed to experiment branches.


------------------------------------------------------------
11. Task Execution Protocol
------------------------------------------------------------

Each development task must follow the workflow:

1. Read project context files:

CONTEXT/project_brief.md
CONTEXT/repo_map.md
CONTEXT/experiment_matrix.md

2. Follow task instructions in:

PROMPTS/task_xxx.md

3. Implement minimal required code changes.

4. Verify code execution.

5. Update documentation.

6. Record the change in CHANGELOG_DEV.md.


------------------------------------------------------------
12. Safety Rule for Research Code
------------------------------------------------------------

This repository supports academic research.

Therefore:

1. Experimental results must remain reproducible.

2. All hyperparameters must be recorded.

3. Random seeds must be fixed.

4. No hidden modifications are allowed.


------------------------------------------------------------
13. If Uncertain
------------------------------------------------------------

If the AI agent is uncertain about a modification:

DO NOT modify the code.

Instead:

- ask for clarification
- propose a plan before implementation