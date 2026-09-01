---
module: fundamentals-of-ai
category: general
tags: [ml, unsupervised-learning, anomaly-detection, outlier-detection, isolation-forest, one-class-svm, lof]
tools: [Scikit-learn]
attack-type: 
exam-relevance: core
---

# Anomaly Detection

## Summary

Identifies data points that deviate significantly from normal behavior. Also called **outlier detection**. Anomalies can signal fraud, system failures, cyberattacks, or medical emergencies. Algorithms learn the pattern of normal data and flag deviations.

## Three Types of Anomalies

| Type | Description | Example |
|------|-------------|---------|
| **Point** | Single data point deviates significantly | Spike in network traffic, unusual transaction amount |
| **Contextual** | Anomalous within a specific context, not in isolation | 30°C temperature in winter (normal in summer) |
| **Collective** | Group of points collectively deviate, though individual points may look normal | Surge of logins from many unknown IPs (coordinated attack) |

## Detection Technique Categories

| Category | Approach |
|----------|---------|
| **Statistical** | Assumes normal data follows a distribution (e.g., Gaussian); flags deviations. Z-score, modified z-score, boxplots. |
| **Clustering-based** | Outliers don't belong to any cluster or fall in sparse clusters. K-means, density-based clustering. |
| **ML-based** | Learns patterns from normal data; flags non-conforming points. One-Class SVM, Isolation Forest, LOF. |

## One-Class SVM

![[assets/one_class_svm.png]]

Learns a **boundary enclosing normal data**. Points outside the boundary are anomalies. Uses kernel functions for non-linear boundaries (same kernel trick as [[Support-Vector-Machines]]).

- Trained on normal data only — no anomaly labels needed
- Works well for high-dimensional data
- Sensitive to hyperparameter tuning (kernel choice, nu parameter)

## Isolation Forest

![[assets/isolation_forest.png]]

Isolates anomalies by **randomly partitioning data** into isolation trees. Anomalies are "few and different" — they require fewer splits to isolate → shorter path lengths in the trees.

**Anomaly score formula:**

```python
score(x) = 2 ** (-E(h(x)) / c(n))
```

| Symbol | Meaning |
|--------|---------|
| `E(h(x))` | Average path length of point x across all isolation trees |
| `c(n)` | Normalization factor — average path length of a BST with n nodes |
| `n` | Number of data points |

| Score | Interpretation |
|-------|---------------|
| Close to **1** | Likely anomaly (isolated quickly) |
| Close to **0.5** | Likely normal (hard to isolate) |

**How it works:** recursively pick a random feature → random split value → repeat until point is isolated in a leaf node. Repeat across many trees; average path length = anomaly score.

## Local Outlier Factor (LOF)

![[assets/local_outlier_factor.png]]

**Density-based** method. Compares the local density of a point to its k nearest neighbors. Points with significantly lower local density than their neighbors are outliers.

**LOF score formula:**

```python
LOF(p) = (Σ lrd(o) / k) / lrd(p)
```

Where `lrd` is the **local reachability density**:

```python
lrd(p) = 1 / (Σ reach_dist(p, o) / k)
```

- `reach_dist(p, o)` = max(actual distance(p, o), k-distance(o))
- k-distance(o) = distance from o to its kth nearest neighbor

| LOF Score | Interpretation |
|-----------|---------------|
| **≈ 1** | Point has similar density to neighbors → likely normal |
| **>> 1** | Point has much lower density than neighbors → likely outlier |

LOF is effective when **cluster density varies** — point anomalies in sparse regions are flagged even when surrounded by other sparse points.

## Algorithm Comparison

| | One-Class SVM | Isolation Forest | LOF |
|-|--------------|-----------------|-----|
| Approach | Boundary-based | Tree partitioning | Density-based |
| Handles non-linearity | Yes (kernels) | Yes | Yes (local neighborhoods) |
| Scalability | Moderate | High | Low (expensive on large data) |
| Interpretability | Low | Medium | Medium |
| Best for | High-dim data | Large datasets | Variable-density clusters |

## Data Assumptions

| Assumption | Detail |
|-----------|--------|
| **Normal data distribution** | Statistical methods assume Gaussian; ML methods are more flexible |
| **Feature relevance** | Poor feature selection degrades all anomaly detection methods |
| **Labeled data** | Some supervised variants require labels; the three above do not |

## Related Notes

- [[Unsupervised-Learning-Algorithms]] — anomaly detection is one of the three unsupervised problem categories; see anomaly vs outlier distinction
- [[Support-Vector-Machines]] — One-Class SVM uses the same kernel trick; key difference is one-class (no negative examples)
- [[K-Means-Clustering]] — clustering-based anomaly detection; points in sparse/small clusters flagged as outliers

## Sources / References

- COAE Course Module: Fundamentals of AI — Anomaly Detection
