
Core concept: [[Adversarial Machine Learning AML]]

#### 🖊️ AML Attack Categories

| Category                | Description                             | Example                                                     |
| ----------------------- | --------------------------------------- | ----------------------------------------------------------- |
| **[[AML Evasion]]**     | Creating inputs that are misclassified  | A malware sample modified slightly to bypass detection      |
| **[[AML Poisoning]]**   | Injecting crafted data during training  | Adding mislabeled benign samples to degrade spam classifier |
| **[[AML Inference]]**   | Gaining knowledge about training data   | Membership inference: “Was this user’s data used?”          |
| **[[AML Extraction]]**  | Stealing model functionality or weights | Reconstructing a fraud model via output predictions         |
| **[[AML Backdooring]]** | Inserting logic triggers                | A facial recognition model unlocked by a specific pattern   |


#### 📔 AML in Large Language Models (LLMs)

#### [](https://redteamleaders.coursestack.com/courses/4cc576dd-934b-4289-a100-8a233fe07ef2/take/13-adversarial-ml-techniques-for-offensive-operations#user-content-red-team-aml-scenarios)Red Team AML Scenarios:

|Scenario|Technique|
|---|---|
|Jailbreaking Guardrails|Evasion via indirect prompt chaining|
|Indirect Prompt Leaks|Prompt injection via retrievers or external memory|
|Hidden Commands in Fine-tune|Backdoors in LoRA-trained models|
|LLM Extraction|Using structured API queries to reconstruct behavior|


####  📗 Tools and Frameworks for AML Red Teaming

| Toolkit                                      | Description                                                |
| -------------------------------------------- | ---------------------------------------------------------- |
| **[[Adversarial Robustness Toolbox (ART)]]** | IBM’s toolkit for AML (evasion, poisoning, inference)      |
| **[[Foolbox]]**                              | Python-based evasion testing on PyTorch, TF                |
| **[[TextAttack]]**                           | NLP-focused AML (e.g., LLM adversarial prompts)            |
| [[PromptInject]]<br>[[Gauntlet]]             | LLM prompt manipulation frameworks                         |
| **[[BackdoorBox]]**                          | Open-source framework for backdooring and defending models |





### Properties
---
📆 created   {{15-08-2025}} 20:07
🏷️ tags: #offensiveaisecurity #ai

---

