---
module: fundamentals-of-ai
category: general
tags: [ml, supervised-learning, training, inference, prediction]
tools: [Scikit-learn, PyTorch]
attack-type: 
exam-relevance: core
---

# Supervised Learning — Core Concepts

## Summary

The building blocks of supervised learning: what goes in (training data, features, labels), how the model learns (training), and what comes out (prediction, inference).

## Training Data

The labeled dataset used to train the model — input features paired with output labels. Quality and quantity directly impact accuracy and generalization ability.

Think of it as example problems with correct solutions — the algorithm learns from these to solve similar problems in the future.

## Features

Measurable properties or characteristics of the data that serve as **input** to the model. Selecting relevant features is crucial for model effectiveness.

**Example (house price prediction):**
- Size
- Number of bedrooms
- Location
- Age of the house

## Labels

The known outcomes or target variables — the **"correct answers"** the model aims to predict.

**Example:** in house price prediction, the label is the actual price.

## Model

A mathematical representation of the relationship between features and labels. Learned from training data. Think of it as a function: `features → predicted label`.

## Training

The process of feeding training data to the algorithm and iteratively adjusting the model's parameters to minimize prediction errors.

## Prediction vs Inference

| Concept | Focus | Example |
|---------|-------|---------|
| **Prediction** | Generating actionable outputs | Classifying an email as spam, forecasting stock prices |
| **Inference** | Understanding structure and patterns in data | Determining which features matter most, estimating coefficients, analyzing input impact on predictions |

Prediction is a specific application of inference. Inference is broader — it includes understanding relationships, estimating parameters, and explaining/interpreting results.

## Related Notes

- [[Supervised-Learning-Algorithms]] — overview and problem types
- [[Model-Evaluation-and-Generalization]] — how to assess model quality
- [[ML-Learning-Paradigms]] — supervised in context of all paradigms

## Sources / References

- COAE Course Module: Fundamentals of AI — Supervised Learning Algorithms
