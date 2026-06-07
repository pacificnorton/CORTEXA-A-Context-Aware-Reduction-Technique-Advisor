# Resource Efficiency and Scalability Indicators

## Overview

This document describes the rationale and computational framework used to derive the **Resource Efficiency** and **Scalability** indicators proposed in CORTEXA.

These indicators were introduced as complementary decision-support mechanisms intended to provide users with additional information regarding the practical implications of selecting a data reduction technique. While the recommendation engine itself optimizes reduction methods according to the objectives stored in the knowledge base, users may also wish to understand how recommended techniques differ in terms of computational requirements and their expected behaviour when applied to increasingly large datasets.

To address this need, we perform a theoretical complexity analysis of all considered reduction methods and derive two interpretable indicators that summarize their relative computational characteristics. These indicators are not involved in the recommendation process itself. Instead, they provide supplementary information intended to support informed decision-making.

---

# Motivation

Data reduction techniques are traditionally evaluated according to their analytical performance, such as their ability to preserve information, improve predictive accuracy, or reduce dimensionality. However, practical deployments often involve additional considerations related to computational resources.

Two methods may produce similar analytical results while exhibiting substantially different computational requirements. Likewise, a method that performs efficiently on a small dataset may become impractical when applied to larger datasets due to its growth rate.

These considerations are particularly relevant in modern data-intensive environments, where computational resources represent both operational and environmental concerns. Increasing processing requirements generally imply longer execution times, higher hardware utilization, greater energy consumption, and potentially higher infrastructure costs.

Directly measuring such quantities, however, presents several challenges. Execution times depend on hardware configurations, software implementations, operating systems, and parallelization strategies. Similarly, energy consumption and financial costs vary across infrastructures and deployment environments. As a result, these measurements are difficult to generalize and cannot easily be incorporated into a reproducible comparison framework.

For this reason, CORTEXA adopts a complexity-based assessment strategy. By relying on theoretical operation counts derived from the literature, the framework provides a hardware-independent approximation of the computational effort associated with each reduction method.

The resulting indicators do not directly measure energy consumption, carbon emissions, or monetary cost. Instead, they provide proxies that can help users identify methods that are likely to require fewer computational resources or exhibit more favourable scaling behaviour as data volumes increase.

Consequently, the proposed indicators may provide indirect insights regarding both environmental and economic considerations while remaining independent from specific execution environments.

---

# Complexity-Based Assessment

The proposed assessment relies on the theoretical computational complexity of each reduction technique.

For every considered method, a complexity function was derived from the literature and expressed as an operation-count model. These functions estimate the number of elementary operations required by the algorithm as a function of dataset characteristics such as the number of observations, the number of features, the number of categories, or the number of classes.

Formally, for a reduction method (r), the complexity model can be represented as

[
C_r = f_r(n,p,\ldots),
]

where (C_r) denotes the estimated number of operations required by the method and (f_r) represents its complexity function.

The exact variables involved depend on the characteristics of each reduction technique. For example, some methods depend primarily on the number of observations, while others are more strongly influenced by the number of features or by additional parameters specific to their underlying algorithms.

Rather than attempting to estimate execution times, the framework evaluates these complexity functions under controlled dataset conditions. This approach provides a reproducible and implementation-independent comparison of computational requirements across methods.

Here are the algorithmical complexities considered in this work :

| Method | Standard Complexity | Variables | Basic Justification | More Precise Complexity | Why / Justification |
|----------|----------|----------|----------|----------|----------|
| PCA | \(O(\min(n^2p,\;np^2))\) | \(n\): samples, \(p\): features | PCA is computed via SVD of the centered data matrix. | \(O(\min(n^2p,\;np^2))\) | The standard formula is already the precise one for dense full SVD. It comes directly from LAPACK/Golub-SVD complexity analysis. |
| MCA | \(O(nK^2)\) | \(n\): samples, \(K\): binary variables after one-hot encoding | MCA performs SVD on the indicator matrix. | \(O(\min(n^2K,\;nK^2))\) | More precise because SVD cost depends on whether \(n>K\) or \(K>n\). The earlier formula assumes \(K<n\). |
| FAMD | \(O(nK^2 + np^2)\) | \(K\): encoded categorical dimensions, \(p\): numerical features | FAMD combines MCA and PCA. | \(O(\min(n^2K,\;nK^2)+\min(n^2p,\;np^2))\) | More precise because both the MCA and PCA stages have shape-dependent SVD costs. |
| LDA | \(O(cp^2+p^3)\) | \(c\): classes, \(p\): features | Compute class statistics and eigendecomposition. | \(O(np^2+cp^2+p^3)\) | More precise because covariance estimation requires scanning all \(n\) samples before eigendecomposition. |
| Truncated SVD | \(O(npk)\) | \(n\): samples, \(p\): features, \(k\): retained components | Iterative decomposition of top singular vectors. | \(O(qnpk)\) | More precise because randomized/Lanczos SVD needs \(q\) power iterations (typically \(q=2\)–10) to converge. |
| RFE | \(O(k\,T_{\text{model}}(n,p))\) | \(k\): elimination iterations, \(T_{\text{model}}\): training cost | Retrains model at each elimination step. | \(O\!\left(\sum_{i=1}^{k} T_{\text{model}}(n,p_i)\right)\) | More precise because the number of features decreases at each iteration (\(p_1>p_2>\cdots\)). Training cost is not constant. |
| MI | \(O(p\,n\log n)\) | \(p\): features, \(n\): samples | One MI score per feature. | \(O\!\left(\sum_{j=1}^{p} n\log n\right)=O(p\,n\log n)\) | Same asymptotic result, but explicitly shows independent per-feature computation. |
| VT | \(O(np)\) | \(n\): samples, \(p\): features | Compute feature variances. | \(O(np)+O(p)\) | Additional threshold comparison step exists, though dominated by variance computation. |
| Correlation Filter | \(O(p^2n)\) | \(p\): features, \(n\): samples | Compute all pairwise correlations. | \(O\!\left(\frac{p(p-1)}{2}n\right)\) | More precise because there are exactly \(\frac{p(p-1)}{2}\) feature pairs. |
| Cluster Sampling | \(O(N)+O(c\,n_c)\) | \(N\): population, \(c\): clusters, \(n_c\): sampled items per cluster | Assign clusters then sample. | \(O(N+c+n_c\,c)\) | More precise because cluster indexing/selection has separate overhead before sample extraction. |
| Stratified Sampling | \(O(N+g\,n_g)\) | \(g\): strata, \(n_g\): sampled per stratum | Assign strata then sample. | \(O\!\left(N+g+\sum_{i=1}^{g} n_i\right)\) | More precise because strata may have unequal sample sizes. |
| Random Sampling | \(O(n)\) | \(n\): sample size | Random draw. | \(O(n)\) or \(O(N)\) | Depends on implementation: direct index generation vs shuffling the full population. |
| Systematic Sampling | \(O(n)\) | \(n\): sample size | Select every \(k\)-th item. | \(O(n)+O(N_{\text{sort}})\) | If the population is already ordered, only selection is needed; otherwise preprocessing/sorting may dominate. |

---

# Scenario Construction

Computational complexity is inherently dependent on dataset characteristics. Evaluating methods under a single dataset configuration would therefore provide only a partial view of their behaviour.

To obtain a broader perspective, complexity estimates are computed across a collection of representative scenarios covering different dataset sizes and feature configurations.

The experimental design combines three feature configurations with three observation levels, resulting in a total of nine scenarios.

The feature configurations represent increasing levels of structural complexity:

| Configuration | Features ((p)) | Categories ((K)) | Classes ((c)) |
| ------------- | -------------- | ---------------- | ------------- |
| Small         | 10             | 5                | 2             |
| Medium        | 50             | 20               | 5             |
| Large         | 200            | 80               | 10            |

For each feature configuration, three observation levels are considered:

[
n = N \in {100,;5000,;20000}.
]

The resulting design enables the assessment of both moderate and substantial increases in dataset size while preserving consistency across reduction methods.

By evaluating all methods across the same collection of scenarios, the framework can compare not only their absolute computational requirements but also the way these requirements evolve when the data scale changes.

---

# Relative Complexity Analysis

The raw operation counts produced by complexity functions can differ by several orders of magnitude. Direct comparison of these values is therefore difficult to interpret.

To facilitate analysis, CORTEXA evaluates all methods relative to a common reference.

Random Sampling is selected as the baseline method due to its simplicity and comparatively low computational requirements. For each scenario, the operation count of every reduction technique is compared against the operation count of Random Sampling.

Let (C_r) denote the operation count of method (r) and (C_{\mathrm{RS}}) the operation count of Random Sampling for the same scenario. The relative complexity ratio is computed as

[
R_r=\frac{C_r}{C_{\mathrm{RS}}}.
]

This ratio expresses the computational effort required by a method relative to the baseline.

A value of

[
R_r = 1
]

indicates complexity equivalent to Random Sampling, whereas

[
R_r = 100
]

indicates a method requiring approximately one hundred times more operations under the same conditions.

The analysis is repeated independently for each of the nine scenarios. The resulting ratios provide a consistent basis for comparing reduction techniques despite large differences in absolute operation counts.

In practice, the computational workflow implemented in CORTEXA follows four main steps:

1. The operation count of every reduction method is computed for each scenario using its theoretical complexity function.

2. The operation count of Random Sampling is used as a baseline reference.

3. Relative complexity ratios are computed for all methods and scenarios.

4. The resulting ratios are aggregated to derive the Resource Efficiency and Scalability indicators presented to the user.

This procedure transforms heterogeneous complexity functions into a common comparison framework that remains independent of implementation details and execution environments.

---

# Logarithmic Normalization

Even after normalization by the baseline, complexity ratios often span several orders of magnitude.

For example, some methods may exhibit ratios close to one, while others may exceed several thousand. Using these values directly would produce highly skewed visualizations and make meaningful comparisons difficult.

To address this issue, a logarithmic transformation is applied to the relative complexity ratios.

For a method (r), the transformed value is computed as

[
L_r = \log_{10}(R_r).
]

The logarithmic scale preserves relative differences while compressing extreme values into a more interpretable range.

For example,

[
\log_{10}(1)=0,
]

[
\log_{10}(10)=1,
]

[
\log_{10}(100)=2,
]

[
\log_{10}(1000)=3.
]

Consequently, equal distances on the transformed scale correspond to equal multiplicative increases in computational effort.

This normalization forms the basis of the visual analyses presented in this repository and facilitates the interpretation of methods whose computational requirements differ by several orders of magnitude.

---

# Resource Efficiency and Scalability Indicators

## Resource Efficiency Indicator

### Motivation

The optimization process used by CORTEXA relies on four objectives extracted from the literature and stored within the knowledge base. Although these objectives characterize the expected behaviour of reduction methods, they do not explicitly communicate the computational resources required to execute a recommendation.

In practical settings, resource consumption increasingly influences method selection. Large-scale analytical workflows may involve millions of observations, repeated model retraining, cloud-based infrastructures, or constrained computing environments. Under these conditions, two reduction methods exhibiting similar analytical performance may differ substantially in their computational requirements.

To provide additional decision support, CORTEXA introduces a **Resource Efficiency Indicator**. Unlike the computational-cost objective used during recommendation, which is represented as a binary characteristic derived from the literature, this indicator provides a quantitative estimate of the relative computational effort associated with a recommended method.

The indicator is not involved in the recommendation process itself. Instead, it is computed after recommendation generation and serves as an explanatory proxy that helps users assess the potential resource implications of the proposed alternatives.

### Why Complexity-Based Estimation?

Direct measurements of execution time, energy consumption, or financial cost depend on numerous external factors, including:

* hardware architecture,
* processor specifications,
* memory availability,
* implementation details,
* software libraries,
* parallelization strategies.

Such measurements would therefore be difficult to generalize across environments.

To maintain hardware independence, CORTEXA relies on theoretical algorithmic complexity. For each reduction method, an operation-count function was derived from the literature and expressed as a function of dataset characteristics such as the number of observations and variables.

The resulting operation counts provide a machine-independent estimate of computational effort and enable consistent comparisons between methods.

---

## Scenario-Based Complexity Assessment

Because computational complexity depends on dataset characteristics, a single complexity value cannot adequately describe a reduction method.

To capture this variability, the evaluation framework considers multiple representative dataset configurations generated from combinations of:

* dataset size ((n)),
* population size ((N)),
* feature dimensionality,
* analytical task characteristics.

Three feature-complexity profiles (small, medium, and large) are combined with three observation scales, producing nine representative scenarios.

For each scenario, the theoretical operation count of every reduction method is computed using its corresponding complexity model.

This strategy allows the analysis to capture both differences between methods and variations induced by dataset growth.

---

## Relative Complexity Computation

Absolute operation counts can differ by several orders of magnitude, making direct comparison difficult.

To facilitate interpretation, all methods are evaluated relative to a baseline reference method:

[
r_i=\frac{C_i}{C_{\text{baseline}}}
]

where:

* (C_i) denotes the operation count of method (i),
* (C_{\text{baseline}}) corresponds to the operation count of random sampling.

The ratio (r_i) indicates how many times more computational effort a method requires compared with the baseline.

For example:

[
r_i = 100
]

means that the method is expected to require approximately one hundred times more elementary operations than random sampling under the same dataset conditions.

---

## Logarithmic Normalization

The computed ratios may span several orders of magnitude. Direct visualization would therefore compress most methods into a narrow region while allowing only the most expensive techniques to dominate the scale.

To improve interpretability, a logarithmic transformation is applied:

s_i = \log_{10}(r_i)

where:

* (r_i) is the relative complexity ratio,
* (s_i) is the normalized complexity score.

This transformation preserves ordering while reducing the influence of extreme values.

For instance:

| Relative Complexity | Logarithmic Score |
| ------------------- | ----------------- |
| 1×                  | 0                 |
| 10×                 | 1                 |
| 100×                | 2                 |
| 1000×               | 3                 |

The normalized values form the basis of the visual indicators presented to users.

---

## Relation to Environmental and Economic Proxies

The Resource Efficiency Indicator should not be interpreted as a direct measure of energy consumption or monetary cost. However, computational effort is strongly related to both quantities.

Methods requiring fewer operations generally:

* consume fewer computational resources,
* execute faster,
* require less infrastructure capacity,
* reduce repeated processing costs.

Consequently, Resource Efficiency can be viewed as an indirect proxy for both environmental and operational impacts.

In CORTEXA, this relationship is intentionally presented as an approximation rather than a quantitative claim. The indicator therefore informs users about the likely resource implications of a recommendation without requiring hardware-specific measurements.

---

## Scalability Indicator

### Motivation

While Resource Efficiency describes the computational effort associated with a specific dataset configuration, it does not indicate how that effort evolves as datasets grow.

Two methods may exhibit similar computational requirements on small datasets while behaving very differently on larger ones. A recommendation that appears efficient today may become impractical when data volumes increase.

For this reason, CORTEXA complements Resource Efficiency with a **Scalability Indicator**.

### Definition

The scalability assessment evaluates how rapidly computational effort increases across the predefined scenario space.

Let:

[
C_{\text{small}}
]

denote the operation count obtained in the smallest scenario and

[
C_{\text{large}}
]

the operation count obtained in the largest scenario.

The scalability growth factor is defined as

[
G=\frac{C_{\text{large}}}{C_{\text{small}}}
]

A lower value of (G) indicates that computational requirements increase slowly as datasets grow, whereas higher values reveal methods whose resource demands escalate rapidly.

### Interpretation

Resource Efficiency and Scalability provide complementary information:

| Indicator           | Question Answered                                    |
| ------------------- | ---------------------------------------------------- |
| Resource Efficiency | How computationally demanding is the method today?   |
| Scalability         | How rapidly will this demand increase as data grows? |

A method may therefore:

* exhibit high resource efficiency but poor scalability,
* exhibit moderate resource efficiency but excellent scalability,
* perform well on both dimensions,
* perform poorly on both dimensions.

Together, these indicators offer a more complete view of long-term computational sustainability.

---

# Visualization Analysis

The computational assessment generates three complementary visualizations intended for different levels of analysis.

![Visualization Analysis](./images/Complexity.jpeg)

## Heatmap Analysis

The heatmap provides a global overview of relative computational complexity across all methods and scenarios.

Each cell represents the complexity ratio between a reduction method and the baseline method for a given scenario. Color intensity increases with computational demand using logarithmic scaling.

The heatmap is particularly useful for:

* identifying consistently efficient methods,
* detecting methods that become expensive only under specific conditions,
* comparing entire families of reduction techniques.

Because it summarizes all scenarios simultaneously, this visualization is primarily intended for research analysis and validation rather than end-user presentation.

---

## Relative Complexity Comparison

The grouped comparison chart displays the relative complexity ratios obtained across scenarios.

This visualization emphasizes the magnitude of differences between methods and highlights how rankings evolve as dataset characteristics change.

It enables the identification of:

* methods with stable computational behaviour,
* methods highly sensitive to dataset growth,
* large complexity gaps between competing alternatives.

Compared with the heatmap, the grouped chart provides a clearer quantitative interpretation of relative computational effort.

---

## Scalability Analysis

The scalability plot illustrates how each reduction method evolves across increasing dataset scales.

Rather than focusing on absolute complexity levels, this visualization emphasizes growth behaviour.

Methods with flatter trajectories demonstrate better scalability because their computational requirements increase more slowly as datasets expand.

Conversely, steep trajectories indicate algorithms whose resource demands grow rapidly and may become impractical for large-scale applications.

This visualization is particularly valuable for understanding long-term behaviour and justifying the Scalability Indicator proposed in CORTEXA.

