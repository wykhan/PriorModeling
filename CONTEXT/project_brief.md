# Project Brief

## 1. Project Title

Prior Modeling for Millimeter-Wave Concealed Object Detection  
based on CenterNet Framework


---

## 2. Background

Millimeter-wave (mmWave) imaging is widely used in security screening
to detect concealed objects under clothing. Compared with natural
images, mmWave images exhibit several challenges:

- low spatial resolution
- strong noise and clutter
- limited dataset size
- unstable detection confidence

Most existing detection frameworks (e.g., CenterNet, YOLO) treat object
detection as a purely data-driven task and ignore domain knowledge about
object spatial distribution.

However, in practical security screening scenarios:

- concealed objects usually appear in specific body regions
- object sizes follow constrained distributions
- certain spatial configurations are more likely than others

These **structural priors** can be exploited to improve detection
stability and reliability.


---

## 3. Research Goal

The goal of this project is to integrate **structural prior modeling**
into the CenterNet detection framework for mmWave concealed object
detection.

Specifically, we aim to model:

- **Position Prior**  
  Probability distribution of object locations on the human body.

- **Size Prior**  
  Probability distribution of object bounding box sizes.

These priors will be incorporated into the detection pipeline to refine
object probability estimation.


---

## 4. Probabilistic Formulation

Standard detector:

P(O, C | I)

Where:

I : input mmWave image  
O : object location  
C : object class  

Our proposed formulation introduces latent structural prior Z:

P(O, C | I, Z)

Where Z represents structural priors learned from dataset statistics.


---

## 5. Implementation Strategy

Baseline detector:

CenterNet (official implementation)

Key modifications include:

1. Prior estimation module
2. Prior probability map generation
3. Fusion of prior probability with detector output
4. Evaluation of prior-aware detection performance


---

## 6. Experimental Objectives

The project will implement the following experiment groups:

Baseline

CenterNet without structural prior

Experiment 1

CenterNet + Position Prior

Experiment 2

CenterNet + Size Prior

Experiment 3

CenterNet + Combined Structural Prior


Evaluation metrics include:

- AP
- AP50
- detection stability 
- calibration 


---

## 7. Expected Contributions

1. A structural prior modeling framework for mmWave detection

2. Integration of prior distributions into CenterNet

3. Empirical validation on mmWave security datasets

4. Improved detection robustness under noisy imaging conditions


---

## 8. Scope

This repository focuses on:

- detection model modification
- prior modeling
- experiment implementation

Out of scope:

- radar signal processing
- mmWave image reconstruction
- hardware system design


---

## 9. Reproducibility

All experiments should be reproducible through:

- version-controlled code
- fixed experiment configuration
- logged training parameters


---

## 10. Target Outcome

A research prototype capable of demonstrating that
**structural prior modeling improves concealed object detection
performance in mmWave images.**

The implementation will support the experiments required for the
associated research paper.