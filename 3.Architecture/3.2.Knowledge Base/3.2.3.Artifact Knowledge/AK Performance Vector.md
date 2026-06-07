# Computation of the Performance Vector

## Performance ##
We have build 2 Model Assessment funcitons in order to evaluate the performance, one for each analytical task considered in this work.

### ***Classification***

Predictive performance for Classification is assessed by training Logistic Regression, Decision Tree, and XGBoost classifiers on both the original and reduced datasets using stratified 5-fold cross-validation. The five required metrics (Accuracy, Precision, Recall, F1-score, ROC-AUC) are averaged and converted into an accept/reject decision using the following rule-based scoring scheme:

- **Metric-level scoring.**  
  For each of the five metrics:
  - Add 2 points if $m \ge 0.80$  
    *(*high-quality performance*).
  - Add 1 point if $0.60 \le m < 0.80$  
    *(*acceptable performance*).

- **Precision–Recall balance.**  
  If $|\mathrm{Precision} - \mathrm{Recall}| < 0.10$, add 1 point  
  *(*ensures that the reduced data do not create strong class-balance distortions*).

- **Overfitting penalty.**  
  If all five metrics exceed $0.96$, subtract $2.5$ points  
  *(*very high scores commonly indicate overfitting*). 

- **Final decision.**  
  The model is accepted if the total score is at least $3$, and rejected otherwise  
  *(*minimal level ensuring that multiple aspects of performance remain preserved*). 

Among the three classifiers, only the best-performing one contributes to the final decision.

### ***Regression***

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

$$
Score_{reg} = Score_{R^2} + Score_{RMSE} + Score_{MAE}
$$

The contribution of the $R^2$ metric is defined as:

$$
Score_{R^2} =
\begin{cases}
0, & R^2 < 0.60 \\
R^2, & 0.60 \leq R^2 < 0.80 \\
2R^2, & R^2 \geq 0.80
\end{cases}
$$

The contribution of the RMSE metric is defined as:

$$
Score_{RMSE} =
\begin{cases}
0, & RMSE > 0.50 \\
(1-RMSE), & 0.25 < RMSE \leq 0.50 \\
2(1-RMSE), & RMSE \leq 0.25
\end{cases}
$$

The contribution of the MAE metric is defined as:

$$
Score_{MAE} =
\begin{cases}
0, & MAE > 0.50 \\
(1-MAE), & 0.25 < MAE \leq 0.50 \\
2(1-MAE), & MAE \leq 0.25
\end{cases}
$$

The final regression score is therefore obtained by aggregating the three metric-specific contributions:

$$
Score_{reg} =
Score_{R^2}
+
Score_{RMSE}
+
Score_{MAE}
$$

A reduction method is considered to preserve predictive performance when:

$$
Score_{reg} \geq 1.5
$$

This threshold prevents situations where excellent performance on a single metric compensates for severe weaknesses on the others. Consequently, accepted methods must simultaneously exhibit sufficient explanatory power, predictive accuracy, and numerical stability.

When multiple regressors are evaluated, the final performance assessment associated with a reduction method corresponds to the aggregation of the individual regression scores, providing a model-independent estimate of predictive preservation across different regression paradigms.



## Reduction Efficiency
We compare the 
$(F x O) - (F_r x O_r)$ 
where:

  - $F$ = Number of Features of the initial dataset
  - $O$ = Number of Observations of the initial dataset
  - $F_r$ = Number of Features of the reduced dataset
  - $O_r$ = Number of Observations of the reduced dataset

## Interpretability
  - **High:** The reduction process preserves original variables or instances, allowing straightforward explanation of results (e.g., MI, RFE, VT, Sampling methods).
  - **Medium:** The reduction produces transformed representations or aggregated structures that remain partially interpretable (e.g., PCA, MCA, FAMD, Cluster Sampling).

## Computational Cost
  - **Low:** Linear-time or near-linear algorithms.
  - **Medium:** Algorithms requiring pairwise statistics, clustering assignments, or matrix operations of moderate complexity.
  - **High:** Iterative optimization, decomposition methods, or repeated model training procedures.


| Method | Interpretability | Computational Cost |
|----------|----------|----------|
| PCA | Medium | Medium |
| MCA | Medium | Medium |
| FAMD | Medium | High |
| LDA | High | High |
| t-SVD | Medium | High |
| RFE | High | High |
| MI (Mixed) | High | Medium |
| MI (Numerical) | High | Medium |
| Variance Threshold (VT) | High | Low |
| Correlation Filter | High | Medium |
| Cluster Sampling (Mixed) | Medium | Medium |
| Cluster Sampling (Numerical) | Medium | Medium |
| Cluster Sampling (Categorical) | Medium | Medium |
| Stratified Sampling (Mixed) | High | Medium |
| Stratified Sampling (Numerical) | High | Medium |
| Stratified Sampling (Categorical) | High | Medium |
| Random Sampling (Mixed) | High | Low |
| Random Sampling (Numerical) | High | Low |
| Random Sampling (Categorical) | High | Low |
| Systematic Sampling (Mixed) | High | Low |
| Systematic Sampling (Numerical) | High | Low |
| Systematic Sampling (Categorical) | High | Low |

