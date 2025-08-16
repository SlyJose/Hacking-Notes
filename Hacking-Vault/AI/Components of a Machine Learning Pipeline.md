
These components are part of [[ML (Machine Learning) in Security]] and can be attacked specifically:

| Component                   | Security Concern                              | Attack Vectors                               |
| --------------------------- | --------------------------------------------- | -------------------------------------------- |
| **[[Data Collection]]**     | Unverified data sources                       | Poisoning, mislabeling, distributional shift |
| **[[Feature Engineering]]** | Sensitive features may leak private data      | Feature inference                            |
| **[[Model Training]]**      | Training code or libraries can be compromised | Supply chain attack, backdoor injection      |
| **[[Model Evaluation]]**    | Evaluation on synthetic metrics can be gamed  | Metric manipulation                          |
| **[[Inference]]**           | Inference APIs become the attack interface    | Model extraction, [[Prompt Injection]]       |

> **Example**: If an adversary can inject mislabeled data into a spam detection training set, they could train the model to misclassify phishing emails as benign.

#### 🚀 - A
---
1. A
2. B
3. C

---
#### 📦 - B
--- 

#### 🖊️ - C


⚠ Alert 1
⚠ Alert 2
⚠ Alert 3


--- 

 1️⃣ A
 2️⃣ B
 
--- 

❗C


### Properties
---
📆 created   {{12-08-2025}} 15:00
🏷️ tags: #offensiveaisecurity #ai

---
