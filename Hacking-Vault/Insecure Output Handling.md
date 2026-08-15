Neglecting to validate LLM outputs may lead to downstream security exploits, including code execution that compromises systems and exposes data.

- **Goal**: Deliver outputs that execute undesired system actions.
- **Example**:
    
    - Outputting shell commands in JSON that trigger external plugins:
    
    ```json
    { "action": "run_shell", "cmd": "rm -rf /" }  
    ```
    
- **Abuse Surface**: Chatbots connected to automation pipelines or agents.

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
📆 created   {{16-08-2025}} 22:29
🏷️ tags: #offensiveaisecurity #ai

---
