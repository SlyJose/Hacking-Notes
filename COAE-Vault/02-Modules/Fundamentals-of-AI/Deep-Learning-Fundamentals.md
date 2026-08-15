---
module: fundamentals-of-ai
category: general
tags: [deep-learning, neural-networks, cnn, rnn, transformers]
tools: [PyTorch, TensorFlow, Keras]
attack-type: 
exam-relevance: core
---

# Deep Learning Fundamentals

## Summary

Deep Learning (DL) is a subfield of ML that uses neural networks with **multiple layers** to learn and extract features from complex data. Particularly powerful for unstructured or high-dimensional data: images, audio, and text.

## Key Characteristics

- **Hierarchical Feature Learning:** each layer captures increasingly abstract features. In image recognition, lower layers detect edges and textures; higher layers identify shapes and objects.
- **End-to-End Learning:** models directly map raw input data to desired outputs without manual feature engineering.
- **Scalability:** models scale well with large datasets and computational resources — suitable for big data applications.

## Common Neural Network Architectures

### Convolutional Neural Networks (CNNs)

- Specialized for **image and video** data
- Use convolutional layers to detect local patterns and spatial hierarchies
- Applications: image classification, object detection, image segmentation

### Recurrent Neural Networks (RNNs)

- Designed for **sequential data** (text, speech)
- Have loops that allow information to persist across time steps
- Applications: sentiment analysis, time-series prediction

### Transformers

- Recent advancement, particularly effective for **NLP** tasks
- Leverage **self-attention mechanisms** to handle long-range dependencies
- Applications: machine translation, text generation, chatbots (GPT, BERT)

## State-of-the-Art Applications

| Domain | Tasks |
|--------|-------|
| Computer Vision | Image classification, object detection, image segmentation |
| NLP | Sentiment analysis, machine translation, text generation |
| Speech Recognition | Audio-to-text transcription, speech synthesis |
| Reinforcement Learning | Game playing, robot control |

## Why This Matters for AI Red Teaming

Understanding DL architectures is essential for:
- Crafting adversarial examples against CNNs (evasion attacks)
- Understanding transformer vulnerabilities (prompt injection)
- Knowing what gradient access means for white-box attacks

## Related Notes

- [[AI-ML-DL-Relationship]]
- [[ML-Learning-Paradigms]]

## Sources / References

- COAE Course Module: Fundamentals of AI — Introduction to Machine Learning
