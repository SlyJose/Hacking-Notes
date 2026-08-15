---
module: fundamentals-of-ai
category: general
tags: [ml, supervised-learning, classification, regression]
tools: [Scikit-learn, PyTorch, TensorFlow]
attack-type: 
exam-relevance: core
---

# Supervised Learning Algorithms

## Summary

Supervised learning algorithms learn from **labeled data** — each data point has a known outcome. The algorithm learns a mapping function from input features to output labels, then uses that mapping to predict labels for new, unseen data. This is the cornerstone of many ML applications.

**Analogy:** like teaching a child to identify fruits by repeatedly showing examples with labels ("this is an apple", "this is an orange") until they can identify new fruits on their own.

## Two Problem Types

### Classification

Predict a **categorical label** (discrete outcome).

**Examples:**
- Classifying emails as spam or not spam
- Identifying images as cats, dogs, or birds
- Malware vs benign classification

### Regression

Predict a **continuous value** (numeric outcome).

**Examples:**
- Predicting house price based on size, location, features
- Forecasting stock market values
- Estimating network traffic volume

## How It Works

1. Feed labeled dataset (features + labels) to the algorithm
2. Algorithm iteratively adjusts internal parameters to minimize prediction errors
3. Model learns patterns/relationships between features and labels
4. Trained model predicts labels for new, unseen data

## Related Notes

- [[ML-Learning-Paradigms]] — supervised learning in context of all three paradigms
- [[Supervised-Learning-Core-Concepts]] — training data, features, labels, model, inference
- [[Model-Evaluation-and-Generalization]] — accuracy, overfitting, cross-validation, regularization

## Sources / References

- COAE Course Module: Fundamentals of AI — Supervised Learning Algorithms
