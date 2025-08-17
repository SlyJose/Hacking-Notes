These are entry points that an adversary can target:

#### 🚀 - Prompt Interfaces
---
- Web UIs, chatbots, CLI agents
- Vulnerable to [[Prompt Injection]], _[[Context Overflow]]_, _[[AI Jailbreaks]]_ 
---
#### 📦 - Plugins and Tools
--- 
- LLM-activated browser, code execution, file access tools
- Vulnerable to _[[Toolchain Abuse]]_, _[[AI Arbitrary File Execution]]_, _[[AI Credential Exfiltration]]_
#### 🖊️ - Contextual Memory session state
---
- Vector embedding, history files, external memory (e.g., Redis)
- Vulnerable to _[[Persistent Poisoning]]_, _[[Vector Clustering Attacks]]_
#### 🚀 - Retrieval Augmented Generation
---
- Search systems, internal KBs
- Attack surface includes _[[Knowledge Base Poisoning]]_, _[[Embedding Attacks]]_, _[[Prompt-Data Injection]]_
---
#### 📦 - Model API's and Agents
--- 
- Exposed OpenAI, Anthropic, HuggingFace, or custom endpoints
- Vulnerable to _[[Rate-Based Extraction]]_, _[[Jailbreak-as-a-Service]]_, _[[Billing Abuse]]_
#### 🖊️ - Training and Fine Tunning Pipelines
- Data preprocessing, prompt datasets, labeling systems
- Vulnerable to _[[Training Data Poisoning]]_, _[[AI Backdoor Injection]]_, _[[LoRA Vector Hijacking]]_


### Properties
---
📆 created   {{15-08-2025}} 19:42
🏷️ tags: #offensiveaisecurity #ai

---
