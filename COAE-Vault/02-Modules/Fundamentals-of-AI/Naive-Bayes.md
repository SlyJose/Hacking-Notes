---
module: fundamentals-of-ai
category: general
tags: [ml, supervised-learning, classification, naive-bayes, probabilistic, bayes-theorem]
tools: [Scikit-learn]
attack-type: 
exam-relevance: core
---

# Naive Bayes

## Summary

A probabilistic classification algorithm based on **Bayes' theorem**. Predicts the class of a data point by computing the posterior probability for each class and selecting the highest. Fast, simple, and effective for text classification (spam filtering, sentiment analysis).

Core assumption: **conditional independence** — each feature is assumed independent of all others given the class label. This assumption is almost always violated in practice, yet Naive Bayes still performs surprisingly well.

## Bayes' Theorem

```python
P(A|B) = (P(B|A) * P(A)) / P(B)
```

| Term | Meaning |
|------|---------|
| `P(A\|B)` | **Posterior** — probability of A given B has occurred |
| `P(B\|A)` | **Likelihood** — probability of seeing B if A is true |
| `P(A)` | **Prior** — baseline probability of A before seeing evidence |
| `P(B)` | **Evidence** — overall probability of B (normalizing constant) |

### Worked Example — Disease Test

- Disease prevalence: `P(A) = 0.01`
- Test accuracy (sensitivity): `P(B|A) = 0.95`
- False positive rate: `P(B|¬A) = 0.05`

```python
# Law of total probability
P(B) = P(B|A)*P(A) + P(B|¬A)*P(¬A)
     = (0.95 * 0.01) + (0.05 * 0.99)
     = 0.0095 + 0.0495 = 0.059

# Bayes' theorem
P(A|B) = (0.95 * 0.01) / 0.059 = 0.0095 / 0.059 ≈ 0.161
```

**Takeaway:** even with a 95%-accurate test, a positive result only means ~16% chance of disease because the prior is very low (1% prevalence). Prior probability strongly influences posterior.

## How Naive Bayes Classifies

1. **Prior probabilities** — compute `P(class)` for each class from the training data
2. **Likelihoods** — compute `P(feature | class)` for each feature–class pair
3. **Apply Bayes** — for a new data point, compute posterior for each class:
   ```python
   P(class | features) ∝ P(class) * Π P(feature_i | class)
   ```
4. **Predict** — assign the class with the highest posterior probability

## Types of Naive Bayes

| Type | Feature type | Use case |
|------|-------------|---------|
| **Gaussian** | Continuous, normally distributed | Age/income-based prediction |
| **Multinomial** | Discrete counts | Text classification (word frequencies) |
| **Bernoulli** | Binary (present/absent) | Document classification (word presence) |

Choose based on the nature of the features, not the target variable.

## Data Assumptions

| Assumption | Detail |
|-----------|--------|
| **Feature independence** | Features are conditionally independent given the class (the "naive" assumption) |
| **Data distribution** | Choice of classifier variant (Gaussian/Multinomial/Bernoulli) must match feature distribution |
| **Sufficient training data** | Needed for reliable probability estimation; less critical than many other algorithms |

## Naive Bayes vs Decision Trees

| | Naive Bayes | Decision Trees |
|-|-------------|---------------|
| Approach | Probabilistic | Rule-based splits |
| Assumption | Feature independence | No linearity required |
| Strength | Fast, text data | Non-linear relationships |
| Output | Class probability | Class label (or value) |

## Related Notes

- [[Decision-Trees]] — alternative classification algorithm; rule-based rather than probabilistic
- [[Logistic-Regression]] — also outputs class probabilities but assumes linearity of log-odds; no independence assumption
- [[Supervised-Learning-Algorithms]] — Naive Bayes is a canonical classification algorithm
- [[Model-Evaluation-and-Generalization]] — precision/recall/F1 are the key metrics for spam/classification tasks

## Sources / References

- COAE Course Module: Fundamentals of AI — Naive Bayes
