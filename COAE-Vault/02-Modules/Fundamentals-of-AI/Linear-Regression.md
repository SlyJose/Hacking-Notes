---
module: fundamentals-of-ai
category: general
tags: [ml, supervised-learning, regression, ols, linear-regression]
tools: [Scikit-learn, NumPy]
attack-type: 
exam-relevance: core
---

# Linear Regression

## Summary

A fundamental supervised learning algorithm that predicts a **continuous target variable** by establishing a linear relationship between the target and one or more predictor variables. The goal is to find the best-fitting line that minimizes the sum of squared differences between predicted and actual values.

**Analogy:** predicting house price based on size — as size increases, price generally increases. Linear regression quantifies this relationship with a straight line.

## Regression vs Classification

Regression predicts a **continuous value** (a number within a range). Classification predicts a **categorical label**.

| Regression | Classification |
|-----------|---------------|
| House price | Spam / not spam |
| Daily temperature | Cat / dog |
| Website visitor count | Malware / benign |

## Simple Linear Regression

One predictor variable, one target variable:

```python
y = mx + c
```

| Symbol | Meaning |
|--------|---------|
| `y` | Predicted target variable |
| `x` | Predictor variable |
| `m` | Slope (relationship between x and y) |
| `c` | Y-intercept (value of y when x = 0) |

## Multiple Linear Regression

Multiple predictor variables:

```python
y = b0 + b1*x1 + b2*x2 + ... + bn*xn
```

| Symbol | Meaning |
|--------|---------|
| `y` | Predicted target variable |
| `x1...xn` | Predictor variables |
| `b0` | Y-intercept |
| `b1...bn` | Coefficients (relationship of each predictor to target) |

## Ordinary Least Squares (OLS)

The standard method for finding optimal coefficient values. Minimizes the total error between predicted and actual values.

**Process:**

1. **Calculate residuals** — difference between actual y and predicted y for each data point
2. **Square the residuals** — ensures all values positive, gives more weight to larger errors
3. **Sum the squared residuals** — single value called **Residual Sum of Squares (RSS)**
4. **Minimize RSS** — adjust coefficients to find the smallest possible RSS

**Intuition:** find the line that minimizes the total area of the squares formed between data points and the line — the "line of best fit."

## Assumptions

Linear regression requires four assumptions to be valid. Violating them can produce inaccurate or misleading predictions.

| Assumption | Meaning |
|-----------|---------|
| **Linearity** | Linear relationship exists between predictor and target variables |
| **Independence** | Observations in the dataset are independent of each other |
| **Homoscedasticity** | Variance of errors is constant across all levels of predictor variables (residual spread is uniform) |
| **Normality** | Errors are normally distributed (important for valid coefficient inference) |

## Related Notes

- [[Supervised-Learning-Algorithms]] — linear regression is a regression-type supervised algorithm
- [[Model-Evaluation-and-Generalization]] — evaluation metrics, overfitting, regularization (L1/L2 apply directly to linear regression coefficients)
- [[Supervised-Learning-Core-Concepts]] — features = predictor variables, labels = target variable

## Sources / References

- COAE Course Module: Fundamentals of AI — Linear Regression
