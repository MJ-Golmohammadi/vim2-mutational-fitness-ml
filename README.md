# VIM-2 Mutational Fitness Prediction

**Leakage-aware, position-aware machine learning for predicting VIM-2 mutational fitness from sequence, biochemical, evolutionary, and structural features.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)

---

## Overview

This repository contains the computational workflows, feature-engineering procedures, machine-learning analyses, validation frameworks, statistical analyses, and visualization scripts developed for the prediction of **VIM-2 metallo-β-lactamase mutational fitness**.

The study investigates whether mutation-associated fitness can be predicted from interpretable molecular features and whether explicit sequence-position information improves model generalization to **previously unseen sequence positions**.

A central methodological component of this work is a **leakage-aware, position-disjoint evaluation framework**, in which mutations from the same sequence position are prevented from appearing in both training and outer-test sets. This provides a more stringent assessment of generalization than conventional random-split evaluation.

---

## Research Objective

The primary objective is to characterize and predict the phenotypic consequences of VIM-2 amino-acid substitutions using complementary molecular information.

The machine-learning framework evaluates the contribution of:

- Structural context
- Local sequence context
- Biochemical and physicochemical properties
- Evolutionary substitution information
- Side-chain properties
- Hydrogen-bond-related features
- Sequence-position information

The study further evaluates whether explicit sequence-position information provides additional predictive value when tested under a strict position-disjoint validation scheme.

---

## Model Architectures

Two predefined feature architectures are compared throughout the primary analysis:

### Model A — No Position

The baseline architecture uses molecular, sequence-derived, biochemical, evolutionary, and structural features without explicit sequence-position information.

### Model B — With Position

The position-aware architecture uses the same feature framework while additionally incorporating explicit `Sequence_Position`.

The comparison is designed to determine whether sequence-position information contributes to predictive performance beyond the molecular characteristics of individual substitutions.

---

## Phenotypic Conditions

The dataset contains nine experimental phenotype conditions spanning multiple antibiotic selection environments:

- Ampicillin — 2 μg/mL — 25°C
- Ampicillin — 16 μg/mL — 25°C
- Ampicillin — 128 μg/mL — 25°C
- Ampicillin — 2 μg/mL — 37°C
- Ampicillin — 16 μg/mL — 37°C
- Ampicillin — 128 μg/mL — 37°C
- Cefotaxime — 0.5 μg/mL — 37°C
- Cefotaxime — 4 μg/mL — 37°C
- Meropenem — 0.031 μg/mL — 37°C

---

## Dataset

The analysis comprises:

- **5,016 mutation observations**
- **266 sequence positions**
- **9 experimental phenotype conditions**

The original experimental measurements and associated metadata are retained separately from derived machine-learning features.

Target-associated uncertainty measurements are treated as uncertainty information rather than independent predictive phenotypes and are not used as predictors of their corresponding targets.

Identifiers, fold assignments, and other leakage-prone metadata are excluded from model predictors.

---

## Feature Framework

The predictive feature space integrates complementary molecular descriptors designed to capture different aspects of mutation-associated functional change.

### Structural Features

Structural descriptors characterize the spatial and physicochemical environment surrounding each mutated residue, including measures related to:

- Solvent accessibility
- Structural distances
- Active-site context
- Metal-site proximity
- Loop regions
- Local structural environment
- Local packing and contact characteristics
- Structural flexibility-related descriptors

### Local Sequence Features

Sequence-derived descriptors capture the local context surrounding each substitution, including information from neighboring residues and sequence-level properties.

### Biochemical Features

Mutation-level biochemical descriptors characterize changes associated with:

- Hydrophobicity
- Charge
- Molecular weight
- Polarity
- Amino-acid physicochemical properties
- Evolutionary substitution preferences

### Evolutionary Features

Evolutionary substitution information is incorporated using substitution scores such as **BLOSUM62** to represent the evolutionary plausibility of individual amino-acid replacements.

### Side-Chain and Hydrogen-Bond Features

Additional descriptors characterize changes in side-chain properties and hydrogen-bonding capacity introduced by amino-acid substitutions.

### Position Feature

For Model B, explicit `Sequence_Position` is incorporated to evaluate whether positional information provides predictive information beyond mutation-specific molecular descriptors.

---

## Validation Framework

The primary evaluation uses a **nested, position-aware cross-validation framework** designed to prevent positional leakage.

### Position-Disjoint Outer Evaluation

Sequence positions are partitioned into disjoint outer folds.

Consequently:

> Mutations belonging to a sequence position assigned to an outer test fold are not available during model training for that fold.

This ensures that the primary evaluation measures the ability of the model to generalize to **sequence positions not observed during training**.

### Nested Model Selection

Hyperparameter optimization is performed within the training data rather than using the outer test data.

The final outer-test evaluation therefore provides an independent assessment of predictive performance.

### Primary Metrics

Model performance is evaluated using:

- R²
- RMSE
- MAE
- Spearman correlation

Out-of-fold predictions are pooled across outer folds for the primary performance assessment.

---

## Secondary Random-Split Benchmark

A conventional random 5-fold cross-validation benchmark is included as a secondary analysis.

The random-split evaluation is not used for primary hyperparameter optimization or architecture selection. Its purpose is to provide a benchmark against a less stringent validation strategy.

This distinction allows the difference between conventional random-split performance and position-disjoint performance to be evaluated without compromising the primary leakage-safe framework.

---

## Citation

If you use this repository, please cite the associated manuscript.

**Preprint**

*https://www.researchsquare.com/article/rs-11081248/v1*

---

## Correspondence

**Mohammad Javad Golmohammadi**

University of Tehran

Tehran, Iran

ORCID: https://orcid.org/0000-0002-9277-0023

Email: Mohammad.jg75@gmail.com

---
---

## Analysis Workflow

The computational workflow proceeds through the following major stages:

```text
Data Validation
      │
      ▼
Exploratory Analysis
      │
      ▼
Feature Construction
      │
      ▼
Advanced Structural / Molecular Features
      │
      ▼
Model Development & Hyperparameter Optimization
      │
      ▼
Position-Aware Nested Outer-Test Evaluation
      │
      ├──────────────► Random-Split Benchmark
      │
      ▼
Statistical Comparison
      │
      ▼
Robustness Analysis
      │
      ▼
Final Figures & Manuscript Outputs
