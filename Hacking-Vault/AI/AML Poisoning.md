
**Goal**: Corrupt model performance by inserting malicious data into the training pipeline.

- **Availability Attack**: Causes model to fail generally.
- **Integrity Attack**: Subtle, targeted misbehavior (e.g., a backdoor trigger).

#### [](https://redteamleaders.coursestack.com/courses/4cc576dd-934b-4289-a100-8a233fe07ef2/take/13-adversarial-ml-techniques-for-offensive-operations#user-content-example)Example:

An attacker adds 100 samples with `"malicious"` label falsely marked as `"benign"` in a crowdsourced dataset, degrading accuracy over time.

#### [](https://redteamleaders.coursestack.com/courses/4cc576dd-934b-4289-a100-8a233fe07ef2/take/13-adversarial-ml-techniques-for-offensive-operations#user-content-tools)Tools:

- **[[PoisonFrog]]**
- **[[Clean-Label Backdoor Attack]]**
- **[[TrojanNN]]**

> These attacks are especially dangerous in open-source or collaborative fine-tuning (LoRA, PEFT).

#### 🖊️ A


#### 📔 B


####  📗 C


#### ⚠ Opsec




### Properties
---
📆 created   {{15-08-2025}} 20:14
🏷️ tags: #offensiveaisecurity #ai

---

