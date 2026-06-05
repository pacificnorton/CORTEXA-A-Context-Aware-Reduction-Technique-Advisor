## This is the resulting structure of the knowledge base:

---

![Class Diagram](../images/ClassDiagram.jpeg)

---

### The knowledge base is alimented through this process:

![Algo](../images/Algo1.jpeg)
![](../images/Algo2.jpeg)

We construct a unified knowledge base $\mathcal{K}$ that characterizes the behaviour of data reduction techniques across controlled data conditions. We begin by selecting a comprehensive set $M$ of 13 methods covering dimensionality reduction, feature selection, and sampling, alongside their 63 admissible ordered pairwise combinations. This ensures balanced coverage of vertical, horizontal, and hybrid reduction paradigms without prioritizing any single family.

A global technical space $\Omega$ is defined as the Cartesian product of five dataset characteristics: relationship linearity $L \in \Omega_L$, distribution normality $N \in \Omega_N$, feature-type composition $C \in \Omega_C$, number of features $F \in \Omega_F$, and number of observations $O \in \Omega_O$. Each element of this space represents a dataset profile $p \in \Omega$. For every reduction method $m \in M$, an applicability profile $P(m) \subseteq \Omega$ is constructed by specifying the admissible values for each characteristic, reflecting constraints such as numerical-only requirements, linearity assumptions, or incompatibility with mixed-type features.

The intersection of these two structures yields the set of feasible dataset profiles $\mathcal{P}_{\mathrm{feas}}$, i.e., data conditions for which at least one method is applicable. Each feasible profile $p \in \mathcal{P}_{\mathrm{feas}}$ is associated with its set of compatible reduction methods $C_p$, and these associations are stored in the compatibility map $\mathcal{C}$. This ensures methodological coherence and prevents the evaluation of techniques on unsuitable datasets.

For each feasible profile, a synthetic dataset $D$ satisfying the specified conditions is generated, forming the synthetic universe $\mathcal{D}$. All compatible reduction methods $m \in C_p$ are then applied to these datasets, and a multi-objective performance vector is computed for each reduced output.

To identify recurrent behavioural patterns, all performance vectors are assembled in the set $G$ and clustered in the multi-objective space. Each cluster is represented by a centroid summarising its characteristic trade-off structure, and method frequencies within clusters quantify the stability of techniques across similar regimes.

The Pareto-optimal centroids—those not dominated across the four objectives—form the final abstraction layer of the knowledge base $\mathcal{P}$, providing a non-redundant set of efficient behavioural profiles. They retain the intrinsic trade-offs without arbitrary weighting and constitute the basis on which the online recommendation module aligns dataset characteristics with user priorities.
