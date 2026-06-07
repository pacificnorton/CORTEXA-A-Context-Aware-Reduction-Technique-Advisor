# Lexicographic Selection

## Why Lexicographic Selection?

Many recommendation systems rely on weighted-sum optimization, where multiple objectives are aggregated into a single score through a set of numerical weights.

Although simple to implement, weighted approaches present several limitations in the context of data-reduction method selection:

* Users often find it difficult to assign meaningful numerical weights to objectives.
* Small changes in weights may lead to substantially different recommendations.
* Trade-offs become difficult to interpret.
* Important objectives can be unintentionally compensated by improvements in less important ones.

For example, a method with poor predictive performance could still obtain a high overall score if it performs exceptionally well on interpretability or computational efficiency. In practice, many users prefer certain objectives to be treated as priorities rather than as quantities that can be traded arbitrarily.

To preserve these priorities explicitly, CORTEXA adopts a lexicographic decision strategy.

---

## Principle

Lexicographic selection ranks alternatives according to a user-defined priority order.

Let

$$
Π = (o₁, o₂, o₃, o₄)
$$

denote the ordered list of objectives specified by the user.

The first objective represents the most important criterion, while the last objective represents the least important one.

Candidate methods are compared according to the first objective. If two methods obtain identical values, the comparison proceeds to the second objective. The process continues until a difference is observed.

The ranking therefore follows the same logic as a dictionary: the first differing criterion determines the ordering.

---

## Connection with User Preferences

During recommendation, users provide a preference ranking over the four optimization objectives considered by CORTEXA:

* Predictive Performance Preservation
* Reduction Effectiveness
* Interpretability
* Computational Efficiency

For example, a user may specify:

```
Performance
→ Interpretability
→ Computational Efficiency
→ Reduction Effectiveness
```

This ordering indicates that predictive performance is the primary concern. Consequently, a method offering better predictive performance will always be preferred, regardless of improvements obtained on lower-priority objectives.

The recommendation process therefore reflects the priorities explicitly expressed by the user rather than relying on predefined weighting schemes.

---

## Connection with the Knowledge Base

The Knowledge Base stores the behavioural profile of each reduction method under different analytical contexts.

For a given dataset profile and analytical task, each method is associated with a behavioural vector describing its observed performance on the four optimization objectives.

When a recommendation request is submitted:

1. The Decision Engine identifies the analytical context.
2. Matching behavioural profiles are retrieved from the Knowledge Base.
3. The user-defined priority ordering is applied.
4. Methods are ranked lexicographically according to the selected objective order.

The Knowledge Base therefore provides the empirical evidence used for comparison, while lexicographic selection provides the decision mechanism used to transform this evidence into a personalized recommendation list.

---

## Illustrative Example

Assume two reduction methods have the following behavioural profiles:

| Method | Performance | Interpretability | Reduction | Cost |
| ------ | ----------- | ---------------- | --------- | ---- |
| PCA    | 0.92        | 0.30             | 0.80      | 0.40 |
| RFE    | 0.90        | 0.90             | 0.60      | 0.70 |

Suppose the user specifies the following priority order:

```
Performance → Interpretability → Cost → Reduction
```

The comparison starts with performance:

```
PCA = 0.92
RFE = 0.90
```

Because PCA achieves a higher value on the most important objective, it is ranked above RFE.

The remaining objectives are not considered because the decision has already been determined by the highest-priority criterion.

This behaviour guarantees that the recommendation remains fully aligned with the priorities expressed by the user.
