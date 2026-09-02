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

What is the goal of an SVM?::Find the optimal hyperplane that maximally separates classes by maximizing the margin — the distance between the hyperplane and the nearest data points (support vectors) of each class.

What are support vectors?::The data points closest to the decision hyperplane. They define and "support" the hyperplane and the margin — removing other points doesn't change the hyperplane.

Write the SVM hyperplane equation and define each term.
?
w * x + b = 0. w = weight vector (perpendicular to hyperplane), x = input feature vector, b = bias term (shifts hyperplane from origin).

Write the SVM optimization objective.
?
Minimize: 1/2 * ||w||²
Subject to: yi * (w·xi + b) >= 1 for all i
Minimizing ||w||² maximizes the margin; the constraint ensures correct classification with margin ≥ 1.

What is the kernel trick in SVMs?::A technique that maps data into a higher-dimensional space using a kernel function, where it becomes linearly separable. The resulting linear hyperplane maps back to a non-linear boundary in the original space.

Name the three common SVM kernel functions and their use cases.
?
Polynomial — adds polynomial terms (x², x³), moderate non-linearity. RBF/Gaussian — Gaussian mapping, most versatile, handles complex patterns (default choice). Sigmoid — sigmoid-shaped boundary, similar to logistic regression.

What are SVMs' data assumptions?::Minimal — no distributional assumptions, effective in high-dimensional spaces (features > data points), and relatively robust to outliers since optimization focuses on margin not individual points.

What are the three categories of unsupervised learning problems?::Clustering (group similar data points), Dimensionality Reduction (reduce features while preserving information), Anomaly Detection (identify points that deviate from the norm)

What is the key difference between supervised and unsupervised learning?::Supervised learning uses labeled data with known outcomes; unsupervised learning uses unlabeled data and discovers structure without predefined answers.

What are the three main similarity measures used in unsupervised learning and when is each used?
?
Euclidean distance — straight-line distance, general clustering. Cosine similarity — angle between vectors, best for text/NLP where direction matters more than magnitude. Manhattan distance — sum of absolute differences, useful in high-dimensional or grid-like spaces.

What is the curse of dimensionality?::As the number of features grows, data becomes sparse and distances between points lose meaning, degrading algorithm performance. Dimensionality reduction targets the intrinsic (true underlying) dimensionality to counter this.

What are cohesion and separation in cluster validity?::Cohesion measures how similar points are within a cluster (higher = more compact). Separation measures how different clusters are from each other (higher = more distinct). Both should be high for good clustering.

What is the difference between an anomaly and an outlier?::An anomaly deviates significantly from expected patterns — operationally tied to fraud, attacks, or errors. An outlier is a broader term for any data point far from the majority — may be an error, unusual observation, or interesting pattern.

Why is feature scaling essential before distance-based unsupervised algorithms?::Unscaled features with larger ranges dominate distance calculations. Min-Max scaling (fixed range) or standardization (zero mean, unit variance) ensures all features contribute equally.

What are the four steps of the K-means algorithm?
?
(1) Initialization — pick K random centroids. (2) Assignment — assign each point to nearest centroid by Euclidean distance. (3) Update — recalculate centroids as mean of assigned points. (4) Iterate steps 2–3 until centroids stabilize or max iterations reached.

What is WCSS and how is it used in the elbow method?::Within-Cluster Sum of Squares — total variance within each cluster. Plot WCSS vs K; the "elbow" where WCSS stops decreasing sharply is the recommended K. Beyond the elbow, complexity increases without significant compactness gain.

What does a silhouette score of -1, 0, and 1 each mean?
?
1 = point well-matched to its cluster, poorly matched to others. 0 = point near the decision boundary between clusters. -1 = point likely assigned to the wrong cluster.

What are the three data assumptions K-means makes?
?
(1) Clusters are spherical and roughly similar in size. (2) Features must be on the same scale (standardize before applying). (3) Sensitive to outliers — they distort centroids and corrupt cluster assignments.

What is the Euclidean distance formula used in K-means?::d(x, y) = sqrt(Σ(xi - yi)²) — the straight-line distance between two points across all feature dimensions.

What is PCA and what problem does it solve?::Principal Component Analysis — a dimensionality reduction technique that transforms high-dimensional data into a lower-dimensional representation while preserving maximum variance. Solves the curse of dimensionality and enables visualization, noise reduction, and faster ML.

What are the six steps of PCA?
?
(1) Standardize data (zero mean, unit variance). (2) Compute the covariance matrix. (3) Compute eigenvectors and eigenvalues of the covariance matrix. (4) Sort eigenvectors by descending eigenvalue. (5) Select top k eigenvectors. (6) Transform data: Y = X * V.

What is an eigenvector and eigenvalue in plain terms?
?
Eigenvector: a direction that remains unchanged when a linear transformation (matrix multiplication) is applied — it only stretches or shrinks. Eigenvalue: the scalar factor by which the eigenvector stretches/shrinks. In PCA, eigenvectors are the principal component directions; eigenvalues represent how much variance each captures.

Write the PCA eigenvalue equation and define each term.
?
C * v = λ * v. C = covariance matrix of standardized data, v = eigenvector (principal component direction), λ = eigenvalue (variance explained by that component). Larger λ → more important component.

How do you choose the number of PCA components to keep?::Plot explained variance ratio vs number of components and choose k where cumulative variance reaches a target threshold (commonly 95%). More components = more information, less compression.

What are PCA's three data assumptions?::Linearity (assumes linear feature relationships), Correlation (works best when features are correlated), Scale sensitivity (larger-scale features dominate — always standardize first).

What are the three types of anomalies?
?
Point — single data point deviates significantly (e.g., spike in network traffic). Contextual — anomalous within a specific context but not in isolation (e.g., 30°C in winter). Collective — a group of points collectively deviate even if individual points look normal (e.g., coordinated login attempts from many IPs).

What are the three categories of anomaly detection techniques?::Statistical (assumes Gaussian distribution, uses z-score/boxplots), Clustering-based (outliers don't belong to any cluster), ML-based (learns normal patterns — One-Class SVM, Isolation Forest, LOF).

How does One-Class SVM detect anomalies?::It learns a boundary enclosing all normal training data using kernel functions. Points outside the boundary at inference time are flagged as anomalies. Only trained on normal data — no anomaly labels needed.

How does Isolation Forest work and what is its anomaly score formula?
?
Randomly partitions data into isolation trees by picking random features and split values. Anomalies need fewer splits to isolate → shorter path lengths.
score(x) = 2^(-E(h(x)) / c(n)). Score near 1 = anomaly; near 0.5 = normal.

How does Local Outlier Factor (LOF) detect anomalies?::Compares the local density of a point to its k nearest neighbors. Points with much lower local density than neighbors get high LOF scores (>> 1) and are flagged as outliers. Effective when cluster density varies across the dataset.

What is the LOF score formula and what does each symbol mean?
?
LOF(p) = (Σ lrd(o) / k) / lrd(p). lrd(p) = local reachability density of p. lrd(o) = local reachability density of neighbor o. k = number of nearest neighbors. LOF >> 1 means p is much less dense than its neighbors → outlier.

Compare One-Class SVM, Isolation Forest, and LOF on scalability and best use case.
?
One-Class SVM — moderate scalability, best for high-dimensional data. Isolation Forest — high scalability, best for large datasets. LOF — low scalability (expensive), best when cluster density varies significantly.

What is the difference between model-based and model-free RL?::Model-based — agent builds an internal model of the environment to plan actions (map of the maze). Model-free — agent learns directly from experience with no environment model (navigating without a map, pure trial and error).

Define the six core RL concepts: agent, environment, state, action, reward, policy.
?
Agent — learner/decision-maker. Environment — everything outside the agent; responds to actions. State — current snapshot of the environment. Action — decision the agent makes that transitions the environment. Reward — scalar feedback (positive reinforces, negative discourages). Policy — strategy mapping states to actions; goal is to maximize cumulative reward.

What are the two types of value functions in RL?
?
State-value V(s) — expected cumulative reward from state s under a given policy. Action-value Q(s,a) — expected cumulative reward from taking action a in state s, then following policy. Q-values are the basis of Q-learning.

What is the discount factor (γ) in RL and what do extreme values mean?
?
γ controls how much future rewards are valued. γ=0 → only immediate reward matters (myopic). γ=1 → all future rewards weighted equally (far-sighted). Typical range: 0.9–0.99.

What is the difference between episodic and continuous RL tasks?::Episodic — interaction ends at a terminal state (e.g., winning a game, reaching maze exit). Continuous — no explicit end, runs indefinitely (e.g., robot arm control, traffic management).

What is the difference between a deterministic and stochastic policy?::Deterministic — always selects the same action in a given state. Stochastic — selects actions with certain probabilities, allowing exploration.

What is a Q-value?::The expected cumulative reward an agent obtains by taking a specific action in a given state and then following the optimal policy afterward. Stored in the Q-table for every state-action pair.

Write the Q-learning update rule and define each term.
?
Q(s, a) = Q(s, a) + α * [r + γ * max(Q(s', a')) - Q(s, a)]
α = learning rate, r = immediate reward, γ = discount factor, max(Q(s', a')) = best Q-value from next state s'. The update pulls Q(s,a) toward the Bellman target r + γ * max(Q(s', a')).

What is the Q-table and how is it structured?::A lookup table storing Q-values for every state-action pair. Rows = states, Columns = actions, Cells = Q-values. Initialized to zero and updated iteratively as the agent learns.

What are the six steps of the Q-learning algorithm?
?
(1) Initialize Q-table. (2) Choose action via exploration-exploitation strategy. (3) Take action, observe new state s' and reward r. (4) Update Q-value using Bellman equation. (5) Update current state to s'. (6) Repeat until Q-values converge or stopping condition met.

What is the epsilon-greedy strategy and how does ε affect behavior?
?
With probability ε → take a random action (explore). With probability 1-ε → take the action with the highest Q-value (exploit). High ε = heavy exploration; low ε = heavy exploitation. Common practice: start high and decay ε over training.

What are the two data assumptions Q-learning makes?::Markov property (next state depends only on current state and action, not history) and stationary environment (transition probabilities and rewards don't change over time).
