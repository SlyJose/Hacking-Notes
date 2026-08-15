
**Model Context Protocol (MCP)** is a structured framework designed to:

- Standardize the communication context between user inputs, system prompts, memory, tools, and outputs.
- Control and analyze the _prompt flow_, _tool usage_, and _context manipulation_.
- Serve as an **attack surface mapping tool** and **simulation controller** for red teamers using LLMs.

MCP is essential for red teams because it allows **surgical manipulation** of the model pipeline.

#### 🖊️ MCP Components

| Component    | Description                                           | Attack Relevance                          |
| ------------ | ----------------------------------------------------- | ----------------------------------------- |
| SystemPrompt | Sets behavioral anchor for the model                  | Target for jailbreak or override          |
| UserPrompt   | Main user input                                       | Injection point for chained payloads      |
| Tools        | List of available plugins or functions                | Abuse surface for tool exploitation       |
| Memory       | Retained knowledge between sessions or steps          | Target for poisoning or leakage           |
| EmbeddingCtx | Knowledge retrieval context (e.g. vector store / RAG) | Entry point for indirect prompt injection |
| Intermediate | Results and messages from internal steps              | Leak path for sensitive operations        |

How can Red Team's use MCP to their advantage?: [[MCP in Red Teaming]]




#### 📔 B


####  📗 C


#### ⚠ Opsec




### Properties
---
📆 created   {{16-08-2025}} 23:03
🏷️ tags: #offensiveaisecurity #ai

---

