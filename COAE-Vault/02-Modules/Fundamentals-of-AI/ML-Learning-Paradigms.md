---
module: fundamentals-of-ai
category: general
tags: [ml, supervised-learning, unsupervised-learning, reinforcement-learning]
tools: [Scikit-learn, PyTorch, TensorFlow]
attack-type: 
exam-relevance: core
---

# ML Learning Paradigms

## Summary

Machine Learning algorithms use statistical techniques to identify patterns, trends, and anomalies within datasets, allowing systems to make predictions, decisions, or classifications based on new input data — without explicit programming. Three main paradigms exist: supervised, unsupervised, and reinforcement learning.

## Supervised Learning

The algorithm learns from **labeled data** — each data point is associated with a known outcome or label. The model learns the mapping from inputs to outputs.

**Examples:**
- Image classification (cat vs dog)
- Spam detection
- Fraud prevention

## Unsupervised Learning

The algorithm learns from **unlabeled data** — no outcome or label is provided. The model discovers hidden structure in the data.

**Examples:**
- Customer segmentation
- Anomaly detection
- Dimensionality reduction

## Reinforcement Learning

The algorithm learns through **trial and error** by interacting with an environment and receiving feedback as rewards or penalties.

**Examples:**
- Game playing
- Robotics
- Autonomous driving

## How It Works (Intuition)

Example: an ML algorithm trained on a dataset of images labeled "cat" or "dog." By analyzing features and patterns, it learns to distinguish between them. When presented with a new image, it predicts the label based on learned knowledge.

## Applications Across Industries

| Industry | Applications |
|----------|-------------|
| Healthcare | Disease diagnosis, drug discovery, personalized medicine |
| Finance | Fraud detection, risk assessment, algorithmic trading |
| Marketing | Customer segmentation, targeted advertising, recommendation systems |
| Cybersecurity | Threat detection, intrusion prevention, malware analysis |
| Transportation | Traffic prediction, autonomous vehicles, route optimization |

## Related Notes

- [[AI-ML-DL-Relationship]]
- [[Deep-Learning-Fundamentals]]

## Sources / References

- COAE Course Module: Fundamentals of AI — Introduction to Machine Learning
