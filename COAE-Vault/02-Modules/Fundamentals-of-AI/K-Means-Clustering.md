---
module: fundamentals-of-ai
category: general
tags: [ml, unsupervised-learning, clustering, k-means, euclidean-distance, elbow-method, silhouette]
tools: [Scikit-learn, NumPy]
attack-type: 
exam-relevance: core
---

# K-Means Clustering

## Summary

A popular unsupervised algorithm that partitions a dataset into **K distinct, non-overlapping clusters** by minimizing within-cluster variance. Data points are grouped so points within a cluster are close to each other and far from points in other clusters. Commonly used for customer segmentation, anomaly detection preprocessing, and topic discovery.

![[assets/k_means_clustering.png]]

## Algorithm Steps

1. **Initialization** — randomly select K data points as initial centroids
2. **Assignment** — assign each data point to its nearest centroid (by Euclidean distance)
3. **Update** — recalculate each centroid as the mean of all points assigned to it
4. **Iterate** — repeat steps 2–3 until centroids no longer change significantly or max iterations reached

## Euclidean Distance

The standard distance metric used to measure similarity between points:

```python
d(x, y) = sqrt(Σ (xi - yi)**2)
```

Where `xi` and `yi` are the values of the i-th feature for points `x` and `y`.

## Choosing the Optimal K

No universal method — combine techniques with domain expertise.

### Elbow Method

Plot WCSS (Within-Cluster Sum of Squares) against K values. The **elbow point** — where WCSS stops decreasing sharply — is a good K estimate.

```
Steps:
1. Run K-means for K = 1, 2, 3, ..., n
2. Calculate WCSS for each K
3. Plot WCSS vs K
4. Pick K at the "elbow" — beyond it, gains diminish (risk of overfitting)
```

Lower WCSS = more compact clusters. The elbow balances compactness vs. model complexity.

### Silhouette Analysis

Quantitative method — measures how well each point fits its assigned cluster vs. neighboring clusters.

**Silhouette score range: -1 to 1**

| Score | Interpretation |
|-------|---------------|
| Close to **1** | Point well-matched to its cluster, poorly matched to others |
| Close to **0** | Point near the decision boundary between clusters |
| Close to **-1** | Point likely assigned to the wrong cluster |

```
Steps:
1. Run K-means for a range of K values
2. Calculate silhouette score for each data point
3. Average scores for each K
4. Choose K with the highest average silhouette score
```

### Other Considerations

- **Domain expertise** — desired granularity should match the real-world use case (e.g., number of viable marketing segments)
- **Computational cost** — higher K = more resources
- **Interpretability** — clusters should be meaningful in context

## Data Assumptions

| Assumption | Impact if violated |
|-----------|-------------------|
| **Spherical clusters of similar size** | Non-spherical or uneven clusters → poor results |
| **Feature scale matters** | Larger-scale features dominate distance → always standardize/normalize first |
| **Sensitivity to outliers** | Outliers distort centroids → consider removing or using a robust variant |

## Related Notes

- [[Unsupervised-Learning-Algorithms]] — K-means is the canonical clustering algorithm; see similarity measures and cluster validity concepts
- [[Model-Evaluation-and-Generalization]] — silhouette score and WCSS parallel supervised evaluation metrics; same overfitting risk applies
- [[Support-Vector-Machines]] — both are sensitive to feature scale; standardization is required for both

## Sources / References

- COAE Course Module: Fundamentals of AI — K-Means Clustering
