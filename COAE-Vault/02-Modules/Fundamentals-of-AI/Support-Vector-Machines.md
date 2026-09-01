---
module: fundamentals-of-ai
category: general
tags: [ml, supervised-learning, classification, regression, svm, kernel-trick, hyperplane]
tools: [Scikit-learn]
attack-type: 
exam-relevance: core
---

# Support Vector Machines (SVMs)

## Summary

A supervised learning algorithm for **classification and regression**. SVMs find the **optimal hyperplane** that maximally separates classes by maximizing the margin between the decision boundary and the nearest data points (**support vectors**). Particularly effective on high-dimensional data.

## Core Concepts

### Margin and Support Vectors

- **Margin** — distance between the hyperplane and the nearest data points of each class
- **Support vectors** — the data points closest to the hyperplane; they define it
- Maximizing the margin → more robust boundary → better generalization

### The Hyperplane Equation

```python
w * x + b = 0
```

| Symbol | Meaning |
|--------|---------|
| `w` | Weight vector — perpendicular to the hyperplane |
| `x` | Input feature vector |
| `b` | Bias term — shifts the hyperplane relative to the origin |

## Linear SVM

Used when data is **linearly separable** (a straight hyperplane perfectly separates classes).

![[assets/svm_optimal_hyperplane.png]]

**Optimization objective:**

```python
Minimize:   1/2 * ||w||^2
Subject to: yi * (w * xi + b) >= 1  for all i
```

- Minimizing `||w||²` maximizes the margin
- Constraint ensures all points are correctly classified with margin ≥ 1
- `yi` is the class label: `-1` or `+1`

## Non-Linear SVM and the Kernel Trick

When data is **not linearly separable**, SVMs use the **kernel trick** — a function that maps data to a higher-dimensional space where it *becomes* linearly separable. The resulting hyperplane maps back to a non-linear decision boundary in the original space.

![[assets/svm_non_linear.png]]

### Kernel Functions

| Kernel | How it works | Best for |
|--------|-------------|---------|
| **Polynomial** | Adds polynomial terms (x², x³…) | Moderate non-linearity |
| **RBF (Gaussian)** | Gaussian function mapping to higher-dim space | Complex non-linear patterns; most versatile |
| **Sigmoid** | Sigmoid-shaped boundary (similar to logistic regression) | Neural-network-like boundaries |

RBF is the default choice — start here unless there's a specific reason to use another.

## Data Assumptions

| Property | Detail |
|----------|--------|
| **No distributional assumption** | Does not require features to be normally distributed |
| **High-dimensional data** | Effective when features outnumber data points |
| **Robust to outliers** | Optimizes margin, not individual point fit |

## SVM vs Other Classifiers

| | SVM | Decision Trees | Naive Bayes | Logistic Regression |
|-|-----|---------------|-------------|---------------------|
| Non-linear | Yes (kernel) | Yes (splits) | No | No |
| High-dim | Excellent | Degrades | Good | Moderate |
| Probabilistic output | No | No | Yes | Yes |
| Interpretable | Low | High | Medium | Medium |

## Related Notes

- [[Decision-Trees]] — alternative for non-linear classification; tree-based rather than margin-based
- [[Logistic-Regression]] — also uses a hyperplane concept (decision boundary) but is probabilistic and assumes linearity
- [[Naive-Bayes]] — probabilistic classifier; contrasts with SVM's geometric approach
- [[Model-Evaluation-and-Generalization]] — margin maximization is a form of implicit regularization; see also kernel hyperparameter tuning

## Sources / References

- COAE Course Module: Fundamentals of AI — Support Vector Machines
