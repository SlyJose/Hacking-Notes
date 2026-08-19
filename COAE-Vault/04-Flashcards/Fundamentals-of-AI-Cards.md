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

What is logistic regression used for?::Binary classification — predicts which of two classes an input belongs to (0 or 1, spam or not spam)

What is the sigmoid function and what does it output?::P(x) = 1 / (1 + e^-z). Maps any input to a value between 0 and 1, representing the probability of belonging to the positive class.

What is `z` in the logistic regression sigmoid function?::The linear combination of features: z = m1·x1 + m2·x2 + ... + mn·xn + c — identical in form to the linear regression equation

What is a decision boundary in logistic regression?::The threshold that separates classes. In 2D it's a line; in higher dimensions it's a hyperplane. Inputs above threshold → positive class, below → negative class.

What is a hyperplane?::A flat subspace one dimension below the ambient space that acts as a decision boundary — a line in 2D, a flat plane in 3D, and the equivalent concept in higher dimensions

What is the default classification threshold in logistic regression and how does adjusting it affect outcomes?
?
Default is 0.5. Raising it reduces false positives but increases false negatives (stricter). Lowering it catches more positives but increases false positives (permissive). Trade-off depends on cost of each error type.

What are the four data assumptions of logistic regression?
?
(1) Binary outcome (only two classes), (2) Linearity of log-odds (linear relationship between features and log(p/1-p)), (3) No/little multicollinearity (correlated predictors distort coefficients), (4) Large sample size (needed for reliable parameter estimation).

What is multicollinearity and why is it a problem for logistic regression?::When predictor variables are highly correlated with each other. Makes it hard to isolate the individual effect of each predictor, producing unstable and misleading coefficients.

How does logistic regression differ from linear regression?
?
Linear regression outputs a continuous value using an identity function. Logistic regression outputs a probability [0,1] using the sigmoid function and makes class predictions via a threshold. Both share the same linear combination z as input.

What are the three components of a decision tree?::Root node (full dataset, first split), Internal nodes (feature splits with branches), Leaf nodes (final class prediction or regression value)

What does Gini impurity measure and what is its formula?
?
Probability of misclassifying a randomly chosen element. Lower = purer subset.
Gini(S) = 1 - Σ(pi²), where pi is the proportion of class i in the set.

What does entropy measure in the context of decision trees?::Disorder or uncertainty in a set. Lower entropy = more homogeneous. Formula: Entropy(S) = -Σ(pi · log2(pi))

What is information gain and how is it used?::The reduction in entropy achieved by splitting on a feature. The feature with the highest information gain is chosen as the split point. Formula: InfoGain(S, A) = Entropy(S) - Σ((|Sv|/|S|) · Entropy(Sv))

What are the three stopping criteria for growing a decision tree?::Maximum depth reached, minimum number of data points in a node, or all data points in a node belong to the same class (pure node)

What data assumptions do decision trees require?::Minimal — no linearity assumption, no normality assumption, and relatively robust to outliers since splits are based on feature values not distance calculations

How do decision trees differ from logistic regression in terms of assumptions?::Decision trees require no linearity assumption and handle non-linear feature relationships; logistic regression assumes a linear relationship between features and the log-odds of the outcome

What is the "naive" assumption in Naive Bayes?::That all features are conditionally independent given the class label — i.e., the presence of one feature does not affect any other feature's probability, given the class

Write Bayes' theorem and define each term.
?
P(A|B) = (P(B|A) * P(A)) / P(B)
P(A|B) = posterior (probability of A given B), P(B|A) = likelihood, P(A) = prior, P(B) = evidence/normalizing constant

What are the four steps of Naive Bayes classification?
?
(1) Calculate prior P(class) for each class. (2) Calculate likelihood P(feature|class) for each feature. (3) Compute posterior P(class|features) ∝ P(class) * Π P(feature_i|class). (4) Predict the class with the highest posterior.

What are the three types of Naive Bayes and when is each used?
?
Gaussian — continuous features assumed normally distributed. Multinomial — discrete count features (e.g., word frequency in text). Bernoulli — binary features (e.g., word present/absent in document).

Why can a highly accurate test still give a low posterior probability of disease?::Because the prior probability (disease prevalence) is very low. Bayes' theorem multiplies the likelihood by the prior — a rare event remains unlikely even after a positive test result.

How does Naive Bayes compare to Logistic Regression for classification?::Both output class probabilities, but Naive Bayes assumes feature independence and is more efficient; logistic regression models feature interactions and assumes linearity of log-odds. Naive Bayes often wins on text data; logistic regression on structured data.
