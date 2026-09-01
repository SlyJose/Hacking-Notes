---
module: fundamentals-of-ai
category: general
tags: [ml, unsupervised-learning, clustering, dimensionality-reduction, anomaly-detection, similarity]
tools: [Scikit-learn, NumPy]
attack-type: 
exam-relevance: core
---

# Unsupervised Learning Algorithms

## Summary

Algorithms that operate on **unlabeled data** to discover hidden patterns, structures, and relationships without predefined outcomes. No "correct answers" are provided — the algorithm infers structure from the data's inherent characteristics alone.

High-level coverage in [[ML-Learning-Paradigms]]. This note covers the deep mechanics and core concepts.

## Three Problem Categories

| Category | Goal | Example |
|----------|------|---------|
| **Clustering** | Group similar data points together | Customer segmentation, topic modelling |
| **Dimensionality Reduction** | Reduce feature count while preserving information | Compressing features before supervised learning |
| **Anomaly Detection** | Identify data points that deviate from the norm | Fraud detection, network intrusion detection |

## Core Concepts

### Similarity Measures

Most unsupervised algorithms depend on quantifying how alike two data points are:

| Measure | Description | Use case |
|---------|-------------|---------|
| **Euclidean Distance** | Straight-line distance in multi-dimensional space | General-purpose clustering |
| **Cosine Similarity** | Angle between two vectors — higher angle = less similar | Text/NLP (direction matters more than magnitude) |
| **Manhattan Distance** | Sum of absolute coordinate differences | Grid-like or high-dimensional spaces |

### Clustering Tendency

Before applying clustering, assess whether the data actually has natural groupings. Uniformly distributed data has no meaningful clusters — applying a clustering algorithm produces arbitrary groups.

### Cluster Validity

Evaluating cluster quality after the algorithm runs:

| Metric | Measures | Ideal value |
|--------|----------|------------|
| **Cohesion** | Similarity of points within a cluster | High (compact clusters) |
| **Separation** | Difference between clusters | High (distinct clusters) |
| **Silhouette score** | Combined cohesion + separation per point | Closer to 1 |
| **Davies-Bouldin index** | Average similarity of each cluster to its most similar cluster | Lower is better |

### Dimensionality and the Curse of Dimensionality

- **Dimensionality** — number of features in the dataset
- **Curse of dimensionality** — as feature count grows, data becomes sparse and distances between points lose meaning, degrading algorithm performance
- **Intrinsic dimensionality** — the true underlying dimensionality of the data, often much lower than the raw feature count; dimensionality reduction targets this

### Anomaly vs Outlier

| Term | Definition |
|------|-----------|
| **Anomaly** | Data point deviating significantly from expected patterns — often signals fraud, errors, or attacks |
| **Outlier** | Data point far from the majority — broader term; may be error, unusual observation, or interesting pattern |

Anomaly is the more operationally specific term in security contexts.

### Feature Scaling

Essential before distance-based unsupervised algorithms — unscaled features with larger ranges dominate distance calculations:

| Technique | What it does |
|-----------|-------------|
| **Min-Max Scaling** | Scales each feature to a fixed range (e.g., [0, 1]) |
| **Standardization (Z-score)** | Transforms features to zero mean, unit variance |

## Related Notes

- [[ML-Learning-Paradigms]] — unsupervised learning in context of all three paradigms
- [[Model-Evaluation-and-Generalization]] — cluster validity metrics parallel supervised evaluation metrics
- [[Supervised-Learning-Algorithms]] — contrast: labeled data, predefined outcomes

## Sources / References

- COAE Course Module: Fundamentals of AI — Unsupervised Learning Algorithms
