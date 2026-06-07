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

## Performance Assessment Function ($g_{r,1}$)

The objective of the performance assessment function is to evaluate whether a reduction method preserves the predictive capability of the original dataset for regression tasks after reduction.

For each generated dataset, the original data and the reduced data obtained after applying a reduction method are used to train and evaluate two regression models:

- Linear Regression
- XGBoost Regressor

The model pool is intentionally heterogeneous. Linear Regression captures linear relationships and provides a baseline for interpretable predictive behavior, whereas XGBoost captures complex nonlinear dependencies and interaction effects. Evaluating both models ensures that reduction methods are assessed across different modeling assumptions.

The predictive performance of each regressor is evaluated using three complementary metrics:

- Coefficient of Determination (\(R^2\))
- Root Mean Squared Error (RMSE)
- Mean Absolute Error (MAE)

These metrics jointly assess explanatory power, predictive accuracy, and numerical stability. The coefficient of determination measures the proportion of variance explained by the model, RMSE penalizes large prediction errors, and MAE quantifies the average absolute deviation while remaining more robust to outliers.

A composite regression score is then computed as:

\[
Score_{reg}
=
Score_{R^2}
+
Score_{RMSE}
+
Score_{MAE}
\]

The contribution of the \(R^2\) metric is defined as:

\[
Score_{R^2}
=
\begin{cases}
0, & R^2 < 0.60 \\
R^2, & 0.60 \leq R^2 < 0.80 \\
2R^2, & R^2 \geq 0.80
\end{cases}
\]

The contribution of the RMSE metric is defined as:

\[
Score_{RMSE}
=
\begin{cases}
0, & RMSE > 0.50 \\
(1-RMSE), & 0.25 < RMSE \leq 0.50 \\
2(1-RMSE), & RMSE \leq 0.25
\end{cases}
\]

The contribution of the MAE metric is defined as:

\[
Score_{MAE}
=
\begin{cases}
0, & MAE > 0.50 \\
(1-MAE), & 0.25 < MAE \leq 0.50 \\
2(1-MAE), & MAE \leq 0.25
\end{cases}
\]

The final regression score is therefore obtained by aggregating the three metric-specific contributions:

\[
Score_{reg}
=
Score_{R^2}
+
Score_{RMSE}
+
Score_{MAE}
\]

A reduction method is considered to preserve predictive performance when:

\[
Score_{reg} \geq 1.5
\]

This threshold prevents situations where excellent performance on a single metric compensates for severe weaknesses on the others. Consequently, accepted methods must simultaneously exhibit sufficient explanatory power, predictive accuracy, and numerical stability.

When multiple regressors are evaluated, the final performance assessment associated with a reduction method corresponds to the aggregation of the individual regression scores, providing a model-independent estimate of predictive preservation across different regression paradigms.

