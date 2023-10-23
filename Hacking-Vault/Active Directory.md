
A Windows domain is a group of users and computers under the administration of a given business. 

The main idea behind a domain is to centralize the administration of common components of a Windows computer network in a single repository called Active Directory (AD). 

The server that runs the Active Directory services is known as a Domain Controller (DC).

# 🚀 - Active Directory components
---
1. [[Users]]
2. [[Computers]]
3. [[Groups]]

---

# 📜 Active Directory containers

These are default [[Groups]] created in the [[Active Directory]] environment:

- Builtin : default groups available to any Windows host
- [[Computers]]
- [[Domain Controller]]s
- [[Users]]
- [[Service Accounts]]


--- 
# 📦Active Directory Authentication

All credentials are stored in the [[Domain Controller]]. Whenever a user tries to authenticate to a service using domain credentials, the service will need to ask the [[Domain Controller]] to verify if they are correct. 

Two protocols can be used for network authentication in windows domains:

- [[Kerberos]]: Used by any recent version of Windows. This is the default protocol in any recent domain.
- [[NetNTLM]]: Legacy authentication protocol kept for compatibility purposes.

While NetNTLM should be considered obsolete, most networks will have both protocols enabled.

## 1️⃣ -> Project Deliverables
- Outline what deliverables are to be produced by the project 

## 2️⃣ -> Benefits
- Describe the benefits from the project

--- 
# ❗❗ -> Risks
- Describe any known risks or possible risks here

### Properties
---
📆 created   {{22-10-2023}} 18:37
🏷️ tags: #activedirectory
---
