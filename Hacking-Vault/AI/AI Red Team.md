
#### 🖊️ Red Team vs AI Red Team

|Traditional Red Teaming|AI Red Teaming|
|---|---|
|Focuses on systems and network exploitation|Focuses on model behavior and logic abuse|
|Targets deterministic logic|Targets statistical and emergent behavior|
|Relies on CVEs and misconfigurations|Relies on adversarial inputs, model flaws, poisoning|

> AI systems cannot be "patched" in the traditional sense. Offensive testing must anticipate **novel emergent behavior** and exploit implicit design weaknesses.

#### 📔  **Goals of AI Red Teaming**

1. **Expose and exploit unsafe behavior** in ML systems.
2. **Model attacker strategies** such as [[Prompt Injection]], [[Logic corruption]], or [[Model Theft]].
3. **Validate system boundaries** under adversarial conditions.
4. **Simulate abuse and misuse scenarios**, not just technical vulnerabilities.


####  📗 **Phases of an AI Red Team Operation**

| Phase                      | Description                                                    |
| -------------------------- | -------------------------------------------------------------- |
| **Planning**               | Identify model purpose, threat landscape, abuse cases          |
| **Reconnaissance**         | Study model architecture, API, inputs, defenses                |
| **Threat Modeling**        | Build attacker persona: insider, external, malicious developer |
| **Adversarial Simulation** | Generate targeted adversarial inputs (e.g., jailbreaks)        |
| **Post-Exploitation**      | Trigger side-effects, unauthorized outputs, or unsafe actions  |
| **Reporting**              | Document system responses, gaps, recommendations               |


#### ⚠ Methodologies and Frameworks

[[OWASP Top 10 for LLMs]]
[[MITRE ATLAS]]
[[Anthropic Constitutional AI Red Teaming]]
[[NIST AI RMF]]


#### 📔  Categories of AI Red Team Tests
| Category                      | Example Techniques                              |
| ----------------------------- | ----------------------------------------------- |
| **Prompt Engineering Abuse**  | Indirect prompt injection, persona switching    |
| **Data Supply Chain Attacks** | Dataset poisoning, model pretraining corruption |
| **Inference API Exploits**    | Extraction, over-query, rate abuse              |
| **Access Control Failures**   | Plugin misuse, sandbox breakout                 |
| **Misalignment Detection**    | Covert goal changes, contradictory outputs      |

#### 🖊️ [[MCP in Red Teaming]]

#### ⚠ [[Tools for AI Red Team Operations]]




### Properties
---
📆 created   {{15-08-2025}} 20:29
🏷️ tags: #offensiveaisecurity #ai

---

