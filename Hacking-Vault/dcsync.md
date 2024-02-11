DCSync is a technique used particularly in the context of [[Active Directory]] environments, to retrieve password data from a [[Domain Controller]] without requiring elevated privileges or directly interacting with the targeted user's account. 

DCSync is a feature within the [[Mimikatz]] toolkit.


## 🖊️ How it works

1. **Domain Controller Interaction**: Mimikatz uses DCSync to interact with the domain controller's replication protocol, which is used to synchronize data between domain controllers in an Active Directory environment.
    
2. **Password Retrieval**: With DCSync, an attacker can request password data for a specified user account from the domain controller. This includes the password hash ([[NetNTLM]] or [[Kerberos]]) and other relevant information associated with the account.
    
3. **Low-Level Access**: Unlike traditional methods of password retrieval, such as credential dumping or brute force attacks, DCSync does not require elevated privileges or direct access to the target user's account. Instead, it leverages the domain controller's replication mechanism, making it a stealthy and effective technique for obtaining password data.


### Properties
---
📆 created   {{10-02-2024}} 21:57
🏷️ tags: #activedirectory #redteam 

---

