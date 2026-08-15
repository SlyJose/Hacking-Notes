---
tags: [flashcards, fundamentals-of-ai]
---

# Fundamentals of AI Flashcards

What is the relationship between AI, ML, and DL?
?
AI ⊃ ML ⊃ DL. AI is the broadest field (intelligent systems). ML is a subfield that learns from data. DL is a subfield of ML using multi-layer neural networks.

What are the three main ML learning paradigms?::Supervised learning (labeled data), Unsupervised learning (unlabeled data), Reinforcement learning (trial and error with rewards/penalties)

In supervised learning, what does the algorithm learn from?::Labeled data — each data point is associated with a known outcome or label

Give three examples of supervised learning tasks::Image classification, spam detection, fraud prevention

In unsupervised learning, what does the algorithm learn from?::Unlabeled data — no outcome or label is provided; the model discovers hidden structure

Give three examples of unsupervised learning tasks::Customer segmentation, anomaly detection, dimensionality reduction

How does reinforcement learning work?::The algorithm learns through trial and error by interacting with an environment and receiving feedback as rewards or penalties

What are the three key characteristics of Deep Learning?::Hierarchical feature learning, end-to-end learning, and scalability with large datasets

What type of neural network is specialized for image and video data?::Convolutional Neural Networks (CNNs) — use convolutional layers to detect local patterns and spatial hierarchies

What type of neural network is designed for sequential data like text and speech?::Recurrent Neural Networks (RNNs) — have loops that allow information to persist across time steps

What are Transformers and what are they used for?::A DL architecture that uses self-attention mechanisms to handle long-range dependencies; particularly effective for NLP tasks (GPT, BERT)

What does "end-to-end learning" mean in Deep Learning?::The model directly maps raw input data to desired outputs without manual feature engineering

What does "hierarchical feature learning" mean in DL?::Each layer captures increasingly abstract features — lower layers detect edges/textures, higher layers identify shapes/objects

What is the primary goal of AI?::To augment human capabilities — enhance decision-making and productivity, not just replace human efforts

Name four key areas of AI::Natural Language Processing (NLP), Computer Vision, Robotics, Expert Systems

What are the two main types of supervised learning problems?::Classification (predict categorical label) and Regression (predict continuous value)

What is the difference between classification and regression?
?
Classification predicts a categorical label (spam/not spam, cat/dog). Regression predicts a continuous value (house price, stock forecast).

What are "features" in ML?::Measurable properties or characteristics of the data that serve as input to the model (e.g., size, location, bedrooms for house price prediction)

What are "labels" in ML?::The known outcomes or target variables — the "correct answers" the model aims to predict

What is the difference between prediction and inference?
?
Prediction focuses on generating actionable outputs (classify email, forecast price). Inference is broader — it includes understanding structure, estimating parameters, and explaining relationships between variables.

What is overfitting?::When a model learns the training data too well (including noise/outliers), leading to poor generalization on new data — the model memorizes instead of learning patterns

What is underfitting?::When a model is too simple to capture underlying patterns — poor performance on both training and new data

What does cross-validation do?::Splits data into multiple folds, trains on different combinations, validates on the remaining fold — reduces overfitting and gives a more reliable performance estimate

What is the difference between L1 and L2 regularization?
?
L1 adds a penalty equal to the absolute value of coefficients (drives some to zero, acts as feature selection). L2 adds a penalty equal to the square of coefficients (shrinks all but keeps all features).

What are the four common evaluation metrics for supervised learning?
?
Accuracy (correct predictions / total), Precision (true positives / predicted positives), Recall (true positives / actual positives), F1-Score (harmonic mean of precision and recall).

What is generalization in ML?::The model's ability to accurately predict outcomes for new, unseen data not used during training

What is linear regression?::A supervised learning algorithm that predicts a continuous target variable by finding the best-fitting straight line through the data

What is the equation for simple linear regression?::y = mx + c, where y = predicted target, x = predictor, m = slope, c = y-intercept

What is the equation for multiple linear regression?::y = b0 + b1·x1 + b2·x2 + ... + bn·xn, where b0 = intercept and b1...bn = coefficients for each predictor

What is Ordinary Least Squares (OLS)?
?
A method for finding optimal coefficients in linear regression. Steps: (1) calculate residuals (actual - predicted), (2) square each residual, (3) sum them into Residual Sum of Squares (RSS), (4) adjust coefficients to minimize RSS.

What are the four assumptions of linear regression?
?
Linearity (linear relationship between predictors and target), Independence (observations are independent), Homoscedasticity (constant error variance across predictor levels), Normality (errors are normally distributed).

What is homoscedasticity?::The variance of errors is constant across all levels of the predictor variables — the spread of residuals is roughly uniform across predicted values

What is the Residual Sum of Squares (RSS)?::The sum of all squared residuals (differences between actual and predicted values) — the single value OLS aims to minimize
