---
module: fundamentals-of-ai
category: general
tags: [ml, unsupervised-learning, dimensionality-reduction, pca, eigenvectors, eigenvalues, covariance]
tools: [Scikit-learn, NumPy]
attack-type: 
exam-relevance: core
---

# Principal Component Analysis (PCA)

## Summary

A **dimensionality reduction** technique that transforms high-dimensional data into a lower-dimensional representation while preserving maximum variance. It identifies **principal components** — new variables that are linear combinations of the original features, ordered by how much variance they capture.

Use cases: feature extraction, data visualization, noise reduction, preprocessing before ML models.

![[assets/pca.png]]

## Three Key Concepts

| Concept | Definition |
|---------|-----------|
| **Variance** | Spread of data points around the mean. PCA maximizes this along each principal component. |
| **Covariance** | Relationship between two features. PCA uses the covariance matrix to find directions of maximum variance. |
| **Eigenvectors / Eigenvalues** | Eigenvectors = directions of principal components. Eigenvalues = amount of variance each component explains. |

## Algorithm Steps

1. **Standardize** — subtract mean, divide by std dev for each feature (ensures equal scale)
2. **Covariance matrix** — compute pairwise covariances between all features
3. **Eigenvectors + eigenvalues** — solve the eigenvalue equation on the covariance matrix
4. **Sort** — order eigenvectors by descending eigenvalue (most variance first)
5. **Select k components** — choose the top k eigenvectors for the target dimensionality
6. **Transform** — project original data onto the selected components

```python
Y = X * V
```

| Symbol | Meaning |
|--------|---------|
| `Y` | Transformed data in lower-dimensional space |
| `X` | Original data matrix |
| `V` | Matrix of selected eigenvectors (principal components) |

## Eigenvectors and Eigenvalues

### The Eigenvalue Equation

```python
A * v = λ * v
```

- `v` = eigenvector — the direction that remains unchanged under transformation
- `λ` (lambda) = eigenvalue — the scalar by which `v` is stretched/shrunk
- In PCA, `A` is the **covariance matrix C**:

```python
C * v = λ * v
```

Eigenvectors with **larger eigenvalues** capture more variance → selected first.

### Worked Example (Rubber Band)

```python
A = [[2, 0],
     [0, 1]]
v = [1, 0]

A * v = [2, 0]   # same direction, stretched by factor 2
# → eigenvector: [1, 0], eigenvalue: λ = 2
```

### Solving the Equation

| Method | Notes |
|--------|-------|
| **Eigenvalue Decomposition** | Direct computation |
| **SVD (Singular Value Decomposition)** | More numerically stable; preferred in practice |

## Choosing the Number of Components

Plot **explained variance ratio** vs number of components. Choose k where cumulative explained variance reaches a target threshold (commonly **95%**).

- Higher k → more information preserved, less compression
- Lower k → more compression, some information lost
- The plot resembles the elbow method from [[K-Means-Clustering]]

![[assets/pca_facial_features.png]]

*Example: eigenfaces — PCA principal components from a face image database, each capturing a different facial variation pattern.*

## Data Assumptions

| Assumption | Detail |
|-----------|--------|
| **Linearity** | Assumes linear relationships between features — non-linear structure will not be captured |
| **Correlation** | Works best when features are correlated (otherwise little variance to redistribute) |
| **Scale sensitivity** | Larger-scale features dominate — always standardize before applying |

## Related Notes

- [[Unsupervised-Learning-Algorithms]] — PCA is the canonical dimensionality reduction technique; see curse of dimensionality and intrinsic dimensionality
- [[K-Means-Clustering]] — PCA is commonly applied before K-means to reduce dimensionality; both use explained variance concepts
- [[Support-Vector-Machines]] — PCA as preprocessing can improve SVM performance on high-dimensional data
- [[Model-Evaluation-and-Generalization]] — explained variance ratio is the PCA analogue of evaluation metrics; overfitting via too few components is a risk

## Sources / References

- COAE Course Module: Fundamentals of AI — Principal Component Analysis
