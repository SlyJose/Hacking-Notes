---
module: fundamentals-of-ai
category: general
tags: [deep-learning, neural-networks, cnn, rnn, transformers, backpropagation, activation-functions, loss-function, optimizer]
tools: [PyTorch, TensorFlow, Keras]
attack-type: 
exam-relevance: core
---

# Deep Learning Fundamentals

## Summary

Deep Learning (DL) is a subfield of ML that uses **artificial neural networks with multiple layers** to automatically learn hierarchical representations from raw data — no manual feature engineering required. Particularly powerful for unstructured, high-dimensional data: images, audio, and text.

DL sits within the broader hierarchy: AI ⊃ ML ⊃ DL. See [[AI-ML-DL-Relationship]] and [[ML-Learning-Paradigms]].

## Motivation

| Goal | Detail |
|------|--------|
| **Solve complex problems** | Outperforms traditional ML on image recognition, speech, NLP — learns intricate patterns from vast data |
| **Mimic the human brain** | Inspired by biological neural networks; processes information hierarchically, as humans do |

## Key Characteristics

- **Hierarchical Feature Learning:** each layer captures increasingly abstract features (edges → shapes → objects in CNNs)
- **End-to-End Learning:** maps raw input directly to output without hand-crafted features
- **Scalability:** performance improves with more data and compute

## Core Building Blocks

### Artificial Neural Networks (ANNs)

Computing systems modelled on biological neural networks. Composed of **nodes (neurons)** organised in layers, connected by weighted edges. The network learns by adjusting edge weights based on training data.

### Layers

| Layer Type | Role |
|------------|------|
| **Input layer** | Receives raw data |
| **Hidden layers** | Perform computations and extract features; the more layers, the "deeper" the network |
| **Output layer** | Produces final prediction or classification |

### Activation Functions

Introduce **non-linearity** — without them, stacking layers is equivalent to a single linear transformation and cannot model complex patterns.

| Function | Formula | Behaviour |
|----------|---------|-----------|
| **Sigmoid** | `1 / (1 + e^(-x))` | Squashes output to (0, 1); used in output layer for binary classification |
| **ReLU** | `max(0, x)` | Returns 0 for negative inputs, x for positive; default for hidden layers |
| **Tanh** | `(e^x - e^(-x)) / (e^x + e^(-x))` | Squashes to (-1, 1); zero-centred, better gradient flow than sigmoid |

### Backpropagation

The training algorithm. Calculates the **gradient of the loss function** with respect to each weight using the chain rule, then nudges weights in the direction that reduces loss. Iterates until the model converges.

### Loss Function

Measures the error between predictions and ground truth. Training objective is to minimise this.

| Task | Common Loss |
|------|-------------|
| Regression | Mean Squared Error (MSE) |
| Binary classification | Binary Cross-Entropy |
| Multi-class classification | Categorical Cross-Entropy |

### Optimizers

Use gradients from backpropagation to update weights.

| Optimizer | Characteristic |
|-----------|---------------|
| **SGD** | Simple, reliable; can be slow to converge |
| **Adam** | Adaptive learning rates per parameter; most commonly used in practice |
| **RMSprop** | Adapts learning rate based on recent gradient magnitudes; good for RNNs |

### Hyperparameters

Set before training; control the learning process. Key examples:

| Hyperparameter | Impact |
|----------------|--------|
| **Learning rate** | Step size for weight updates — too high = oscillation, too low = slow convergence |
| **Number of hidden layers** | Depth of the network; more layers = more expressive but harder to train |
| **Neurons per layer** | Width; affects model capacity |
| **Batch size** | Number of samples per gradient update |
| **Epochs** | Number of full passes through the training data |

## Common Architectures

### Convolutional Neural Networks (CNNs)

- Specialized for **image and video** data
- Convolutional layers detect local spatial patterns; pooling layers downsample
- Applications: image classification, object detection, segmentation

### Recurrent Neural Networks (RNNs)

- Designed for **sequential data** (text, speech, time-series)
- Loops allow information to persist across time steps
- Applications: sentiment analysis, time-series prediction

### Transformers

- State-of-the-art for **NLP** and increasingly vision
- **Self-attention mechanisms** model long-range dependencies without recurrence
- Applications: text generation, translation, chatbots (GPT, BERT, LLaMA)

## Applications

| Domain | Tasks |
|--------|-------|
| Computer Vision | Image classification, object detection, segmentation |
| NLP | Sentiment analysis, translation, text generation |
| Speech | Audio-to-text transcription, speech synthesis |
| Reinforcement Learning | Game playing, robot control — see [[Reinforcement-Learning-Algorithms]] |

## Why This Matters for AI Red Teaming

| Attack Surface | Relevance |
|---------------|-----------|
| CNN gradient access | Enables white-box adversarial example crafting (evasion attacks) |
| Transformer architecture | Underpins LLM prompt injection and output manipulation attacks |
| Loss landscape knowledge | Required for understanding model inversion and membership inference |
| Activation function behaviour | Affects model robustness; ReLU networks can be more susceptible to certain perturbations |

## Related Notes

- [[AI-ML-DL-Relationship]] — where DL fits in the AI/ML hierarchy
- [[ML-Learning-Paradigms]] — supervised, unsupervised, RL
- [[Reinforcement-Learning-Algorithms]] — RL uses neural networks for policy/value approximation
- [[Q-Learning]] — tabular RL; Deep Q-Networks extend this with a neural network replacing the Q-table
- [[SARSA]] — on-policy RL; deep variants exist

## Sources / References

- COAE Course Module: Fundamentals of AI — Introduction to Deep Learning
