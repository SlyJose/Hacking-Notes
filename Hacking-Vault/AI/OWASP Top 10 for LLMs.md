The OWASP Top 10 for LLMs is a threat modeling framework designed to guide secure implementation of LLM-powered systems. From a red team perspective, it becomes a **tactical map of abuse cases** and **exploitation techniques**.

#### 🚀 - OWASP Top 10 2024
---
#### LLM01: [[Prompt Injection]] - MITRE T1485 – Prompt Injection
Bypass alignment, coerce output
#### LLM02: [[Insecure Output Handling]]
Trigger system action through response
#### LLM03: [[Training Data Poisoning]] - MITRE T1495 – Poison Model Artifact
Introduce backdoors or triggers
#### LLM04: [[Model Denial of Service]]
Prompt flooding, over-computation
#### LLM05: [[Supply Chain Vulnerabilities]]
Attack 3rd-party models/plugins
#### LLM06: [[Sensitive Information Disclosure]]
Extract PII, internal data
#### LLM07: [[Insecure Plugin Design]] - MITRE T1431 – Abuse of Model Interfaces
Abuse function-calling APIs
#### LLM08: [[Excessive Agency]]
Trigger unintended tool usage
#### LLM09: [[Overreliance]] - MITRE T1429 – Output Manipulation
Encourage unsafe LLM-led automation
#### LLM10: [[Model Theft]]
Perform model extraction attacks

---
#### 📦 - MCP vs OWASP LLM
--- 
##### 1. LLM01 - Prompt Injection

- **MCP Field:** `UserPrompt`, `EmbeddingCtx`
    
- **Offensive Advantage:** `Controlled prompt fuzzing and override testing`
    
- **Explanation:** This category addresses attacks where a malicious prompt is used to override the model's instructions or intent. The MCP framework allows a red teamer to specifically inject payloads into the `UserPrompt` (the direct query) or the `EmbeddingCtx` (retrieved knowledge from a RAG system), which are both common vectors for prompt injection attacks.
    
##### 2. LLM07 - Insecure Plugins

- **MCP Field:** `Tools`
    
- **Offensive Advantage:** `Track and trigger plugin-based attacks`
    
- **Explanation:** This category covers vulnerabilities in how an LLM interacts with external tools or plugins. By controlling the `Tools` field in the MCP profile, a red teamer can simulate scenarios where the LLM is tricked into using a tool in an unauthorized or malicious way.
    
##### 3. LLM06 - Data Disclosure

- **MCP Field:** `Memory`, `Intermediate`
    
- **Offensive Advantage:** `Contextual leak simulation`
    
- **Explanation:** This vulnerability involves an LLM leaking sensitive data it shouldn't. Using MCP, a red teamer can test this by pre-populating the `Memory` field with sensitive data (e.g., "user balance: $10,000") and then crafting a prompt that attempts to make the LLM reveal that information. The `Intermediate` field represents a path where sensitive internal results could be leaked.
    
##### 4. LLM04 - Denial of Service (DoS)

- **MCP Field:** `SystemPrompt`, `UserPrompt`
    
- **Offensive Advantage:** `Prompt flooding and token abuse scenarios`
    
- **Explanation:** DoS attacks on LLMs often involve forcing the model to process an excessive number of tokens, which can lead to high costs or resource exhaustion. The MCP framework allows a red teamer to craft and test these attacks by creating extremely long or complex payloads in the `SystemPrompt` or `UserPrompt` fields to see if they can disrupt the service.



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
📆 created   {{16-08-2025}} 22:22
🏷️ tags: #offensiveaisecurity #ai

---
