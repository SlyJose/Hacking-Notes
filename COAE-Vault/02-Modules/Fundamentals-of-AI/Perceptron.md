---
module: fundamentals-of-ai
category: general
tags: [deep-learning, neural-networks, perceptron, activation-functions, linear-separability]
tools: [Python, NumPy]
attack-type: 
exam-relevance: core
---

# Perceptron

## Summary

The **perceptron** is the fundamental building block of neural networks — a simplified model of a biological neuron capable of making binary decisions. Single-layer perceptrons can only learn **linear decision boundaries**, which limits them to linearly separable problems.

Understanding perceptrons is the entry point to understanding how [[Deep-Learning-Fundamentals]] networks are constructed and trained.

## Structure

![[assets/perceptron.png]]

| Component | Symbol | Role |
|-----------|--------|------|
| **Inputs** | x₁, x₂, ..., xₙ | Features of the input data |
| **Weights** | w₁, w₂, ..., wₙ | Importance of each input; can be positive or negative |
| **Summation** | Σ(wᵢ · xᵢ) | Aggregates weighted inputs into a single value |
| **Bias** | b | Shifts the activation threshold; allows activation when all inputs are zero |
| **Activation function** | f | Introduces non-linearity; produces final output |
| **Output** | y | Binary decision (0 or 1) |

**Formula:**

```python
y = f(Σ(wᵢ · xᵢ) + b)
```

## Worked Example — Play Tennis Decision

Four weather features determine whether to play tennis.

**Encoding:**

| Feature | Values |
|---------|--------|
| Outlook | Sunny=0, Overcast=1, Rainy=2 |
| Temperature | Hot=0, Mild=1, Cool=2 |
| Humidity | High=0, Normal=1 |
| Wind | Weak=0, Strong=1 |

**Weights and bias:**

```python
w1, w2, w3, w4 = 0.3, 0.2, -0.4, -0.2
b = 0.1
```

**Activation function (step):**

```python
def step_activation(x):
    return 1 if x > 0 else 0
```

**Inference — Sunny, Mild, High humidity, Weak wind:**

```python
outlook, temperature, humidity, wind = 0, 1, 0, 0

weighted_sum = (0.3*0) + (0.2*1) + (-0.4*0) + (-0.2*0)  # = 0.2
total_input  = 0.2 + 0.1                                  # = 0.3
output       = step_activation(0.3)                        # = 1 → Play Tennis
```

## Limitations

Single-layer perceptrons can only model **linear decision boundaries** — a single straight line (or hyperplane) separating two classes.

**XOR problem** — the canonical failure case:

| x₁ | x₂ | XOR |
|----|-----|-----|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

No single straight line can separate the 1s from the 0s — XOR is not linearly separable. A single perceptron cannot solve it. **Multi-layer networks** (with hidden layers and non-linear activations) overcome this limitation — the basis of [[Deep-Learning-Fundamentals]].

## Related Notes

- [[Deep-Learning-Fundamentals]] — multi-layer networks that stack perceptrons; backpropagation, activation functions, optimizers
- [[AI-ML-DL-Relationship]] — context for where neural networks fit in the AI hierarchy

## Sources / References

- COAE Course Module: Fundamentals of AI — Perceptrons
