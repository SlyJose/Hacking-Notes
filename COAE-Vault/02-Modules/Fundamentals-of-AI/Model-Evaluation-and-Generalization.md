---
module: fundamentals-of-ai
category: general
tags: [ml, evaluation, overfitting, underfitting, regularization, cross-validation]
tools: [Scikit-learn]
attack-type: 
exam-relevance: core
---

# Model Evaluation and Generalization

## Summary

How to assess whether a model actually works — evaluation metrics, generalization to unseen data, and techniques to prevent overfitting and underfitting.

## Evaluation Metrics

| Metric | What It Measures |
|--------|-----------------|
| **Accuracy** | Proportion of correct predictions out of all predictions |
| **Precision** | Proportion of true positives among all positive predictions (how many predicted positives are correct) |
| **Recall** | Proportion of true positives among all actual positive instances (how many actual positives were found) |
| **F1-Score** | Harmonic mean of precision and recall — balanced measure when classes are imbalanced |

## Generalization

The model's ability to accurately predict outcomes for **new, unseen data** not used during training. A model that generalizes well applies its learned knowledge to real-world scenarios effectively.

## Overfitting

Model learns the training data **too well**, including noise and outliers. Memorizes instead of learning patterns.

- High accuracy on training data, poor performance on new data
- Model is too complex for the data

## Underfitting

Model is **too simple** to capture underlying patterns.

- Poor performance on both training data and new data
- Model lacks capacity to represent the relationships

## Cross-Validation

Technique to assess how well a model generalizes to an independent dataset.

1. Split data into multiple subsets (folds)
2. Train the model on different combinations of folds
3. Validate on the remaining fold each time
4. Average results across all folds

Reduces overfitting risk and provides a more reliable performance estimate than a single train/test split.

## Regularization

Technique to prevent overfitting by adding a **penalty term** to the loss function. Discourages the model from learning overly complex patterns.

| Type | Penalty | Effect |
|------|---------|--------|
| **L1 Regularization** | Absolute value of coefficient magnitudes | Drives some coefficients to zero (feature selection) |
| **L2 Regularization** | Square of coefficient magnitudes | Shrinks coefficients but keeps all features |

## Why This Matters for AI Red Teaming

- **Overfitting** makes models vulnerable to adversarial examples — the model relies on spurious patterns that attackers can exploit
- **Evaluation metrics** help measure attack success rates (e.g., accuracy drop after adversarial perturbation)
- **Understanding generalization** helps predict how models behave on adversarial inputs vs clean data

## Related Notes

- [[Supervised-Learning-Algorithms]] — the algorithms being evaluated
- [[Supervised-Learning-Core-Concepts]] — what gets evaluated (training data, features, models)
- [[Deep-Learning-Fundamentals]] — DL-specific evaluation considerations

## Sources / References

- COAE Course Module: Fundamentals of AI — Supervised Learning Algorithms
