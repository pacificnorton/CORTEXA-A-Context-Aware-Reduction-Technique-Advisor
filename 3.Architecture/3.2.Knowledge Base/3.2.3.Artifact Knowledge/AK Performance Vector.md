### Computation of the Performance Vector

- **Performance**
We have build 2 Model Assessment funcitons in order to evaluate the performance, one for each analytical task considered in this work.

***Classification***

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

