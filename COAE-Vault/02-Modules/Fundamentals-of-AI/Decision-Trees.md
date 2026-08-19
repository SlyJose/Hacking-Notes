---
module: fundamentals-of-ai
category: general
tags: [ml, supervised-learning, classification, regression, decision-trees, gini, entropy, information-gain]
tools: [Scikit-learn]
attack-type: 
exam-relevance: core
---

# Decision Trees

## Summary

A supervised learning algorithm for **classification and regression**. Models predictions as a series of binary decision rules learned from feature values, producing an interpretable tree structure. Unlike [[Logistic-Regression]] and [[Linear-Regression]], decision trees make **no linearity assumption** and handle non-linear relationships natively.

## Tree Structure

| Component | Role |
|-----------|------|
| **Root node** | Starting point — contains the entire dataset; split on best feature |
| **Internal nodes** | Each represents a feature; branches represent possible values/ranges |
| **Leaf nodes** | Terminal nodes — output the final class prediction or regression value |

## Splitting Criteria

At each node, the algorithm evaluates all features and selects the one that produces the most homogeneous (pure) subsets.

### Gini Impurity

Probability of misclassifying a randomly chosen element. Lower = purer.

```python
Gini(S) = 1 - Σ (pi ** 2)
```

Example (30 class A, 20 class B):

```python
pA = 0.6, pB = 0.4
Gini = 1 - (0.6**2 + 0.4**2) = 1 - 0.52 = 0.48
```

### Entropy

Measures disorder/uncertainty in a set. Lower = more homogeneous.

```python
Entropy(S) = - Σ (pi * log2(pi))
```

Example (same dataset):

```python
Entropy = -(0.6 * log2(0.6) + 0.4 * log2(0.4)) ≈ 0.971
```

### Information Gain

Reduction in entropy from splitting on a feature. The feature with the **highest information gain** is chosen.

```python
InfoGain(S, A) = Entropy(S) - Σ ((|Sv| / |S|) * Entropy(Sv))
```

Where `Sv` is the subset where feature `A` has value `v`.

**Worked example** (50 instances, feature F with values 1 and 2):

```python
# F=1: 30 instances (20A, 10B) → Entropy(S1) = 0.9183
# F=2: 20 instances (10A, 10B) → Entropy(S2) = 1.0

Weighted_Entropy = (30/50)*0.9183 + (20/50)*1.0 = 0.951
InfoGain = 0.971 - 0.951 = 0.020
```

## Building the Tree

1. Start at root with the full dataset
2. Calculate Gini/entropy/information gain for every feature
3. Select the best feature → create internal node + branches
4. Recursively split each subset
5. Stop when a stopping criterion is met

### Stopping Criteria

| Criterion | Description |
|-----------|-------------|
| **Maximum depth** | Prevents overly complex trees (overfitting) |
| **Minimum samples per node** | Avoids splits on very small subsets |
| **Pure nodes** | All instances in the node belong to the same class |

## Data Assumptions

Decision trees have **minimal assumptions** — a key advantage over linear models:

| Property | Detail |
|----------|--------|
| **No linearity required** | Handles non-linear feature–target relationships |
| **No normality required** | Data distribution doesn't need to be Gaussian |
| **Robust to outliers** | Splits on feature values, not distance-based calculations |

## Related Notes

- [[Logistic-Regression]] — classification algorithm with strict linearity-of-log-odds assumption; decision trees avoid this
- [[Linear-Regression]] — regression algorithm with strict linearity assumption; decision trees avoid this
- [[Supervised-Learning-Algorithms]] — decision trees cover both classification and regression problem types
- [[Model-Evaluation-and-Generalization]] — max depth and min samples are regularization-like hyperparameters; overfitting is a key risk

## Sources / References

- COAE Course Module: Fundamentals of AI — Decision Trees
