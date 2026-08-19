---
module: fundamentals-of-ai
category: general
tags: [ml, supervised-learning, classification, logistic-regression, sigmoid]
tools: [Scikit-learn, NumPy]
attack-type: 
exam-relevance: core
---

# Logistic Regression

## Summary

A supervised learning algorithm used for **binary classification** — predicts a categorical target with two possible outcomes (0/1, yes/no, spam/not spam). Despite the name, it is a **classifier**, not a regressor.

Output is a **probability score** between 0 and 1 via the sigmoid function. If the score exceeds a threshold (default 0.5), the instance is classified as the positive class.

## Sigmoid Function

Transforms the linear combination of features into a probability:

```python
P(x) = 1 / (1 + e**-z)
```

Where `z` is the same linear combination as in linear regression:

```python
z = m1*x1 + m2*x2 + ... + mn*xn + c
```

- Output always in `[0, 1]` — interpretable as probability
- S-shaped curve: near 0 for very negative `z`, near 1 for very positive `z`
- Introduces non-linearity, allowing complex feature–outcome relationships

## Decision Boundary

The threshold that separates classes:

- `P(x) >= threshold` → positive class (e.g., spam)
- `P(x) < threshold` → negative class (e.g., not spam)

In 2D: a **line**. In higher dimensions: a **hyperplane** (a flat subspace one dimension below the ambient space). Defined by the model's learned coefficients + chosen threshold.

### Threshold Tuning

Default threshold is **0.5** but can be adjusted:

| Raise threshold | Fewer spam flags, more false negatives |
|----------------|----------------------------------------|
| Lower threshold | More spam flags, more false positives |

Trade-off depends on problem: medical diagnosis favors low threshold (catch all cases); spam filter may tolerate a higher one.

## Logistic vs Linear Regression

| | Linear Regression | Logistic Regression |
|-|------------------|---------------------|
| Output | Continuous value | Probability [0,1] |
| Task | Regression | Classification |
| Output function | Identity | Sigmoid |
| Decision | N/A | Threshold-based |

## Data Assumptions

| Assumption | Detail |
|-----------|--------|
| **Binary outcome** | Target must have exactly two classes |
| **Linearity of log-odds** | Linear relationship between features and log(p / 1-p) |
| **No multicollinearity** | Highly correlated predictors distort coefficient estimates |
| **Large sample size** | Small datasets produce unreliable parameter estimates |

## Related Notes

- [[Linear-Regression]] — shares the same `z = mx + c` linear combination; differs in output function
- [[Supervised-Learning-Algorithms]] — logistic regression is the canonical binary classification algorithm
- [[Supervised-Learning-Core-Concepts]] — features, labels, training process
- [[Model-Evaluation-and-Generalization]] — accuracy, precision/recall, F1 — all relevant for classifier evaluation

## Sources / References

- COAE Course Module: Fundamentals of AI — Logistic Regression
