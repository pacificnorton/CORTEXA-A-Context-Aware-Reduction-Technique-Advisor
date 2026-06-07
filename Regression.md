# Regression Evaluation

## Overview

This page documents the experimental protocol used to evaluate CORTEXA in regression settings.

The objective of these experiments is not to identify the best regression model or the best data reduction technique in isolation. Instead, the goal is to evaluate whether the recommendations produced by CORTEXA remain aligned with user preferences across a wide range of dataset conditions and objective trade-offs.

The evaluation follows the same philosophy as the knowledge-base construction process described in the paper. Synthetic datasets are generated under controlled conditions, reduction methods are applied, predictive performance is measured, and the resulting recommendation behavior is analyzed.

---

# Experimental Design

## Why Synthetic Data?

The recommendation problem addressed by CORTEXA depends on dataset characteristics.

Evaluating the system only on a limited collection of real datasets would leave large portions of the technical space unexplored and could introduce biases related to specific domains.

Synthetic generation offers three advantages:

1. Complete control over dataset characteristics.
2. Reproducible experiments.
3. Exhaustive coverage of feasible dataset profiles.

This allows the evaluation to remain independent from any particular application domain while ensuring that all considered technical conditions can be represented.

---

# Dataset Generation

## Technical Profiles

Each dataset is generated from a predefined technical profile.

A profile specifies:

| Characteristic | Description |
|--------------|-------------|
| Relationship Structure | Linear or non-linear relationships |
| Distribution Shape | Normal or non-normal distributions |
| Feature Types | Numerical or mixed attributes |
| Number of Features | Low, medium, or high dimensionality |
| Number of Observations | Small, medium, or large datasets |

The Cartesian product of these characteristics defines the global technical space explored during evaluation.

Only profiles compatible with at least one reduction method are retained.

---

## Regression Target Construction

For each feasible profile, synthetic observations are generated according to the specified statistical properties.

The target variable is then created using a regression-generating mechanism consistent with the profile assumptions.

Examples include:

- Linear combinations of predictors
- Non-linear transformations
- Interaction effects
- Controlled noise injection

This procedure ensures that the generated datasets exhibit the intended complexity while preserving consistency with the corresponding technical profile.

Multiple datasets are generated for each profile in order to reduce randomness and improve robustness.

---



