### Computation of the Performance Vector

- **Performance**
We have build 2 Model Assessment funcitons in order to evaluate the performance, one for each analytical task considered in this work.

***Classification***

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



***Regression***

- **Reduction Efficiency**


- **Interpretability**
  - **High:** The reduction process preserves original variables or instances, allowing straightforward explanation of results (e.g., MI, RFE, VT, Sampling methods).
  - **Medium:** The reduction produces transformed representations or aggregated structures that remain partially interpretable (e.g., PCA, MCA, FAMD, Cluster Sampling).

- **Computational Cost**
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

