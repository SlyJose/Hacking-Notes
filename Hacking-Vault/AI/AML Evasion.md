
**Goal**: Subvert a trained model at inference time using adversarial samples.

#### [](https://redteamleaders.coursestack.com/courses/4cc576dd-934b-4289-a100-8a233fe07ef2/take/13-adversarial-ml-techniques-for-offensive-operations#user-content-white-box-attack)White-box Attack:

- Attacker has access to model gradients or architecture.
- Uses techniques like [[Fast Gradient Sign Method FGSM]] or [[Projected Gradient Descent PGD]]

#### [](https://redteamleaders.coursestack.com/courses/4cc576dd-934b-4289-a100-8a233fe07ef2/take/13-adversarial-ml-techniques-for-offensive-operations#user-content-black-box-attack)Black-box Attack:

- Attacker only interacts via API.
- Uses model-agnostic perturbations or transferability.

```python
from art.attacks.evasion import FastGradientMethod
from art.estimators.classification import TensorFlowClassifier

adv_crafter = FastGradientMethod(classifier=model, eps=0.2)
x_test_adv = adv_crafter.generate(x=x_test)
```

> Evasion attacks are common against image classifiers, malware detection systems, and spam filters.


#### 🖊️ A


#### 📔 B


####  📗 C


#### ⚠ Opsec




### Properties
---
📆 created   {{15-08-2025}} 20:10
🏷️ tags: #offensiveaisecurity #ai

---

